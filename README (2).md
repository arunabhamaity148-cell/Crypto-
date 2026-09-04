# Binance Futures AI Bot — Capital-Protection-First

> ⚠️ **সর্বোচ্চ সতর্কতা**: এই সফটওয়্যারটি গবেষণা ও **paper trading** এর জন্য। **Live trading ডিফল্টভাবে বন্ধ থাকে**। ক্রিপ্টোকারেন্সি ফিউচার ট্রেডিং অত্যন্ত ঝুঁকিপূর্ণ; অতীত পারফরম্যান্স ভবিষ্যতের ফলাফলের গ্যারান্টি দেয় না। এই সিস্টেম সম্পূর্ণ ক্যাপিটাল হারাতে পারে।

---

## 📖 এই ডকুমেন্টটি কার জন্য?

এই README টি **বাংলা ভাষায়** লেখা হয়েছে **beginner** দের জন্য যারা:
- Python এবং ট্রেডিং বট সম্পর্কে প্রাথমিক জ্ঞান আছে
- Binance Futures এ অ্যাকাউন্ট আছে
- নিজের কম্পিউটার বা VPS এ বট চালাতে চান
- টাকা হারানোর ঝুঁকি নিয়ে live trading না করে আগে paper trading দিয়ে শিখতে চান

---

## 🎯 এই বটটি কী করে?

এটি একটি **capital-protection-first** অ্যালগরিদমিক ট্রেডিং বট যা Binance USDS-M Futures এ কাজ করে। মানে হলো:

- **প্রথম প্রায়োরিটি**: আপনার ক্যাপিটাল সুরক্ষিত রাখা
- **দ্বিতীয় প্রায়োরিটি**: লাভ করা

বটটি যা করে:
1. **Binance Futures** থেকে live মার্কেট ডেটা সংগ্রহ করে (REST + WebSocket)
2. **25টি ক্রিপ্টো পেয়ার** (BTC, ETH, SOL, BNB ইত্যাদি) পর্যবেক্ষণ করে
3. **13টি স্ট্র্যাটেজি** (6 production + 7 paper-only) দিয়ে ট্রেড সিগন্যাল তৈরি করে
4. **6-লেয়ার Veto Engine** দিয়ে খারাপ সিগন্যাল বাদ দেয়
5. **12 capital-protection circuits** দিয়ে ঝুঁকি নিয়ন্ত্রণ করে
6. **AI Review layer** (advisory-only) দিয়ে অতিরিক্ত সুরক্ষা দেয়
7. সব ট্রেড একটি **trade journal** এ সংরক্ষণ করে
8. **Telegram** এ alert পাঠায়
9. **FastAPI dashboard** এ রিয়েল-টাইম স্ট্যাটাস দেখায়

---

## 🛡️ কেন Paper Mode?

**Paper mode** এ বট:
- ✅ আসল Binance live মার্কেট ডেটা ব্যবহার করে
- ✅ সব সিগন্যাল তৈরি করে
- ✅ সব risk check করে
- ✅ সব journal entry লেখে
- ✅ Telegram alert পাঠায়
- ❌ **কিন্তু কোনো আসল অর্ডার Binance এ পাঠায় না**

**কেন গুরুত্বপূর্ণ:**
- বট প্রথমবার চালালে বাগ থাকতে পারে — paper mode এ টাকা যায় না
- স্ট্র্যাটেজি পরীক্ষা করার জন্য — 15 দিন পর্যন্ত ডেটা দরকার
- ঝুঁকি নিয়ন্ত্রণ যাচাই করার জন্য
- শেখার জন্য — কী হচ্ছে দেখতে পারবেন কোনো ঝুঁকি ছাড়া

---

## 📋 সিস্টেম রিকোয়ারমেন্ট

### আপনার কম্পিউটারে যা দরকার:

1. **Python 3.11 বা তার উপরে**
   - চেক: `python --version`
   - ইনস্টল: https://www.python.org/downloads/

2. **Internet সংযোগ** — Binance API তে access করার জন্য

3. **Binance অ্যাকাউন্ট** ( Spot বা Futures — যেকোনো একটি)
   - https://www.binance.com/ এ ফ্রি অ্যাকাউন্ট খুলুন
   - KYC কমপ্লিট করুন
   - Futures অ্যাকাউন্ট ওপেন করুন

