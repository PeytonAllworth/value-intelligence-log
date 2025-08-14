# Value Intelligence Engine — Public Work Log

> This repo is a non-sensitive work log and architecture overview for a private, contracted project.  
> The production code lives in a private repository.



Aug 13:

Today I began planning out the basics around a job i was contacted about to assist in watching digital asset values and oter metrics. I have been hired to find mutiple metrics and have an expectation of it running smoothly for their clients. I will call on redundant data sets to have backups in place if an API is temporarily unreliable. I also made sure the numbers from both sources are close enough before triggering anything to prevent false data being set out to client. The client signed off on how the tool will handle alerts based on market changes and other relevant metrics. In respect to the clients competitive business metrics my code, specific logic, and custume models are kept private. This README.md can be used to get surface level insights for my contracted work.

**Skills demonstrated:** Python (requests), API integration, data normalization, event-driven logic, redundancy/tolerance checks, CLI tooling, finance/econ metrics.


## System Overview

```mermaid
flowchart LR
    %% ====== Data Sources ======
    subgraph Data_Sources
        A[Price Source A<br/>(e.g., CoinGecko)]
        B[Price Source B<br/>(e.g., CoinPaprika)]
        C[Sentiment Source<br/>(e.g., Fear & Greed)]
    end

    %% ====== Processing Layer ======
    subgraph Processing (Value Intelligence Engine)
        D1[Normalize & Validate<br/>(schemas, units)]
        D2[Redundancy Check<br/>(cross-source tolerance)]
        D3[Value Rules<br/>(e.g., 72h drawdown ≥ 20%)]
        D4[Regime Rules<br/>(e.g., sentiment &lt;45 or &gt;55)]
        D5[Decision Router<br/>(alert if conditions met)]
    end

    %% ====== Notification Sinks ======
    subgraph Notifications
        E[Terminal Print<br/>(MVP)]
        F[Email Alert<br/>(optional next)]
        G[Telegram Bot<br/>(later)]
    end

    %% ====== Optional Storage ======
    subgraph Storage (optional)
        H[Local Logs / CSV]
    end

    %% ====== Flows ======
    A --> D1
    B --> D1
    C --> D1
    D1 --> D2 --> D3
    D2 --> D4
    D3 --> D5
    D4 --> D5
    D5 --> E
    D5 --> F
    D5 -. roadmap .-> G
    D5 --> H
```

**Design goals**
- *Value‑based decisions*: alerts only trigger when value metrics cross defined thresholds, not on every price twitch.
- *Redundancy*: choose action **only** when independent sources agree within a small tolerance.
- *Extensible*: add new metrics, sources, or sinks (email/Telegram) without changing core flow.
```
