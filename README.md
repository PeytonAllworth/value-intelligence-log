# Value Intelligence Engine

> This repo is a non-sensitive overview for a private, contracted project.  
> The production code lives in a private repository.





Alerts clearly defined for test branches and resetting to main:
this section is like a note to self of the exact alerts. i often change the alert threshold to test changes and need them to output more often. when implimenting changes this section helps me double check i dont accidently change the bot objecties.

note to self on order as of Aug 22
Next iteration (in ~2-10 minutes):
├── Sentiment check: Every 5 min ✅
├── Drawdown check: Every 2 min ✅  
├── MA cross check: Every 10 min ✅
└── All using stored historical data


How to Make It Run Automatically:
Option 1: Cloud Hosting (Recommended)
off
GitHub → Cloud Service → Always-on Monitoring
├── Push code to GitHub
├── Deploy to AWS/Google Cloud/DigitalOcean
├── Server runs your script 24/7
└── Sends alerts even when laptop is off


Immediate Actions You Can Take:
1. Test Individual Components:
py
# Test Telegram integration
2. Refine Alert Thresholds:
Make them even more sensitive for faster testing
Adjust cooldown times (currently 60 minutes)
Add more alert types (volume spikes, price volatility)
3. Improve Telegram Messages:
Add more emojis and formatting
Include price charts or links
Add market context (BTC dominance, market cap changes)



next step for me

3.5 CoinMarketCap Integration:
Requires API key for reliable access
More complex setup - not ideal for testing today
Better for production - but we need testing first!


3.75 . Drawdown Alerts:
Source-B API failing for all coins (verification step failing)
Your system requires Source-B verification before sending alerts
Result: No drawdown alerts can fire (find new source b or find why its failing)

Cause:
CoinPaprika (Whjich Is Source-B):
Status: 402 Payment Required
Issue: Hit rate limits, requires payment
Result: All verification failing

Solution: 
temporary removal of backup source
Reminder:
re impliment back up source after dev budget provided

add these 3.9 

Comprehensive Crypto Alert Types
1. Price Movement Alerts
Daily % Move: Notify if price moves ±X% in 24h. (done)
Hourly Volatility Spike: Sudden move of >2–3% in 1 hour.
Breakouts: Price breaks out of last 7/30-day range.
Whale Moves: Single trade >$10M in spot/futures markets.
2. Support & Resistance Alerts
Support Break: Price breaks below major support (calculated from recent lows).
Resistance Break: Price clears a known resistance.
Retest Alert: Price retests a broken support/resistance level.
3. Technical Indicator Alerts
Golden Cross / Death Cross: 50-day SMA crosses above/below 200-day SMA.
Fibonacci Retracements: Price touches 38.2%, 50%, or 61.8% retracement from recent swing high/low.
RSI Alerts: RSI >70 (overbought) or <30 (oversold).
MACD Crossovers: Bullish/bearish MACD signal line cross.
Bollinger Band Breaks: Price closes outside the band (potential reversal or continuation).
4. Volume & Liquidity Alerts
Volume Spike: 2× average hourly/daily volume.
Exchange Flow: Large inflows/outflows from centralized exchanges.
Liquidity Pool Changes: Sudden shifts in top DEX liquidity pools.
5. Market Structure Alerts
Higher Highs / Lower Lows: Trend confirmation alerts.
Trend Reversal Patterns: Double top/bottom, head and shoulders, etc.
Consolidation Breakout: End of sideways chop.
6. Derivatives / Funding Rate Alerts
Funding Rate Extremes: Funding >0.1% (overleveraged longs) or <-0.1% (overleveraged shorts).
Open Interest Surge: Rapid OI increase/decrease → possible squeeze setup.
Liquidation Cascades: $50M+ long or short liquidations in 1h.
7. Sentiment & Flow Alerts
Fear & Greed Index Shift: Large daily moves in sentiment index.
Social Spike: Token trending on Twitter/Telegram/Reddit.
Stablecoin Flows: Large mint/burn or on-chain transfer (USDT, USDC).
8. On-Chain / Whale Alerts
Whale Transfers: >10,000 BTC or >100k ETH moved on-chain.
Dormant Wallet Activity: Coins unmoved for 2+ years suddenly active.
Smart Money Movements: Tracked wallet buys/sells top coins.
🎯 How to Control Noise
Normal Days: Trigger only 0–1 alerts (major moves, volume spikes).
Big Market Days: Trigger 3–5 alerts (trend shifts, golden cross, whale moves).
Add a daily summary digest (optional) so users see movement without needing constant pings.


💻 Development Tasks:
4. Add New Features:
Email notifications (backup to Telegram)
Web dashboard (see alerts in browser)
Alert history (store all alerts in database)
Custom alert rules (user-defined thresholds)
5. Improve Data Quality:
Add more data sources (Binance, Coinbase APIs)
Implement data validation (check for outliers)
Add confidence scores (how reliable is each alert)
6. Create Production Infrastructure:
Docker containerization (easy deployment)
Configuration files (environment variables)
Logging system (better debugging)
Health checks (monitor system status)
📊 Business Development:
7. Plan Your Telegram Group:
Set up group rules and guidelines
Create welcome message explaining alerts
Plan monetization strategy
Design group branding and description
8. Document Everything:
Update README.md with current status
Create user manual for your group
Write technical documentation for developers
Create marketing materials for potential users
🎯 My Recommendation:
Start with #1 and #2 - test your current system and refine the alerts. This will:
✅ Validate everything works before going live
✅ Improve user experience in your Telegram group
✅ Keep you engaged while waiting for data loading
✅ Make your system more valuable to users
Bottom Line:
You have plenty of productive work to do! The data loading is just one part of building a complete system. Use this time to perfect the other components.




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
