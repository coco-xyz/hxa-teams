# Infrastructure

Everything you need to run an investment operations team: AI subscriptions, agent platforms, data sources, communication, and monitoring setup.

---

## AI Subscriptions

You need AI model access for the agents and their specialized tasks.

### For Agent Runtime

| Provider | Plan | Purpose | Est. Cost |
|----------|------|---------|-----------|
| Anthropic (Claude) | Max or API | Primary agent runtime (Roey, Boy, Joey, Zylos_ABCDE) | $100-200/mo per agent |
| OpenAI (GPT) | API | Supplementary analysis, alternative perspective | $20-100/mo |

**Notes:**
- Claude Max provides generous usage for persistent agents running on platforms like Zylos
- Using different AI providers for different analysis tasks gives complementary perspectives
- Investment research may consume more tokens than development tasks due to long-context analysis

### Recommended Configuration

| Role | AI Model | Reasoning |
|------|----------|-----------|
| Investment Coordinator (Roey) | Claude (Anthropic) | Strong at coordination, classification, long-context daily reports |
| Research Analyst (Boy) | Claude (Anthropic) | Deep analytical reasoning for DD reports and backtesting |
| Investment Engineer (Joey) | Claude (Anthropic) | Code generation for models, data pipelines |
| Security Committee (Zylos_ABCDE) | Claude (Anthropic) | Analytical reasoning for audit, compliance, risk assessment |

---

## Agent Platforms

Each agent needs a runtime environment with persistence, communication, and scheduling.

### Recommended: Zylos (All Agents)

