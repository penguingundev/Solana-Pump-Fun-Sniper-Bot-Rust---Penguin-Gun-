Pump.fun Sniper & Validation Bot — Written in Rust

Penguin Gun is a high-speed Pump.fun sniper bot built in Rust, designed with a strict validation-first philosophy.
Before trading even becomes an option, every token must pass multiple layers of real-time, on-chain validation.

The goal is simple:
filter noise, detect real momentum, and avoid traps before the market reacts.

Some features of Penguin Gun;



🧠 Architecture Overview

Penguin Gun is built in phases, allowing safe testing, modular expansion, and production-grade execution.

Phase 1 — Validation Only (SNIPE V2 CLEAN)
	•	❌ No database
	•	❌ No trading
	•	❌ No dashboard

✅ Only:
	•	Real-time token detection
	•	6 primary validation checks
	•	Console-based logging

This phase exists purely to observe, validate, and learn.

⸻

⚡ Core Features

🔍 Real-Time Token Detection
	•	Instantly detects newly created Pump.fun tokens
	•	Powered by Helius WebSocket & RPC
	•	Optimized for low latency and fast reaction time

⸻

🧪 Primary Validation System (V1–V6)

All validations are modular and can be enabled or disabled individually.

V1 — First Buy Amount Validation

Ensures the creator’s first buy is within a defined token range.
Helps filter:
	•	Fake launches
	•	Abnormal supply manipulation

⸻

V2 — Creator SOL Balance Check

Validates the creator wallet’s SOL balance.
Avoids:
	•	Fresh burner wallets
	•	Underfunded scam deployers

⸻

V3 — Wallet Age Validation

Checks how old the creator wallet is (in hours).
Useful for identifying:
	•	Newly created wallets
	•	Short-lived rug wallets

⸻

V4 — Bundler Detection (Slot-Based)

Detects Jito-style bundler activity by monitoring:
	•	Multiple buys in the same slot
	•	Abnormally coordinated transactions

If bundler behavior exceeds limits → validation fails.

⸻

V5 — URI Metadata Match (V2 Scoring System)

A score-based system that evaluates:
	•	Existence of Twitter, Website, Telegram
	•	Whether token name matches metadata content

Includes:
	•	Existence points
	•	Name-match bonus points
	•	Configurable minimum score threshold

This reduces low-effort and copy-paste scam launches.

⸻

V6 — Unique Buy / Sell Transaction Analysis

Monitors early trading activity within a time window:
	•	Minimum unique buyers
	•	Minimum unique sellers
	•	Filters micro-spam buys
	•	Detects early dump behavior

Includes advanced bundler detection based on the first transactions after creation.

⸻

🧩 Secondary Validations (Optional)

Secondary validations run only if all primary validations pass.

DexPaid Validation (DexScreener)

Checks whether the token creator has paid for DexScreener services.
This is a strong legitimacy signal, as scam tokens rarely pay for promotion.

Includes:
	•	Minimum paid service requirement
	•	Optional polling system
	•	Instant buy trigger if payment gets approved

⸻

🔮 Future Validators (Planned)
	•	Minimum liquidity checks
	•	Holder distribution analysis
	•	Additional on-chain behavior heuristics

⸻

💸 Trading Engine (Phase 2 – Optional)

Trading is disabled by default.

🧪 Dry Run Mode
	•	Simulates all transactions
	•	No SOL is spent
	•	Mandatory for testing and strategy validation

⸻

📊 Risk & Position Management
	•	Max position size per trade
	•	Max concurrent trades
	•	Slippage control with retry logic
	•	Liquidity thresholds before entry

⸻

🚪 Exit Strategy System

Supports multiple exit mechanisms:
	•	Stop loss
	•	Take profit
	•	Trailing stop
	•	Maximum hold duration
	•	Minimum hold time (prevents instant stop-outs)

Volume-Based Exit (Optional)

Exits positions if:
	•	Volume stalls
	•	Buyer momentum disappears

Emergency Exit

Instant exit on extreme price drops within a short time window.

⸻

🔄 Buy / Sell Retry Logic

Production-safe retry system:
	•	Incremental slippage widening
	•	Metadata timeouts
	•	Separate retry logic for buys and sells

Designed to handle:
	•	Network congestion
	•	RPC delays
	•	Partial transaction failures

⸻

🌊 Flow Detector (Experimental)