4. **API Key** (পেপার মোডের জন্য read-only যথেষ্ট)
   - Binance → API Management → Create API
   - **গুরুত্বপূর্ণ**: "Withdrawal" permission বন্ধ রাখুন!
   - "Futures Trading" permission optional (paper mode তে দরকার নেই)

5. **Telegram Bot** (optional — alert পাওয়ার জন্য)
   - @BotFather এ নতুন বট তৈরি করুন
   - Bot token নিন
   - @userinfobot কে message দিয়ে আপনার chat_id জানুন

---

## 🚀 ইনস্টলেশন (Step-by-Step)

### Step 1: কোড ডাউনলোড করুন

এই ZIP ফাইলটি extract করুন যেকোনো ফোল্ডারে, যেমন:

```bash
cd /home/your_username/
unzip binance_futures_ai_bot_FINAL.zip -d binance_futures_ai_bot
cd binance_futures_ai_bot
```

### Step 2: Virtual Environment তৈরি করুন

Linux/Mac:
```bash
python3 -m venv .venv
source .venv/bin/activate
```

Windows:
```cmd
python -m venv .venv
.venv\Scripts\activate
```

### Step 3: Dependencies ইনস্টল করুন

```bash
pip install --upgrade pip
pip install -e ".[dev]"
```

এটি কিছুক্ষণ নেবে (5-10 মিনিট)।

### Step 4: Configuration ফাইল তৈরি করুন

```bash
cp .env.example .env
```

`.env` ফাইলটি একটি text editor দিয়ে খুলুন (যেমন nano, vim, বা VS Code):

```bash
nano .env
```

নিচের fields গুলো পূরণ করুন:

```env
# TRADING MODE — সবসময় paper রাখুন শুরুতে
TRADING_MODE=paper
LIVE_TRADING=false
LIVE_CONFIRMATION=

# BINANCE API (read-only key পেপার মোডের জন্য যথেষ্ট)
BINANCE_API_KEY=আপনার_বায়ন্যান্স_এপিআই_কী
BINANCE_API_SECRET=আপনার_বায়ন্যান্স_সিক্রেট
BINANCE_TESTNET=false
BINANCE_USE_PUBLIC_DATA=true

# PAPER CAPITAL
PAPER_INITIAL_CAPITAL_INR=200000
PAPER_INR_USD_RATE=83.0
PAPER_EQUITY_CURRENCY=USDT

# DATABASE (SQLite default — কোনো ইনস্টল করা লাগে না)
DATABASE_URL=sqlite:///./data/bot.db

# DASHBOARD
DASHBOARD_HOST=127.0.0.1
DASHBOARD_PORT=8080
DASHBOARD_USER=admin
DASHBOARD_PASS=একটি_শক্তিশালী_পাসওয়ার্ড_দিন

# TELEGRAM (optional — খালি রাখলে বট চলবে কিন্তু alert পাবেন না)
TG_BOT_TOKEN=আপনার_টেলিগ্রাম_বট_টোকেন
TG_CHAT_ID=আপনার_টেলিগ্রাম_চ্যাট_আইডি

# AI (MockAIProvider default — কোনো AI API দরকার নেই)
AI_PROVIDER=mock

# LOGGING
LOG_LEVEL=INFO
LOG_FORMAT=console
```

ফাইল সেভ করুন এবং বন্ধ করুন।

### Step 5: Database ইনিশিয়ালাইজ করুন

```bash
python -m bot.scripts.init_db --drop
```

আউটপুটে দেখবেন:
```
init_db_start
db_engine_ready url=sqlite
db_drop_all_tables
db_initialised
init_db_done
```

### Step 6: টেস্ট রান করুন (সব ঠিক আছে কিনা দেখুন)

```bash
pytest -q
```

আউটপুট:
```
============================= 89 passed in 11.30s ==============================
```

89 টেস্ট পাস করলে সব ঠিক আছে।

---

## 🏃 বট চালানো

### Step 7: Strategy Engine চালু করুন

টার্মিনাল 1 খুলুন এবং:

```bash
cd /home/your_username/binance_futures_ai_bot
source .venv/bin/activate
python -m bot.main
```

