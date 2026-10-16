# Marcel Julien

### AI systems architect — agentic systems, quantitative trading, remote sensing. Built, deployed, operated.

15+ years in information systems. I write the hypothesis before the test, measure against a baseline, and switch off
what doesn't pass, whether it's a trading agent, a satellite classifier or an outbound pipeline.

---

## Systems

### 📣 B2B outbound & CRM automation — US market (current)
I build and run the outbound machine behind a B2B sales team: the contact databases, the email sending, and the
coordination of every lead from first touch to a rep's call list. Built and operated
inside an autonomous development loop on Claude Code: a planning model writes the PRD, tasks are routed to cheaper or
stronger models by complexity, progress is auto-committed and every session is archived.

- **Databases:** owner and company data assembled from public records, places APIs and paid lookups, deduplicated and
  loaded into the CRM.
- **Cold-email infrastructure:** SmartLead across 3 sending domains and 9 mailboxes. Warm-up, capacity planning and
  multi-market launches (Tennessee, Broward, Dallas).
- **CRM architecture:** GoHighLevel as system of record for **115,000 contacts**. 9-stage pipeline, custom fields,
  lead-ownership rules, and workflows that create opportunities from call dispositions.
- **Two-way sync:** SmartLead → GHL over webhooks and REST. Replies, unsubscribes and do-not-contact reach the CRM,
  and a rep's stage change pauses the cold sequence.
- **Enrichment waterfall** for property-owner data, public records first and paid skip-trace last: Apollo, Google
  Places, PropertyRadar, NeverBounce.
- **Deliverability:** found that verification was silently removing **36 % of a list** before the first send, and
  changed list preparation around it.
- **Lead scoring:** a ZIP-level market model that decides where limited paid-lookup credits are spent.
- **Vendor bake-offs:** data vendors tested against known-answer cases before any annual commitment.
- **Sales enablement:** a scheduled job emails each rep a prioritised daily call list with the previous day's replies,
  meetings and calls.
- **Compliance:** unsubscribe headers, footers and suppression on every campaign; wrongly suppressed contacts audited
  and restored.
- **Lead coordination:** replies, meetings and call outcomes flow back into the pipeline so each lead has one owner
  and one next step.
- Run day to day with that loop, Python, GitHub PRs, and Linear to coordinate the work.

### ⛅ Weather-market trading on Polymarket — live, own capital
Medium-frequency (seconds, not ms) system pricing temperature markets from weather forecasts.

- **Architecture:** FastAPI monolith running 6 asyncio services (market scanner, listing sniper, swarm runner, oracle
  poller, source poller, hourly forecast snapshot) · PostgreSQL (Alembic) as source of truth · append-only SQLite
  archive · Redis · React/Vite console · systemd on Linux · Prometheus/Grafana
- **Signal:** 16 weather forecast sources, including TimesFM (time-series foundation model) served on a separate box →
  per-bucket probabilities vs the CLOB book (WebSocket + Gamma REST)
- **Agents:** 34 in a three-layer consensus: quant (KL divergence, Bayesian LLR, Kalman), forecast personalities,
  Kelly synthesis → TRADE / HOLD / FADE. Signal-fingerprint reasoning cache: 3 s → 30 ms on hits
- **Risk & truth:** kill bar enforced in code on every live agent · ledger rebuilt from on-chain fills, not from the
  bot's own records · paper = live parity (same caps, exit tiers, 15 s confirm against the live book) · capped live
  pilots and pre-registered studies with a placebo and a pass bar. A maker study failed on adverse selection, so it
  was closed.
- 1,000+ commits in 2026

### 🛰️ Agave from space — [code](https://github.com/hubdemarcel/AIforEarthDataSets/tree/claude/cloud-credit-session-8fee9m/agricultura)
Per-pixel supply inventory of agave across the 181 municipalities of the tequila appellation, 1993–2026.

- **Inputs:** Sentinel-2 L2A COGs (AWS `sentinel-cogs`), SCL cloud masking, monthly medians · Landsat 5/7/8
  (1993–2017) · Copernicus DEM 30 m for elevation and slope
- **Features:** NDVI, NDRE, NDMI, BSI per month; dry vs rainy-season statistics; 10-band spectral signature
- **Models:** k-means on greenness calendars to find patterns without labels · Random Forest with held-out plots ·
  multi-year rules for **planting year, harvest (jima), upkeep and confidence**: agave stays flat all year, maize
  peaks Jul–Sep
- **Engineering:** processing by MGRS tile with an unattended nightly queue · Landsat harmonised against stable
  fields, because sensor jumps were inventing plantations (one year dropped from 35,957 to 4,626 ha after the fix) ·
  UTM 13/14 double-count guard · resumable reads, disk-space stop
- **Validation:** vs official statistics (SIAP), log correlation 0.61 across municipalities; over-detection traced
  to peri-urban grassland. A price model tested with rolling-origin validation does not beat the naive baseline, and
  the write-up says why.

### 📈 Multi-asset research engine — paper
- **Collection:** one asyncio process over 20+ live feeds (Polymarket, Kalshi WS, Binance, Coinbase, Hyperliquid,
  Deribit, FX, football), per-source TOML config (`enabled | campaign | off`), a nightly budgeted window and a job
  queue. It replaced 25 processes and ~3 GB of RAM with one at ~200 MB.
- **Point-in-time data:** FRED with every vintage, Banxico, EIA. A value is visible the day after it was published,
  not before.
- **Backtesting:** one card per strategy written before the run, with kill criteria and a held-out vault opened
  once. Three strategies killed so far.