An alternative validation path that races with DexPaid:
	•	Hype detection
	•	Alive flow detection
	•	No-flow rejection
	•	Dump detection

Whichever passes first wins.

⸻

📣 Telegram Integration
	•	Real-time notifications
	•	Token validation results
	•	Trade execution alerts (when enabled)

⸻

🧊 Philosophy

Penguin Gun exists because being early is not enough.
You must validate faster than others can react.

It’s not about shooting often.
It’s about pulling the trigger once — at the right moment.



#  
# SNIPE V2 CLEAN - PURE VALIDATION SYSTEM
 Phase 1: No database, no trading, no dashboard
# Only: Token detection + 6 validation checks + console logging

 # HELIUS API CONFIGURATION
 HELIUS_API_KEY=xxxxxx

 # VALIDATION$1: FIRST BUY AMOUNT (Token amounts)
 # Creator's first buy must be within this range
FIRST_BUY_MIN_TOKENS=30000000
FIRST_BUY_MAX_TOKENS=75000000

 # VALIDATION$2: CREATOR BALANCE (SOL)
 # Creator's wallet SOL balance must be within this range
CREATOR_MIN_SOL=1
CREATOR_MAX_SOL=5000

 # VALIDATION$3: WALLET AGE (hours)
 # Creator's wallet age must be within this range
WALLET_MIN_AGE_HOURS=0.0
WALLET_MAX_AGE_HOURS=24

 # VALIDATION$4: BUNDLER DETECTION (buys per slot)
 # If same slot has >= this many buys → BUNDLER detected (validation fails)
BUNDLER_MAX_SAME_SLOT=10

 # VALIDATION$5: URI METADATA MATCH (V2 - Existence + Match Bonus)
 # EXISTENCE POINTS (URL just being present)
URI_TWITTER_EXIST_SCORE=2   $ Points for having a twitter URL
URI_WEBSITE_EXIST_SCORE=2   $ Points for having a website URL
URI_TELEGRAM_EXIST_SCORE=1  $ Points for having a telegram URL

# MATCH BONUS (name matches the URL content)
URI_MATCH_TWITTER_BONUS=3   $ Bonus if token name matches twitter handle
URI_MATCH_WEBSITE_BONUS=3   $ Bonus if token name matches website domain
URI_MATCH_DESC_BONUS=1      $ Bonus if token name in description

# TOTAL SCORE = Existence + Match Bonus
# Example: website exists (2) + name matches (3) = 5 points total
URI_MATCH_THRESHOLD=2       $ Minimum total score required to pass

# Example scenarios:
# - Twitter + Website exists = 2+2 = 4 pts (PASS with threshold=3)
# - Twitter exists + name matches = 2+3 = 5 pts (PASS)
# - Website exists + Website matches = 2+3 = 5 pts (PASS)
# - Only description match = 1 pt (FAIL with threshold=3)

 # VALIDATION$6: UNIQUE BUY/SELL TRANSACTIONS (monitoring window)
 UNIQUE_BUY_MIN=100              $ Minimum unique buyers required (bundler no)
UNIQUE_SELL_MIN=15             $ Minimum unique sellers required
UNIQUE_WINDOW_SECS=60        $ Observation window (seconds)
UNIQUE_MAX_RETRIES=4          $ Max WebSocket reconnection attempts
UNIQUE_MIN_BUY_SOL=0.05       $ Minimum buy size in SOL (filters micro spam)
UNIQUE_EARLY_SELL_WINDOW_SECS=0 $ Early dump detection threshold (seconds)

# 🆕 BUNDLER DETECTION (RPC-based, from first 20 TXs after create)
UNIQUE_MAX_BUNDLER=2         $ Maximum allowed bundler wallets (total)
# Bundler = multiple wallets buying in same slot (Jito bundle pattern)
# Example: If ≥3 bundler wallets detected in first 20 TXs → REJECT

 # PRIMARY VALIDATION ENABLE/DISABLE TOGGLES (V1-V6)
 # Set to "true" to ENABLE validation, "false" to DISABLE
# All enabled validations must pass for a token to be considered valid