আউটপুটে দেখবেন:

```
=========================================
SAFE MODE
PAPER TRADING
REAL ORDERS DISABLED
=========================================

[info] boot_start
[info] db_engine_ready
[info] db_initialised
[info] server_time_synced offset_ms=-32
[info] universe_refreshed core=20 total=525
[info] strategy_loaded id=C1_FUNDING_HARVEST status=production
[info] strategy_loaded id=E1_OI_PRICE_INTERACTION status=production
... (13 strategies loaded)
[info] ws_starting streams=120
[info] market_data_started symbols=20
[info] paper_engine_started equity_usdt=2409.64
[info] tick btc_regime=NEUTRAL market_state=MIXED news_guard=CAUTION open_positions=0
```

বট এখন চলছে। প্রতি 15 সেকেন্ডে একটি `tick` হবে।

### Step 8: Position Monitor চালু করুন (আলাদা টার্মিনালে)

টার্মিনাল 2 খুলুন:

```bash
cd /home/your_username/binance_futures_ai_bot
source .venv/bin/activate
python -m bot.position_monitor_main
```

এটি একটি **আলাদা প্রসেস** যা strategy engine ক্র্যাশ করলেও চালু থাকে এবং open positions protect করে।

### Step 9: Dashboard চালু করুন (আলাদা টার্মিনালে)

টার্মিনাল 3 খুলুন:

```bash
cd /home/your_username/binance_futures_ai_bot
source .venv/bin/activate
uvicorn bot.dashboard.app:app --host 127.0.0.1 --port 8080
```

ব্রাউজারে যান: http://127.0.0.1:8080

Username: `admin`
Password: `.env` এ যেটা দিয়েছেন সেটা

---

## 📊 Dashboard এ কী দেখবেন

- **Trading mode**: paper (সবসময় paper হবে ডিফল্টে)
- **Live trading**: false
- **AI provider**: mock
- **Open positions**: 0 (শুরুতে কোনো position নেই)
- **Last health**: position monitor এর heartbeat

---

## 🛑 বট বন্ধ করা

প্রতিটি টার্মিনালে `Ctrl+C` চাপুন।

বট বন্ধ হবে। ডেটা `data/bot.db` (SQLite) ফাইলে সেভড থাকে।

---

## ⚠️ সবচেয়ে গুরুত্বপূর্ণ: LIVE MODE

**LIVE TRADING করবেন না যতক্ষণ না:**

1. ✅ 15 দিন paper trading কমপ্লিট হয়
2. ✅ System Acceptance Gate এর 17 criteria সব পাস হয়
3. ✅ Phase 14 Red Team review পাস হয়
4. ✅ আপনি নিজে ম্যানুয়ালি review করেছেন
5. ✅ আপনি কমপক্ষে ₹10,000 ছোট ক্যাপিটাল দিয়ে 30 দিন micro-live করেছেন

**LIVE MODE চালু করতে হলে** তিনটি শর্ত পূরণ করতে হবে (একসাথে):

```env
TRADING_MODE=live
LIVE_TRADING=true
LIVE_CONFIRMATION=I_UNDERSTAND
```

এবং `.env` এ আসল Binance API key (Futures trading permission সহ) থাকতে হবে।

---

## 🔒 নিরাপত্তা (Security) — অবশ্যই পড়ুন

### 1. API Key নিয়ম

- **কখনো** API key কে chat, email, বা public place এ শেয়ার করবেন না
- **সবসময়** IP allowlist চালু রাখুন (শুধু আপনার VPS IP)
- **কখনো** Withdrawal permission চালু রাখবেন না
- **Quarterly** key rotate করুন
- কোনো incident হলে দ্রুত revoke করুন

### 2. .env ফাইল

- `.env` ফাইল কখনো Git এ commit করবেন না (`.gitignore` এ আগে থেকেই আছে)
- `.env` ফাইলের permission `chmod 600` করুন (শুধু আপনি পড়তে পারবেন)
- Backup রাখবেন একটি encrypted password manager এ (যেমন Bitwarden, 1Password)

### 3. যদি আপনার API key চুরি হয়ে যায়

