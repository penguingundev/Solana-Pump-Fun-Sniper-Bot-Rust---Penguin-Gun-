 Architecture Overview

Penguin Gun is built in phases, allowing safe testing, modular expansion, and production-grade execution.

Phase 1 — Validation Only (SNIPE V2 CLEAN)
	•	❌ No database
	•	❌ No trading
	•	❌ No dashboard

 Only:
	•	Real-time token detection
	•	6 primary validation checks
	•	Console-based logging

This phase exists purely to observe, validate, and learn.

⸻

⚡ Core Features

 Real-Time Token Detection
	•	Instantly detects newly created Pump.fun tokens
	•	Powered by Helius WebSocket & RPC
	•	Optimized for low latency and fast reaction time

⸻

 Primary Validation System (V1–V6)

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

 Secondary Validations 

Secondary validations run only if all primary validations pass.

DexPaid Validation (DexScreener)

Checks whether the token creator has paid for DexScreener services.
This is a strong legitimacy signal, as scam tokens rarely pay for promotion.

Includes:
	•	Minimum paid service requirement
	•	Optional polling system
	•	Instant buy trigger if payment gets approved

⸻

 Future Validators (Planned)
	•	Minimum liquidity checks
	•	Holder distribution analysis
	•	Additional on-chain behavior heuristics

⸻

 Trading Engine (Phase 2 – Optional)

Trading is disabled by default.

 Dry Run Mode
	•	Simulates all transactions
	•	No SOL is spent
	•	Mandatory for testing and strategy validation

⸻

 Risk & Position Management
	•	Max position size per trade
	•	Max concurrent trades
	•	Slippage control with retry logic
	•	Liquidity thresholds before entry

⸻

 Exit Strategy System

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

 Buy / Sell Retry Logic

Production-safe retry system:
	•	Incremental slippage widening
	•	Metadata timeouts
	•	Separate retry logic for buys and sells

Designed to handle:
	•	Network congestion
	•	RPC delays
	•	Partial transaction failures

⸻

 Flow Detector 

An alternative validation path that races with DexPaid:
	•	Hype detection
	•	Alive flow detection
	•	No-flow rejection
	•	Dump detection

Whichever passes first wins.

⸻

 Telegram Integration
	•	Real-time notifications
	•	Token validation results
	•	Trade execution alerts (when enabled)

⸻

 Philosophy

Penguin Gun exists because being early is not enough.
You must validate faster than others can react.

It’s not about shooting often.
It’s about pulling the trigger once — at the right moment.
