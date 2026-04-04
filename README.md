# 🚀 Jornada Dev: IA + Engenharia de Software na Prática

> Aprender construindo coisas reais. Cada MVP é um capítulo dessa evolução.

---

## 📖 Sobre esta jornada

Depois de anos na área de tecnologia, decidi que era hora de evoluir — não apenas acompanhar as mudanças, mas dominar as ferramentas que estão redefinindo o mercado.

A estratégia é simples: desenvolver MVPs que resolvem problemas reais do meu próprio dia a dia, aplicando na prática os conceitos do roadmap **Dev Fullstack Java + Angular na Era da IA**.

> **IA substitui tarefas, não contexto.** Quanto mais você domina o que a IA não consegue fazer sozinha — julgamento técnico, contexto de negócio, decisões sob incerteza — mais indispensável você se torna.

Este repositório **não contém código-fonte**. Ele é o **diário de bordo da jornada** — cada pasta documenta o passo a passo de construção de cada MVP: decisões técnicas, arquitetura, erros cometidos, lições aprendidas e links para os repositórios com os fontes.

---

## 🧠 Filosofia de Stack

Cada projeto tem **duas fases de stack**:

- **MVP** — prioridade é velocidade de validação. A stack escolhida é a mais barata e rápida para testar a ideia no mundo real. Pode ser React, Supabase, no-code, Python simples — o que entregar mais rápido.
- **Produto** — após validação, a stack evolui para o roadmap principal: Java 21, Spring Boot, Angular, Cloud. Aqui entra a engenharia de verdade.

> MVP não é sobre a stack certa. É sobre aprender rápido com o menor custo possível.

---

## 🗺️ MVPs