1. **অবিলম্বে** Binance এ লগইন করুন
2. API Management → আপনার API key টি delete করুন
3. নতুন API key তৈরি করুন
4. সব উইথড্রয়াল disable করুন
5. যদি কোনো সন্দেহজনক অর্ডার দেখেন — সাথে সাথে Binance support কে জানান

### 4. VPS নিরাপত্তা

- SSH key-only login (password দিয়ে login disable করুন)
- Firewall (UFW) চালু রাখুন
- `unattended-upgrades` চালু রাখুন
- শুধু প্রয়োজনীয় port খুলুন

---

## 🆘 Common Issues ও Solutions

### Issue 1: "Binance API error code=-1003 way too many requests"

**কারণ**: আপনার IP Binance থেকে rate-limit হয়ে গেছে।

**সমাধান**:
- 15 মিনিট অপেক্ষা করুন
- অথবা `BINANCE_TESTNET=true` দিয়ে testnet এ চালান
- অথবা অন্য VPS / IP থেকে চালান

### Issue 2: "LIVE_TRADING=true but BINANCE_API_KEY/SECRET are empty"

**কারণ**: Live mode চালু করেছেন কিন্তু API key দেননি।

**সমাধান**:
- যদি paper এ চালাতে চান: `LIVE_TRADING=false` রাখুন
- যদি live এ চালাতে চান: সঠিক API key + secret দিন

### Issue 3: "Database not initialized"

**সমাধান**:
```bash
python -m bot.scripts.init_db --drop
```

### Issue 4: Telegram এ message পাচ্ছি না

**সমাধান**:
1. `TG_BOT_TOKEN` এবং `TG_CHAT_ID` সঠিক দিন
2. @BotFather এ বট start করুন
3. Chat ID যাচাই করুন: `https://api.telegram.org/bot<TOKEN>/getUpdates`

### Issue 5: Dashboard 401 Unauthorized

**কারণ**: ভুল password।

**সমাধান**:
- `.env` এ `DASHBOARD_USER` এবং `DASHBOARD_PASS` যাচাই করুন
- ডিফল্ট `admin` / `changeme` — অবশ্যই পরিবর্তন করুন!

### Issue 6: Bot চালু হচ্ছে না, কোনো error নেই

**সমাধান**:
- `pytest` চালান দেখুন সব test পাস করে কিনা
- `LOG_LEVEL=DEBUG` দিন `.env` এ
- Log ফাইল দেখুন: `journalctl -u bot-strategy -f` (systemd হলে)
- বা `tail -f /tmp/bot_run.log` (nohup হলে)

---

## 📚 কী কী আছে এই প্রজেক্টে?

### 13 Strategy

| Status | Strategy | কী করে |
|---|---|---|
| Production | C1_FUNDING_HARVEST | যখন funding rate অনেক বেশি, short করে funding income কাটে |
| Production | E1_OI_PRICE_INTERACTION | Open Interest ও price এর relationship দেখে |
| Production | G1_HMM_REGIME | BTC regime classify করে (filter only) |
| Production | G2_BTC_REGIME_GATE | BTC regime অনুযায়ী alt trade allow/block |
| Production | G3_EVENT_DEFENSIVE | FOMC/CPI এর আগে নতুন ট্রেড ব্লক |
| Production | G4_PORTFOLIO_VOL_TARGET | Portfolio volatility নিয়ন্ত্রণ |
| Paper-only | A1_TSMOM | Time-series momentum |
| Paper-only | A4_CROSS_SECTIONAL_MOMENTUM | Cross-sectional momentum |
| Paper-only | B1_RSI_BB_MEAN_REVERSION | RSI + Bollinger mean reversion |
| Paper-only | B3_RANGE_TRADING | Range boundary trading |
| Paper-only | D1_DONCHIAN_BREAKOUT | Donchian channel breakout |
| Paper-only | D2_VOLATILITY_SQUEEZE | Volatility compression → breakout |
| Paper-only | E4_CVD_DIVERGENCE | CVD divergence (placeholder v1) |

### 6-লেয়ার Veto Engine

1. **Market** — BTC danger, market crash, liquidation cascade
2. **Technical** — invalid structure, poor R:R, overextension
3. **Derivatives** — extreme funding, OI divergence, crowded positioning
4. **Execution** — wide spread, insufficient depth, stale data
5. **News** — critical macro event, Binance incident, delisting
6. **Portfolio** — excessive correlation, max positions, daily loss limit

