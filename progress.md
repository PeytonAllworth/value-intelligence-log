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



## 2025-08-14
- Kicked off a private contract project to design and implement a **value-based market intelligence engine**.
- Today I set up a modular architecture with separate alert, notification, and utility layers.
- I quickly Built and tested an MVP alert pipeline capable of running continuously and triggering based on configurable metrics using hypothical bitcoin price triggers to make sure the logic ran smoothly.
- I also set up private GitHub repo for codebase and public repo(this one!) for work log to maintain confidentiality while showcasing progress for future oppertunities.
- Initial notifier implemented for local testing, laying groundwork for future Telegram integrations for large groups down the line.



# Value Intelligence Engine - Progress Log

## August 14, 2024 - Major System Completion

### ✅ What We Built Today

#### 1. Complete Moving Average Cross System
- **7-day vs 21-day MA detection** for top 20 coins
- **Golden Cross alerts** (7d MA > 21d MA) - bullish momentum
- **Death Cross alerts** (7d MA < 21d MA) - bearish momentum
- **Dual source verification** with CoinGecko + CoinPaprika
- **120-minute cooldown** protection to prevent spam

#### 2. Enhanced Sentiment Alert System
- **Dual source verification** implemented
- **Fear & Greed Index** (Source-A) + **Market sentiment** (Source-B)
- **3-point tolerance** checking between sources
- **Extreme fear (≤25)** and **extreme greed (≥75)** detection
- **60-minute cooldown** protection

#### 3. Complete Redundancy System
- **All 3 metrics** now have dual source verification
- **Price alerts**: 1% tolerance between CoinGecko and CoinPaprika
- **Sentiment alerts**: 3-point tolerance between Fear & Greed and Market data
- **MA cross alerts**: 1% price tolerance for verification
- **Enterprise-grade reliability** achieved

#### 4. Enhanced Rate Limiting & Protection
- **Progressive delays** for historical data fetching (5s base + 2s per request)
- **Exponential backoff** when hitting rate limits
- **Penalty system** for 429 errors (+3 to request counter)
- **Silent operation** during data loading for clean output

#### 5. Trader-Friendly Output System
- **Clean alert formatting** with emojis and clear messaging
- **Progress tracking** for data loading operations
- **Market summary** reports showing scan time and alert count
- **Configurable verbosity** (quiet/verbose modes)

### 🔧 Technical Implementation

#### Files Created/Modified:
- `alerts/ma_cross_alerts.py` - New MA cross detection system
- `utils/math_helpers.py` - Trader-friendly output utilities
- `alerts/price_alerts.py` - Enhanced with dual source verification
- `alerts/sentiment_alerts.py` - Enhanced with dual source verification
- `utils/api_helpers.py` - Added sentiment Source-B API
- `main.py` - Added MA cross metric option
- `utils/price_buffer.py` - Enhanced historical data fetching

#### Key Functions Added:
- `get_ma_cross_status()` - Detects golden/death crosses
- `get_sentiment_b()` - Alternative sentiment source
- `TraderOutput` class - Clean output formatting
- Progressive rate limiting for historical data

### 🎯 Current System Status

#### Alert Types (All Working):
1. **Drawdown Alerts** (`--metric maxdd72`)
   - Thresholds: 3%, 5%, 10%, 15%, 20%
   - 72-hour rolling maximum comparison
   - Dual source verification (1% tolerance)
   - 60-minute cooldown protection

2. **Sentiment Alerts** (`--metric sentiment`)
   - Extreme fear (≤25) and greed (≥75) detection
   - Dual source verification (3-point tolerance)
   - 60-minute cooldown protection

3. **MA Cross Alerts** (`--metric ma_cross`)
   - 7-day vs 21-day moving average crosses
   - Golden cross (bullish) and death cross (bearish)
   - Dual source verification (1% tolerance)
   - 120-minute cooldown protection

#### Redundancy Status: 100% Complete
- ✅ Price alerts: CoinGecko + CoinPaprika
- ✅ Sentiment alerts: Fear & Greed + Market sentiment
- ✅ MA cross alerts: CoinGecko + CoinPaprika

#### Rate Limiting Status: 100% Complete
- ✅ Progressive delays for historical data
- ✅ Exponential backoff on errors
- ✅ Silent operation during data loading

### 🚀 Next Steps

#### Immediate (Ready for Production):
- **Email notifier** integration
- **Telegram bot** integration
- **Scheduling system** (15m/1h intervals)

#### Future Enhancements:
- **Web dashboard** for monitoring
- **Alert history** and analytics
- **Custom threshold** configuration
- **Multiple notification** channels

### 💡 Key Achievements Today

1. **Complete redundancy system** - All metrics now have dual source verification
2. **Production-ready alerts** - All three alert types fully functional
3. **Enterprise-grade reliability** - Rate limiting, cooldowns, and error handling
4. **Trader-friendly interface** - Clean, actionable alerts ready for Telegram
5. **Comprehensive testing** - System tested and working across all metrics

**The Value Intelligence Engine is now a complete, production-ready financial monitoring system with enterprise-grade reliability and redundancy.**