REQUIRE_V1_FIRST_BUY=false      $ V1: First buy amount check
REQUIRE_V2_BALANCE=false        $ V2: Creator balance check
REQUIRE_V3_WALLET_AGE=false     $ V3: Wallet age check
REQUIRE_V4_BUNDLER=false        $ V4: Bundler detection (no bundler = pass)
REQUIRE_V5_URI_MATCH=false      $ V5: URI metadata match
REQUIRE_V6_UNIQUE_TX=true       $ V6: Unique transactions check
 
 
 # 🆕 SECONDARY VALIDATIONS (After Primary V1-V6 Pass)
 # Secondary validations run ONLY if primary validations pass
# Modular system - easy to add new validators

# Master toggle for all secondary validations
ENABLE_SECONDARY_VALIDATIONS=false

# DexPaid Check (DexScreener paid services)
# Checks if token creator paid for DexScreener promotional services
# This is a strong signal of legitimacy (scam tokens rarely pay for ads)
REQUIRE_DEXPAID=true
DEXPAID_MIN_SERVICES=1          $ Minimum paid services required (1 = any paid service)

# 🆕 DexPaid Polling Configuration (waits for payment approval)
# After primary validations pass, poll DexScreener API for payment status
# If status changes to "approved" → INSTANT BUY
DEXPAID_POLLING_ENABLED=true    $ Enable polling (wait for payment)
DEXPAID_POLLING_DURATION_SECS=120$ Maximum wait time (30 seconds)
DEXPAID_POLLING_INTERVAL_SECS=5 $ Check every 2 seconds
# Example: Poll for 30s, check every 2s = max 15 checks
# If "approved" found → INSTANT BUY
# If timeout → REJECT

# 🔮 FUTURE: Additional secondary validators 
# REQUIRE_MIN_LIQUIDITY=false
# MIN_SOL_LIQUIDITY=50.0
#
# REQUIRE_HOLDER_CHECK=false
# MIN_HOLDERS=100

 # 🆕 PHASE 2: TRADING ENGINE CONFIGURATION
 # WARNING: Trading is DISABLED by default. Set ENABLE_TRADING=true to activate.

 # TRADING TOGGLE
 ENABLE_TRADING=false           $ Master switch for trading (false = validation only)

# 🧪 PHASE 5.2: DRY RUN MODE (CRITICAL FOR TESTING)
 # DRY_RUN=true  → Simulates ALL transactions WITHOUT spending SOL
# DRY_RUN=false → Real transactions on mainnet
# ⚠️  ALWAYS TEST WITH DRY_RUN=true FIRST!
#
# PHASE 6.1: Starting with D    RY_RUN for flow testing
DRY_RUN=false

 # POSITION SIZE & RISK MANAGEMENT
 MAX_POSITION_SIZE=0.04         $ Maximum SOL per position (0.5 SOL max)
MAX_CONCURRENT_TRADES=1        $ Maximum number of open positions at once
MIN_LIQUIDITY_SOL=10           $ Minimum liquidity required (10 SOL)
DEFAULT_BUY_AMOUNT_SOL=0.04   $ Default buy amount (0.1 SOL)
SLIPPAGE_TOLERANCE_BPS=500     $ Slippage tolerance (500 = 5%)
PRIVATE_KEY=xxxxxxxxxx

 # HELIUS SENDER / JITO CONFIGURATION (PHASE 6 - Future)
 USE_JITO_FOR_ENTRY=false       $ Use Jito for entry transactions (PHASE 6)
USE_JITO_FOR_EXIT=false        $ Use Jito for exit transactions (PHASE 6)

SENDER_REGION=US_EAST          $ Region: US_EAST, US_SLC, EU_WEST, EU_CENTRAL, EU_NORTH, AP_SINGAPORE, AP_TOKYO
HELIUS_SENDER_TIP_LAMPORTS=1000000 $ Tip for Helius Sender (0.0002 SOL = 200,000 lamports) - Used for entry/exit
SENDER_MIN_TIP_LAMPORTS=1000000   $ Minimum tip for Helius Sender (0.000005 SOL = 5000 lamports)
MIN_TIP_LAMPORTS=10000000       $ Minimum tip (1,000,000 = 0.001 SOL)
AUTO_TIP_FROM_FLOOR=true       $ Auto-fetch 75th percentile tip floor from Jito

 
 # EXIT STRATEGY CONFIGURATION
 STOP_LOSS_PCT=50               $ Stop loss at -20% PnL