### 12 Capital-Protection Circuits

1. Account Kill Switch (ম্যানুয়াল)
2. Daily Loss Limit (-1.0%)
3. Weekly Loss Limit (-2.5%)
4. Max Drawdown Soft (-5%) / Hard (-8%)
5. Max Concurrent Positions (3)
6. Max Correlated Exposure (BTC-beta < 1.0)
7. Max Single-Trade Risk
8. Liquidation Buffer Protection (≥ 3× stop distance)
9. API Failure Protection
10. Data Staleness Protection
11. News Emergency Mode
12. Exchange Incident Mode

### Risk Settings (v1)

- Risk per trade: **0.25%** of equity
- Max concurrent positions: **3**
- Max leverage: **4×** (hard ceiling)
- Daily loss limit: **-1.0%**
- Hard drawdown: **-8%** → emergency close

---

## 💰 খরচ (Cost)

### ফ্রি (মোটেই খরচ নেই)

- Python, FastAPI, pandas, NumPy, SciPy, scikit-learn, pytest, ruff, bandit
- SQLite database
- Telegram Bot API
- Binance public market data + WebSocket
- MockAIProvider (কোনো AI API দরকার নেই)
- Federal Reserve / BLS macro data

### যা খরচ করতে হবে (VPS)

- **Recommended**: Hetzner CCX23 (4 vCPU / 8 GB RAM) — ~₹1,500/মাস
- অথবা নিজের কম্পিউটারে চালালে — ফ্রি (কিন্তু 24×7 চলবে না)

### Optional (পেপার মোডে দরকার নেই)

- OpenAI / Anthropic API — AI Review ভালো করার জন্য
- Perplexity Pro / Genspark — গবেষণার জন্য
- Premium data (Kaiko, Tardis.dev)
- Managed PostgreSQL

---

## 📅 আপনার যাত্রা — Step-by-Step

### Week 1-2: Setup ও শেখা

- [ ] ZIP extract করুন
- [ ] `.env` setup করুন (paper mode)
- [ ] Database init করুন
- [ ] Tests চালান (89 পাস করবে)
- [ ] বট চালু করুন
- [ ] Dashboard দেখুন
- [ ] Telegram message পান

### Week 3-4: পর্যবেক্ষণ

- [ ] প্রতিদিন 15 মিনিট dashboard দেখুন
- [ ] প্রতিদিন daily report Telegram এ পড়ুন
- [ ] কোনো error আছে কিনা লক্ষ্য করুন
- [ ] `docs/KNOWN_LIMITATIONS.md` পড়ুন

### Week 5-15: Paper Trading

- [ ] 15 দিন continuous run
- [ ] `python -m bot.scripts.paper_stop` চালান
- [ ] System Acceptance Gate 17 criteria যাচাই করুন
- [ ] যদি সব পাস হয় → Phase 14 (Red Team)
- [ ] যদি কোনো fail হয় → fix করুন, আবার 15 দিন

### Week 16+: Micro-Live (অত্যন্ত সাবধানে)

- [ ] শুধু যদি paper pass হয়
- [ ] ছোট ক্যাপিটাল (₹10,000) দিয়ে শুরু
- [ ] 30 দিন পর্যবেক্ষণ
- [ ] Max drawdown ≤ 3% থাকলে → Phase 16 (scaling)

---

## 📖 অতিরিক্ত ডকুমেন্ট

`docs/` ফোল্ডারে থাকা ফাইলগুলো পড়ুন:

| File | কী আছে |
|---|---|
| `ARCHITECTURE.md` | সিস্টেম ডিজাইন |
| `INSTALLATION.md` | ইনস্টলেশন গাইড |
| `DEPLOYMENT.md` | VPS / Docker ডিপ্লয়মেন্ট |
| `SECURITY.md` | নিরাপত্তা গাইড |
| `RISK_POLICY.md` | Risk controls এর বিস্তারিত |
| `STRATEGY_SPECIFICATION.md` | স্ট্র্যাটেজি লাইব্রেরি |
| `BINANCE_API_COMPATIBILITY.md` | Binance API reference |
| `PAPER_TRADING.md` | 15-day paper validation |
| `TESTING.md` | টেস্ট স্যুট |
| `TROUBLESHOOTING.md` | Common issues |
| `OPERATIONS.md` | Runbook |
| `IMPLEMENTATION_DECISIONS.md` | Engineering decisions |
| `COST_AUDIT.md` | খরচের হিসাব |
| `KNOWN_LIMITATIONS.md` | সীমাবদ্ধতা সমূহ |
| `FINAL_AUDIT.md` | ফাইনাল অডিট |
| `CHANGELOG.md` | পরিবর্তনের ইতিহাস |