[Zylos](https://github.com/zylos-ai/zylos-core) is the recommended platform for all investment team agents:

**Capabilities:**
- Claude Code integration
- Persistent memory across sessions
- Task scheduler (self-wake for reports, audits, monitoring cycles)
- External communication (hxa-connect, Telegram, external APIs)
- Code execution (for modeling, backtesting, data pipelines)
- 24/7 uptime on cloud servers

**Deployment:**

| Agent | Deployment | Why |
|-------|-----------|-----|
| Roey | Cloud server | Always-on signal monitoring, 24/7 availability |
| Boy | Cloud server | Always-on for time-sensitive research tasks |
| Joey | Cloud or local | Model building, data pipelines — can be local if latency is acceptable |
| Zylos_ABCDE | Cloud server | Independent audit operations, scheduled 03:30 UTC scans |

### Minimum Viable Setup

| Component | Option |
|-----------|--------|
| 1 cloud server (shared) | All 4 agents on a single server with PM2 process management |
| hxa-connect instance | `boot.hxa.net` (BotsHub) |

You can run multiple agents on the same server — they operate in separate Zylos instances and do not interfere with each other.

---

## Data Sources

Investment operations require access to market data, on-chain data, and news feeds.

### Market Data

| Source | Type | Purpose | Est. Cost |
|--------|------|---------|-----------|
| CoinGecko API | Market data | Price feeds, market cap, volume, historical data | Free tier / $129+/mo |
| CoinMarketCap API | Market data | Supplementary price data, rankings | Free tier / $79+/mo |
| Binance API | Exchange data | Real-time trades, order book, funding rates | Free |
| Exchange APIs (various) | Exchange data | Multi-exchange coverage for volume, liquidity | Free |

### On-Chain Data

| Source | Type | Purpose | Est. Cost |
|--------|------|---------|-----------|
| Dune Analytics | On-chain analytics | Custom queries, whale tracking, protocol metrics | Free tier / $349+/mo |
| DefiLlama | DeFi data | TVL, protocol metrics, yield data | Free |
| Etherscan / Block explorers | On-chain data | Transaction lookup, contract analysis | Free / $199+/mo |
| Nansen | On-chain analytics | Wallet labeling, smart money tracking | $150+/mo |

### News and Sentiment

| Source | Type | Purpose | Est. Cost |
|--------|------|---------|-----------|
| CryptoPanic API | News aggregator | Aggregated crypto news with sentiment scoring | Free tier / $49+/mo |
| Twitter/X API | Social media | Influencer tracking, sentiment analysis | $100+/mo |
| RSS feeds | News | Direct from major outlets | Free |

### Research Platforms

| Source | Type | Purpose | Est. Cost |
|--------|------|---------|-----------|
| Messari | Research | Professional crypto research, governance tracking, token profiles | $300+/mo |
| Token Terminal | Financial data | Protocol revenue, earnings, financial metrics | $325+/mo |
| DeFi Safety | Security ratings | Protocol security scores and audits | Free |

**Notes:**
- Start with free tiers and upgrade based on actual usage
- Data source API keys should be managed centrally (assigned to Roey) and shared via secure configuration
- Not all sources are required from day one — prioritize based on investment focus

---

## Communication

### Agent-to-Agent Communication (hxa-connect)

**Instance:** `boot.hxa.net` (BotsHub)
**Organization:** `bman`

Channels to create:

| Channel | Members | Purpose |
|---------|---------|---------|
| `#general` | All agents + BMAN | Red signal broadcasts, daily reports, cross-team announcements |
| `#intel` | Roey, Zylos_ABCDE | Signal triage output, intelligence summaries, audit findings |
| `#research` | Boy, Roey, Joey | DD reports, backtest results, investment modeling |

**Direct message pairs:**
- Roey ↔ Boy (research dispatch)
- Roey ↔ Joey (modeling tasks)
- Zylos_ABCDE ↔ any agent (audit inquiries)

**Thread structure:**
Each investment opportunity or research task gets its own thread within the relevant channel, with attached artifacts (reports, models, audit results).

### Human-Agent Communication

The Strategic Decision Maker (BMAN) needs a channel to interact with the team:

| Platform | Use Case | Setup |
|----------|----------|-------|
| **hxa-connect** | Primary: daily reports, red signals, decisions | BMAN joins `boot.hxa.net` directly |
| **Telegram** | Quick decisions, mobile access | Roey relays via Telegram bot |

**Recommended flow:**
1. Roey posts daily reports and red signals to `#general`
2. BMAN reviews via hxa-connect or Telegram relay
3. BMAN responds with decisions
4. Roey distributes decisions to the team

---

## Knowledge Storage

### Signal and Decision Database

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Signal history | SQLite | Timestamped log of all signals with classification |
| Decision log | SQLite | Investment decisions with reasoning and outcomes |
| Research archive | Qdrant (vector DB) | Semantic search over DD reports and research |
| Model versions | Git | Version-controlled investment models |

**Notes:**
- SQLite is suitable for the signal/decision log — simple, embedded, no infrastructure overhead
- Qdrant enables semantic search over research history ("find all DD reports about L2 scaling solutions")
- Each decision cycle updates the knowledge base for the feedback loop

---

## Infrastructure Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    Cloud Server                          │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Roey (Zylos) — Investment Coordinator            │  │
│  │  - Signal monitoring (market + on-chain APIs)     │  │
│  │  - Scheduler (daily reports 10:00/22:00)          │  │
│  │  - hxa-connect bridge                             │  │
│  └───────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Boy (Zylos) — Research Analyst                   │  │
│  │  - Web research, data analysis                    │  │
│  │  - DD report generation                           │  │
│  └───────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Joey (Zylos) — Investment Engineer               │  │
│  │  - Model building, data pipelines                 │  │
│  │  - Backtesting framework                          │  │
│  └───────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Zylos_ABCDE (Zylos) — Security Committee        │  │
│  │  - Scheduler (daily audit 03:30 UTC)              │  │
│  │  - Compliance + risk scanning                     │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
                          │
                          │ hxa-connect (boot.hxa.net)
                          │
┌─────────────────────────────────────────────────────────┐
│                   hxa-connect Hub                        │
│  - #general (all)                                       │
│  - #intel (Roey, Zylos_ABCDE)                          │
│  - #research (Boy, Roey, Joey)                          │
│  - DM channels                                          │
└─────────────────────────────────────────────────────────┘
                          │
                          │ APIs
                          │
┌─────────────────────────────────────────────────────────┐
│                 External Data Sources                     │
│  - Market APIs (CoinGecko, Binance, etc.)               │
│  - On-chain (Dune, DefiLlama, Nansen)                   │
│  - News (CryptoPanic, RSS, Twitter/X)                   │
│  - Research (Messari, Token Terminal)                     │
└─────────────────────────────────────────────────────────┘
                          │
                          │ Reports + Decisions
                          │
┌─────────────────────────────────────────────────────────┐
│                 Knowledge Storage                        │
│  - SQLite (signal log, decision log)                    │
│  - Qdrant (research archive, semantic search)           │
│  - Git (model versions)                                 │
└─────────────────────────────────────────────────────────┘
```

---

## Cost Estimate

| Item | Monthly Cost | Notes |
|------|-------------|-------|
| Claude Max (per agent) | $100-200 | 4 agents = $400-800/mo |
| Cloud server | $40-100 | Shared server for all agents |
| Data APIs (free tiers) | $0 | CoinGecko, DefiLlama, Binance, etc. |
| Data APIs (paid) | $0-800 | Nansen, Messari, Token Terminal — as needed |
| Qdrant (self-hosted) | $0 | Runs on same server |
| hxa-connect (BotsHub) | $0 | Self-hosted at boot.hxa.net |
| **Total (minimum)** | **$440-900/mo** | **4 agents + free data sources** |
| **Total (full data stack)** | **$1,240-1,700/mo** | **4 agents + paid data APIs** |

Start with free data sources and add paid subscriptions based on research needs and investment volume.
