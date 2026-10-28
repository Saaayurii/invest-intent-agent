# Intent Agent

AI-агент, который превращает торговое намерение на обычном языке в проверенный план заявок, объясняет каждый шаг и исполняет его через T-Invest MCP после подтверждения клиента.

Концепция для программы «Инвест. Премаркет», трек «AI-агент».

> «Зафиксируй половину прибыли по Сберу, а если упадёт ниже 290 — продай всё»

**Страница проекта:** https://saaayurii.github.io/invest-intent-agent/

## Сценарий

```mermaid
flowchart LR
    U[Клиент: текст или голос] --> R{Классификатор}
    R -->|торговая команда| A[AI-агент: JSON-план]
    A --> C[Код: расчёт и риски]
    C --> K[Карточка плана]
    K --> P{Подтверждение}
    P -->|да| E[Исполнение через T-Invest MCP]
    P -->|правка| A
    E --> M[Монитор условий]
    M --> N[Уведомление клиенту]
```

AI понимает намерение и объясняет. Расчёт объёма, проверка рисков и выставление заявок — детерминированный код.

## Архитектура MVP

```mermaid
flowchart TB
    subgraph Client[Клиент]
        W[Next.js + PWA]
    end
    subgraph Backend[Бэкенд]
        API[FastAPI + WebSocket]
        AG[AI-агент: Qwen / Ollama]
        CL[Классификатор да/нет]
        STT[Whisper]
        RISK[Расчёт и риски]
        EX[Исполнитель]
        MON[Монитор условий]
    end
    subgraph Data[Данные]
        PG[(PostgreSQL)]
        RD[(Redis)]
        MS[(Meilisearch: RAG по Markdown)]
    end
    KF{{Kafka}}
    MCP[T-Invest MCP sandbox]
    IA[Invest API: стрим котировок]

    W <--> API
    API --> CL --> AG
    API --> STT
    AG --> MS
    AG --> RISK --> EX --> MCP
    IA --> KF --> MON --> EX
    EX --> KF
    API --- PG
    API --- RD
```

## Статус

Концепция и архитектура. MVP разрабатывается на этапе SYSTEM BUILD; все сделки выполняются в sandbox.

Не является инвестиционной рекомендацией.
