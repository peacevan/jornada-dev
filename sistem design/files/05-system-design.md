# System Design — SmartList

> Como escalar o SmartList de 0 para 50.000 usuários em Salvador.

Este documento apresenta a evolução arquitetural do SmartList em três estágios.
Cada estágio é motivado por um gargalo real — não adicionamos complexidade antes de precisar dela.

---

## Premissas

- O sistema tem dois padrões de acesso bem distintos: **muita leitura** (consultar ranking de preços) e **escrita colaborativa** (registrar preços encontrados no mercado)
- O ranking por bairro é a query mais cara — agrega dados de muitos usuários
- Picos de acesso acontecem nos fins de semana, quando as pessoas vão ao mercado
- O público é Salvador e região — geograficamente concentrado

---

## Estágio 1 — Fase 2 (~500 usuários)

Arquitetura simples e deliberada. Não é ingenuidade — é decisão consciente de não over-engineerizar o MVP.

![Diagrama estágio 1](./images/system-design-fase2.svg)

### Componentes

| Componente | Tecnologia | Responsabilidade |
|---|---|---|
| Frontend | React · Netlify CDN | Interface do usuário |
| Backend | Java 21 · Spring Boot | API REST · regras de negócio |
| Banco | PostgreSQL | Persistência de produtos, preços e estabelecimentos |

### Camadas internas do Spring Boot

```
Controller → Service → Repository → PostgreSQL
```

- `Controller` recebe e valida as requisições REST
- `Service` aplica as regras de negócio (cálculo de ranking, validação de preços)
- `Repository` abstrai o acesso ao banco via Spring Data JPA

### Modelo de dados central

```
Produto              Estabelecimento        RegistroPreco
────────             ───────────────        ─────────────
id (UUID)            id (UUID)              id (UUID)
nome                 nome                   produto_id (FK)
categoria            endereco               estabelecimento_id (FK)
unidade              bairro                 preco
codigo_barras        latitude               data_registro
                     longitude              usuario_id (FK)
                                            fonte (manual/ocr/voz)
```

### Gargalo esperado

A query de ranking agrega preços de todos os usuários por bairro. Com volume, ela fica cara e lenta. **Solução: cache — próximo estágio.**

---

## Estágio 2 — Cache com Redis (~5.000 usuários)

Adicionamos Redis para cachear o ranking por bairro. A maioria das leituras vira cache hit — o banco respira.

![Diagrama estágio 2](./images/system-design-5k.svg)

### O que muda

| Antes | Depois |
|---|---|
| Toda leitura vai ao banco | Cache hit vai ao Redis (memória) |
| Ranking recalculado a cada request | Ranking recalculado a cada 15 min |
| Banco sobrecarregado em pico | Banco protegido pelo cache |

### Estratégia: Cache-aside

```
1. Request chega ao Spring Boot
2. Verifica Redis — cache hit? Retorna direto.
3. Cache miss? Vai ao PostgreSQL, calcula ranking
4. Salva resultado no Redis com TTL de 15 min
5. Retorna para o cliente
```

### Invalidação de cache

Quando um novo preço é registrado no bairro X, o cache do ranking do bairro X é invalidado imediatamente — garantindo que o próximo acesso recalcula com dado fresco.

### Gargalo esperado

Com muitos usuários registrando preços simultaneamente, os inserts começam a competir no banco. **Solução: desacoplar escrita com fila — próximo estágio.**

---

## Estágio 3 — Kafka + múltiplas instâncias (~50.000 usuários)

Adicionamos fila de mensagens para processar registros de forma assíncrona, load balancer para distribuir carga e read replica para separar leitura de escrita no banco.

![Diagrama estágio 3](./images/system-design-50k.svg)

### O que muda

| Componente | Motivo |
|---|---|
| Load balancer (Nginx / AWS ALB) | Distribui requisições entre instâncias |
| Múltiplas instâncias Spring Boot | Escala horizontal — mais poder de processamento |
| Kafka | Desacopla registro de preço da persistência |
| PostgreSQL read replica | Separa carga de leitura (ranking) da escrita (preços) |

### Fluxo de registro de preço com Kafka

```
Usuário registra preço
    ↓
Spring Boot publica evento no Kafka
    ↓ (assíncrono)
Price Consumer consome o evento
    ↓
Persiste no PostgreSQL (primary)
    ↓
Invalida cache Redis do bairro
```

### Por que Virtual Threads aqui?

Java 21 Virtual Threads permitem que cada instância Spring Boot trate milhares de requisições concorrentes sem criar uma thread do SO por requisição. Essencial para o padrão de acesso do SmartList — muitas conexões simultâneas de curta duração.

### Read replica

- **Primary** recebe apenas escritas (novos preços registrados via Kafka consumer)
- **Read replica** serve apenas leituras (consultas de ranking, listagem de produtos)
- PostgreSQL replica os dados do primary para a replica em tempo real

---

## Resumo da evolução

| Estágio | Usuários | Adição | Problema resolvido |
|---|---|---|---|
| 1 | até 500 | Spring Boot + PostgreSQL | Base funcional |
| 2 | até 5.000 | Redis | Leitura lenta do ranking |
| 3 | até 50.000 | Kafka + load balancer + read replica | Escrita concorrente + escala horizontal |

---

## O que vem depois (100k+)

Para escalar além de 50k usuários, os próximos passos seriam:

- **Sharding do PostgreSQL** por região geográfica (bairros de Salvador em partições separadas)
- **CDN para a API** de ranking — respostas estáticas por bairro servidas na borda
- **OpenTelemetry** para observabilidade — rastrear latência, erros e gargalos em produção
- **Circuit breaker** (Resilience4j) para proteger o sistema quando o banco ou Kafka ficarem indisponíveis

---

## Conceitos Java aplicados

| Conceito | Onde |
|---|---|
| Virtual Threads (Java 21) | Concorrência de requisições nas instâncias Spring Boot |
| Spring Data JPA | Acesso ao banco com abstração de queries |
| Spring Cache + Redis | Cache-aside do ranking por bairro |
| Spring Kafka | Produtor e consumer de eventos de preço |
| Bean Validation | Validação de preços registrados |
| Testcontainers | Testes de integração com PostgreSQL e Redis reais |

---

*Este documento faz parte da [Jornada Dev](https://github.com/peacevan/jornada-dev) — acompanhe a construção no LinkedIn: [ivan-amado-developer](https://www.linkedin.com/in/ivan-amado-developer)*
