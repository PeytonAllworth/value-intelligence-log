# Value Intelligence Engine

> This repo is a non-sensitive overview for a private, contracted project.  
> The production code lives in a private repository.





Aug 13:

Today I began planning out the basics around a job i was contacted about to assist in watching digital asset values and oter metrics. I have been hired to find mutiple metrics and have an expectation of it running smoothly for their clients. I will call on redundant data sets to have backups in place if an API is temporarily unreliable. I also made sure the numbers from both sources are close enough before triggering anything to prevent false data being set out to client. The client signed off on how the tool will handle alerts based on market changes and other relevant metrics. In respect to the clients competitive business metrics my code, specific logic, and custume models are kept private. This README.md can be used to get surface level insights for my contracted work.

For more specific insights to daily tasks -> progress.md


Aug 14: 

## What Works Today
- **Live sentiment alert (45/55):** Fear & Greed Index with dual source verification and 3-point tolerance checking. This now prints clean terminal alerts when sentiment flips to fear/greed.
- **Top-20 drawdown detection (72h rolling):** Batched price checking with multiple thresholds: 20%, 15%, 10%, 5%, and 3% drops from 72-hour rolling maximum. This is overkill and temporary to get more outputs when test telegram is live. Forced to includes cooldown protection and dual source verification. Will make it more "live" after other features are added and dev budget is delivered.
- **Moving Average Cross System:** 7-day vs 21-day MA golden cross and death cross detection for all top 20 coins and using a dual source verification to ensure accurate calls; I also have 120-minute cooldown protection.

## How I Keep It Reliable (Free Tier, Temporary)
- **Batched calls** for top-20 prices; efficient historical data fetching with progressive rate limiting and reliable monitering of top 20 coin changes. Logic gaps like screening out usdc or wrapped btc still must be implimented.
- **Local rolling buffer** (72h) to compute drops without re-hitting history endpoints.
- **Complete redundancy system:** All metrics verified with dual sources before sending alerts.
- **Rate limiting & backoff:** Progressive delays and exponential backoff to prevent API overload. Future Dev budget will make calls more "on time".
- **Cooldown protection:** Prevents spam alerts with configurable timeouts.

## System Features
- **Dual Source Verification:** CoinGecko + CoinPaprika for prices, Fear & Greed + Market sentiment for sentiment.
- **Tolerance Requirements:** 1% price variance, 3-point sentiment variance(need to backtest how frequently 3+ point divergence occurs.)
- **Alert Types:** Drawdown alerts, sentiment regime alerts, moving average cross alerts.
- **Clean Output:** Built a trader-friendly formatting with emojis and actionable information.(will adjust later on)

## Near-Term Roadmap
- ✅ **Source B + tolerance** for sentiment and drawdown (COMPLETED)
- ✅ **Any-window drawdown** with cooldown (COMPLETED)
- ✅ **Clean alert formatting** ready for Telegram (COMPLETED)
- ⏳ **Private issues alert sytem** 
- ⏳ **Telegram bot** integration for group alerts. Must check with client on short term and long term asks
- ⏳ **Scheduling system** (15m/1h intervals for automated monitoring) Will condense to on the second alerts with paid API subscriptions.
- ⏳ **Added features** for alerts and financial and economic indicators: TBD
- ⏳ **Added features** for payments tracking

## Current Status
The system is production-ready after telegram logic added, telegram API integration, outputs polished, and sufficient bug fixing, all with enterprise-grade redundancy, rate limiting, and alert protection. All three alert types (drawdown, sentiment, MA crosses) are fully functional with dual source verification.