- **Cross-venue studies:** lead/lag (Binance and Coinbase lead Hyperliquid in 44 of 44 measured hours) · 67
  Polymarket ↔ Kalshi pairs matched by reading both resolution rules · exact fee models per venue
- **Code review at scale:** 195 public trading repos read into fixed-schema cards → 501 numbered findings. 165 of 195
  publish no measured result.

### 🥃 Agaves.pro — directory and market intelligence for agave spirits ([agaves.pro](https://agaves.pro))

**Live today (v1): an automated content and directory engine**
- **Publishing pipeline:** Notion → WordPress/WooCommerce with staged review, image handling and pre-publish checks.
  Thousands of tequila and mezcal product pages live, plus 1,172 articles.
- **Content automation:** ingestion from RSS, web and social; LLM rewrite → fact check → classify → approve, with
  quality gates that catch prompt leakage and false attributions before anything is published; automated newsletter.
- **CRM:** 6,500+ companies in Notion, read and written through the API, cross-referenced with the catalogue and
  Search Console. Contact discovery in four layers (search, LLM classification, logged-in browser check, manual
  review); outreach deduplicated by mailbox after finding 731 records pointing at 20 inboxes.
- **Analytics:** GA4 and Search Console through a service account; an open-source plugin for per-listing traffic.

**In progress (v2): from directory to data platform**
Rebuilding the live directory as an entity graph that serves producers, brands, buyers, importers and consumers.

- **Master registry with stable IDs:** brands (8,221), expressions (18,962, one page each), companies (9,262, with
  dynamic roles: owner, importer, distributor, supplier), distilleries by NOM (439), points of sale, people,
  cocktails, events, offers. Merged IDs survive as aliases, so old facts still find their page.
- **Everything else is a dated fact** pointing at an ID, with source, capture date and confidence. Nothing is
  overwritten; history is part of the value. Doubtful matches go to human review, never auto-merged.
- **Layers:** raw cache → one extractor per source → registry → fact tables → a 360° JSON view per entity. Layers 3–4
  rebuild offline from 1–2.
- **Facts already loaded:** 21,420 competition results (ranking of 2,721 brands) · Search Console by brand, 16 months
  · Google Trends since 2004 with a nightly queue for 8,091 brands · advertisers from BigQuery · online prices (Amazon
  in 7 countries, Google Shopping, Shopify catalogs) · tariffs for ~178 countries · 18,674 federal label approvals
  linking 1,831 US importers to brands · liquor licences from 37 US states (216k locations)
- **Satellite layer:** agave planting year and harvest per municipality, fed from the remote-sensing project above,
  with official-statistics comparison and an "under verification" flag published, not hidden
- **Front end:** 30-page prototype from a static generator: ECharts (colour-blind-safe palette, "view table"), 8
  Leaflet maps (choropleths, appellation atlas by municipality), live faceted search over 8,221 brands
- **Collection rules:** identified agent, robots.txt respected, no captcha bypass, no fake accounts; every paid API
  (DataForSEO) has a spend cap and a log

### 🎬 AI video studio — Huizapol Media
Children's educational series *La Libreta de Sasha* (Club de Curionautas, YouTube and TikTok): **3 episodes released**.
Same pipeline also produces political campaign video as a service.

- **Pipeline in Python:** sourced script → technical screenplay → director's board → character sheets → frames →
  clips → voice → music/SFX → mix → master → shorts, thumbnails, subtitles, chapters
- **Models chosen per shot:** image (gpt-image, Flux, Gemini, Qwen), video (Kling, Seedance, MiniMax, Wan), voice
  (ElevenLabs), lip-synced presenters in one continuous generation instead of stitched joins
- **Code over model** for anything that must be exact: typography, logos, legal straps and end cards rendered with
  ffmpeg
- **QA and cost:** automated checks of plan and cut against a written rulebook; approval gate before every paid step;
  cost cap per model and cost tracked per piece

---

## How I build

- **Agentic workspace:** one repo per business unit, each with a written brief and a living `STATUS.md`, so a person
  or an agent can pick it up cold. Two machines, sync rules, one agent per unit.
- **Tools, not one tool:** VS Code as the main editor; Claude Code with custom commands, skills, sub-agents and MCP
  servers (Notion, Gmail, browser); other coding agents and models when they fit the job better (OpenAI, Gemini,
  DeepSeek, open models). The model is chosen per task, by cost and by what it gets right.
- **Pre-register, then measure:** hypothesis, pass bar and kill bar written before the run.
- **Caps everywhere:** every paid API call has a ceiling and a log.
- **A person publishes:** pipelines prepare; a human approves anything sent, posted or traded beyond its cap.

## Stack

`Python` `asyncio` `FastAPI` `PostgreSQL` `SQLite` `Redis` `WebSockets` `Playwright` `rasterio/rioxarray` `scikit-learn`
`React` `TypeScript` `Astro` `ECharts` `Leaflet` `Linux/systemd` `Docker` `Nginx` `GitHub Actions` `Prometheus/Grafana`
`GoHighLevel` `SmartLead` `Apollo` `webhooks/REST` `Linear (coordination)`
`VS Code` `Claude Code` `MCP` `OpenAI` `Gemini` `DeepSeek` `multi-model LLM APIs` `ffmpeg` `ElevenLabs` `image & video generation APIs`

## Background

Founder, Smart Data Intelligence (2012–): national intelligence observatories for Mexico's Ministry of Energy, the World
Bank and the State of Jalisco. Master in Economic Intelligence (EGE Paris) · CS50x · ES / EN / FR

[LinkedIn](https://www.linkedin.com/in/marceljulien) · marcel.julien@sdintelligence.info