---

## ❓ সাধারণ প্রশ্ন (FAQ)

**Q: আমি কি এই বট দিয়ে টাকা কামাতে পারব?**

A: সম্ভব না। ক্রিপ্টো ট্রেডিং অত্যন্ত ঝুঁকিপূর্ণ। এই বট আপনাকে টাকা হারানো থেকে রক্ষা করার চেষ্টা করে, কিন্তু লাভ গ্যারান্টি দেয় না। কোনো legitimate ট্রেডিং সিস্টেমই লাভ গ্যারান্টি দেয় না।

**Q: কত টাকা দিয়ে শুরু করব?**

A: Paper mode এ শূন্য টাকা (virtual ₹2,00,000) দিয়ে শুরু করুন। Live এ গেলে সর্বোচ্চ ₹10,000 দিয়ে শুরু করুন (যা হারালেও কিছু যায় না)।

**Q: কত সময় পর্যন্ত paper mode এ রাখব?**

A: অন্তত 15 দিন। যদি সব criteria পাস হয়, তারপরেও 30 দিন micro-live ছোট ক্যাপিটালে। এরপরেও সাবধানে সাবধানে।

**Q: AI দরকার কি?**

A: না। MockAIProvider default — কোনো AI API দরকার নেই। বট চলবে। যদি ভালো AI চান, তবে Ollama (local, ফ্রি) বা OpenAI/Anthropic API (paid) ব্যবহার করতে পারেন।

**Q: Binance Testnet vs Mainnet?**

A: Testnet = virtual টাকা, নিরাপদ। Mainnet = আসল টাকা, ঝুঁকিপূর্ণ। শুরুতে Testnet এ চালান। আমাদের paper mode mainnet data ব্যবহার করে কিন্তু virtual execution করে — সেরা উভয় দুনিয়া।

**Q: VPS কোথায় নেব?**

A: Hetzner (সস্তা, ভালো), Vultr, DigitalOcean। AWS/GCP ও ভালো কিন্তু দামি। ভারতের VPS (যেমন Hostinger) ও চলবে কিন্তু latency বেশি।

**Q: আমার IP Binance থেকে banned হয়ে গেছে, কী করব?**

A: 15 মিনিট অপেক্ষা করুন। যদি আবার হয়, `REST_WEIGHT_BUDGET_PER_MIN` কমান (যেমন 1200)।

---

## 📞 সাপোর্ট

এই বট একটি গবেষণা প্রজেক্ট। কোনো প্রতিষ্ঠান এটি support করে না। সমস্যা হলে:

1. `docs/TROUBLESHOOTING.md` পড়ুন
2. `docs/KNOWN_LIMITATIONS.md` পড়ুন
3. GitHub issues চেক করুন
4. Stack Overflow তে প্রশ্ন করুন

---

## ⚖️ লাইসেন্স

MIT License। ফ্রি ব্যবহার, পরিবর্তন, বিতরণ করতে পারেন। কিন্তু কোনো warranty নেই।

---

## ⚠️ চূড়ান্ত সতর্কতা

> **এই সফটওয়্যার "AS IS" দেওয়া হচ্ছে, কোনো ওয়ারেন্টি ছাড়া। ক্রিপ্টোকারেন্সি ফিউচার ট্রেডিং অত্যন্ত ঝুঁকিপূর্ণ এবং আপনার সম্পূর্ণ ক্যাপিটাল হারাতে পারে। যে টাকা হারালে আপনার সমস্যা হবে, সেই টাকা দিয়ে ট্রেড করবেন না।**

**Capital protection first. Profit is secondary.**
**ক্যাপিটাল সুরক্ষা প্রথম। লাভ গৌণ।**