| # | Projeto | Descrição | Status | Repositório |
|---|---------|-----------|--------|-------------|
| 1 | [smartlist](./smartlist/) | Lista inteligente de supermercado com ranking de preços por região | 🟡 Em desenvolvimento | [→ ver código](#) |
| 2 | [helptea](./helptea/) | Registro de evolução para crianças autistas com botão SOS | ⚪ Planejado | — |
| 3 | [garimpeimovel](./garimpeimovel/) | Radar de imóveis da Caixa com alertas de oportunidades | ⚪ Planejado | — |
| 4 | [english-challenge](./english-challenge/) | App para aprender inglês por desafios diários | ⚪ Planejado | — |
| 5 | [less-procrastination](./less-procrastination/) | Ferramenta anti-procrastinação com micro-sprints | ⚪ Planejado | — |

---

## 📁 Estrutura do Repositório

```
/jornada-dev
│
├── smartlist/
│   ├── README.md              ← visão geral e links do projeto
│   ├── 01-problema.md         ← a dor real que motivou o projeto
│   ├── 02-arquitetura.md      ← decisões técnicas e stack escolhida
│   ├── 03-desenvolvimento.md  ← passo a passo da construção
│   └── 04-aprendizados.md     ← erros, soluções e lições
│
├── helptea/
├── garimpeimovel/
├── english-challenge/
└── less-procrastination/
```

---

## 📦 Projetos

### smartlist
> 🟡 Em desenvolvimento · [Ver código →](#)

Crie sua lista de supermercado por voz, texto ou histórico em segundos. O diferencial: os dados coletivos dos usuários formam um **ranking de supermercados mais baratos por região**, como um Waze de preços. Quanto mais pessoas usam, mais inteligente fica.

| | Stack |
|---|---|
| **MVP** | React · Supabase · LLM API |
| **Produto** | Java 21 · Spring Boot · Angular · PostgreSQL · pgvector |

**Conceitos aplicados:**
- Integração com LLM para criação de lista por linguagem natural
- **RAG** para sugestão de itens baseada em histórico do usuário
- Modelo de **dados colaborativos** — inteligência cresce com o uso
- **Angular Signals** para reatividade na interface (fase produto)
- **API REST** com Spring Boot + boas práticas de design (fase produto)

📂 [Acompanhar passo a passo](./smartlist/)

---

### helptea
> ⚪ Planejado

Registre comportamentos, gatilhos e marcos de desenvolvimento da sua criança autista de forma rápida. Gere históricos e relatórios automáticos. E quando vier uma crise: um **botão SOS** com orientações práticas baseadas no perfil dela.

Construído por um pai, para pais.

| | Stack |
|---|---|
| **MVP** | A definir |
| **Produto** | Java 21 · Spring Boot · Angular · PostgreSQL |

**Conceitos aplicados:**
- **DDD (Domain-Driven Design)** — modelagem de um domínio sensível e complexo
- **LLM com contexto** — o botão SOS usa o histórico da criança como contexto para a resposta
- **Geração de relatórios** — exportação estruturada para compartilhar com terapeutas
- **Architecture Decision Records (ADRs)** — documentando decisões críticas de privacidade e segurança

📂 [Acompanhar passo a passo](./helptea/)

---

### garimpeimovel
> ⚪ Planejado

Radar de imóveis da Caixa que baixa automaticamente a lista oficial todos os dias e filtra as melhores oportunidades por região, desconto e modalidade. Nunca mais perca um imóvel com 40% de desconto por não ter visto a tempo.

| | Stack |
|---|---|
| **MVP** | A definir |
| **Produto** | Java 21 · Spring Boot · Angular · Python · PostgreSQL |

**Conceitos aplicados:**
- **Automação e scripting** com Python para coleta diária de dados
- **Event-driven** — notificações disparadas por eventos de novos imóveis ou mudança de preço
- **Observabilidade** — monitoramento do pipeline de coleta com OpenTelemetry
- **CI/CD** — pipeline automatizado para deploy e execução do script diário

📂 [Acompanhar passo a passo](./garimpeimovel/)

---

### english-challenge
> ⚪ Planejado

Para quem procrastina e quer fluência de verdade, sem aulas chatas. Aprenda inglês por meio de desafios diários curtos e gamificados, com feedback inteligente via IA.

| | Stack |
|---|---|
| **MVP** | A definir |
| **Produto** | Java 21 · Spring Boot · Angular · LLM API |

**Conceitos aplicados:**
- **Agentes LLM** — avaliação de respostas e geração de feedback personalizado
- **Prompt engineering** — few-shot e chain-of-thought para feedback educacional eficaz
- **Gamificação** — design de sistema de recompensas e progressão
- **Angular avançado** — standalone components, performance e UX fluída (fase produto)

📂 [Acompanhar passo a passo](./english-challenge/)

---

### less-procrastination
> ⚪ Planejado

Produtividade aplicada ao dia a dia em micro-sprints alcançáveis. Para quem sabe o que precisa fazer, mas fica travado na execução.

| | Stack |
|---|---|
| **MVP** | A definir |
| **Produto** | Java 21 · Spring Boot · Angular |

**Conceitos aplicados:**
- **Product thinking** — aplicar metodologia ágil num produto pessoal
- **Java 21 Virtual Threads** — explorar concorrência moderna no backend (fase produto)
- **Micro-frontends** — experimento com Module Federation no Angular (fase produto)
- **ADRs** — documentando as decisões de produto e tecnologia

📂 [Acompanhar passo a passo](./less-procrastination/)

---

## 🛠️ Stack da Jornada

```
MVP (validação rápida)
  → React · Supabase · Python · no-code · o que entregar mais rápido

Produto (após validação)
  → Backend:   Java 21 · Spring Boot · Virtual Threads · Python
  → Frontend:  Angular 17+ · Signals · Standalone Components
  → Dados:     PostgreSQL · pgvector (busca semântica)
  → IA / LLMs: Spring AI · LangChain4j · Anthropic API · OpenAI API
  → Arquitet.: DDD · Event-driven · REST · ADRs
  → DevOps:    Docker · CI/CD · Terraform · OpenTelemetry
  → Cloud:     AWS / GCP
```

---

## 🧭 Roadmap de Conceitos por Camada

Este projeto segue um roadmap estruturado em 4 camadas de evolução:

| Camada | Foco | Aplicado em |
|--------|------|-------------|
| 1️⃣ Diferencial técnico | Java 21+, Angular Signals, API design | smartlist, less-procrastination |
| 2️⃣ Contexto de negócio | DDD, Event Storming, ADRs, Product thinking | helptea, less-procrastination |
| 3️⃣ Cloud e Observabilidade | CI/CD, OpenTelemetry, Terraform, Docker | garimpeimovel, todos |
| 4️⃣ IA como ferramenta | LLMs, RAG, Agentes, Prompt Engineering | smartlist, helptea, english-challenge |

---

## 📬 Acompanhe a jornada

Cada MVP vira uma série de posts no LinkedIn com decisões técnicas, erros cometidos e lições aprendidas.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Acompanhar_Jornada-0077B5?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/seu-perfil)

---

## 📄 Licença

MIT License — sinta-se à vontade para usar, adaptar e evoluir.