TAKE_PROFIT_PCT=50            $ Take profit at +1000% PnL
TRAILING_STOP_PCT=25            $ Trailing stop at -20% from peak
MAX_HOLD_DURATION_SECS=5     $ Maximum hold time (3600s = 1 hour)

# ✅ NEW: Minimum hold time before stop loss can trigger
# This prevents immediate exit during initial price discovery (first 30-60s)
# Pump.fun tokens are ULTRA volatile in the first minute - price needs time to stabilize
MIN_HOLD_TIME_SECS=3          $ Don't exit via stop loss before 30 seconds

 # VOLUME-BASED EXIT STRATEGY (NEW!)
 # Aktif pozisyonlarda hacim takibi - yetersiz hacimde anında exit
VOLUME_EXIT_ENABLED=false
VOLUME_EXIT_CHECK_INTERVAL_SECS=2       $ Her 10 saniyede kontrol
VOLUME_EXIT_MIN_UNIQUE_BUYERS_DELTA=2   $ Bu aralıkta en az +1 unique buyer
VOLUME_EXIT_MAX_FAILS=2                 $ 2 kez fail => exit

 # EMERGENCY EXIT CONFIGURATION
 EMERGENCY_EXIT_ENABLED=false    $ Enable emergency exit on extreme price drops
EMERGENCY_EXIT_DROP_PCT=50     $ Emergency exit at -50% PnL
EMERGENCY_EXIT_TIME_SECS=30    $ Exit within 30 seconds if triggered

 # MONITORING CONFIGURATION
 PRICE_CHECK_INTERVAL_MS=2000   $ Check price every 2 seconds

 # BUY/SELL RETRY CONFIGURATION (Production-Safe Trading)
 # BUY retry settings
BUY_RETRY_COUNT=3                      $ Max buy retry attempts
BUY_SLIPPAGE_INCREMENT_BPS=100         $ +100bps per retry (1% extra)
BUY_METADATA_TIMEOUT_SECS=3            $ Timeout for fetching buy metadata

# SELL retry settings
SELL_RETRY_COUNT=3                     $ Max sell retry attempts
SELL_SLIPPAGE_WIDEN_BPS=200           $ +200bps per retry (2% extra)

# ⚠️ DEPRECATED: These are not currently used in the code
# SELL_MAX_SLIPPAGE_BPS=1500            $ Cap at 1500bps (15%)
# SELL_CONFIRM_TIMEOUT_MS=10000         $ 10s confirmation timeout
# SELL_RETRY_COOLDOWN_MS=2000           $ 2s between retries
# SELL_CONFIRM_COMMITMENT=confirmed      $ "confirmed" or "finalized"
# PENDING_EXIT_TIMEOUT_SECS=20            $ 20s timeout for pending exit verification

 # FLOW DETECTOR - Buy/Sell Activity Monitoring
 # Enable flow detector (races with DexPaid - first pass wins!)
FLOW_DETECTOR_ENABLED=false

# Polling settings
FLOW_DETECTOR_POLLING_INTERVAL_SECS=2
FLOW_DETECTOR_POLLING_DURATION_SECS=10

# HYPE Detection (instant pass)
FLOW_HYPE_WINDOW_SECS=30
FLOW_HYPE_MIN_BUYERS=15
FLOW_HYPE_MIN_BUY_SOL=0.1
FLOW_HYPE_MAX_SELLERS=0

# ALIVE Detection (normal pass)
FLOW_ALIVE_MIN_BUYERS=10
FLOW_ALIVE_MIN_BUY_SOL=0.7
FLOW_ALIVE_MAX_SELLERS=1

# NO_FLOW Detection (reject)
FLOW_NO_FLOW_MAX_BUYERS=2
FLOW_NO_FLOW_MAX_BUY_SOL=0.3

# DUMP Detection (reject)
FLOW_DUMP_MAX_SELLERS=2
FLOW_DUMP_SELL_RATIO=0.6

 # TELEGRAM BOT CONFIGURATION
 # Enable/disable telegram notifications
TELEGRAM_ENABLED=true

# Telegram bot token (get from @BotFather)
# Example: 123456789:ABCdefGHIjklMNOpqrsTUVwxyz
TELEGRAM_BOT_TOKEN=xxxxxx

# Telegram chat ID (your user ID or group ID)
# Get it from @userinfobot or @getidsbot
# Example: 123456789 (positive for private chat)
TELEGRAM_CHAT_ID=xxxxx
