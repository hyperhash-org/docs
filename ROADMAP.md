HYPER HASH NETWORK — FULL EXECUTION ROADMAP (VERSION 0)
1. MISSION

Hyper Hash Network is a decentralized Bitcoin-mining ecosystem that merges Stratum V2, the Lightning Network, and distributed Template Providers into a coordinated pool-of-pools architecture.

Every Hyper Node — whether public or private — runs its own Lightning wallet, participates in mining, settlement, and liquidity routing, and communicates securely with the Hyper Hash Core.

2. GLOBAL ARCHITECTURE
2.1 Core Plane

The Core Plane is the foundation of the network and contains the following components:

• Bitcoin Node Cluster — Dual (archival + pruned) nodes feeding templates to the Template Providers.
• Template Provider (hh-tp) — Generates Stratum V2 templates; horizontally scalable behind a load-balanced VIP.
• Pool Server (hh-pool) — Manages SV2 session negotiation, share accounting, and Lightning-based payout logic.
• Translator (hh-translator) — Provides compatibility between SV1 and SV2 miners.
• Core API (hyperhash-core) — REST and gRPC gateway providing all control and telemetry endpoints.
• Treasury (hh-treasury) — Manages the 1% pool fee split, operating on Lightning and on-chain rails.
• UI (hh-ui) — Web interface for miners, operators, and node owners.
• Observability (hh-observability) — Prometheus, Grafana, and Loki stack for metrics, logging, and alerts.

2.2 Hyper Node Layer

Each Hyper Node is a self-contained mining edge that performs local template generation, Lightning settlement, and telemetry reporting.

Public and Private Nodes both run the following stack:
hh-node-agent, hh-tp, hh-translator, bitcoind, and lnd or cln (Lightning Daemon).

Requirements for all nodes:
• Must maintain at least one Lightning channel to the Core Treasury.
• Must report Lightning telemetry to Core.
• May re-invest or withdraw Lightning yields via MMI (Management and Monitoring Interface).

Public Hyper Nodes are open to external miners.
Private Hyper Nodes are self-hosted by partners or advanced users.

2.3 User Plane (Miners)

• Native SV2 miners connect directly to the Pool Server.
• Legacy SV1 miners connect through the Translator proxy.
• The Block Subsidy is shared across all participating miners proportional to their valid shares.
• The Winning Miner keeps 100% of the Transaction Fees from the mined block.
• Pool fee is fixed at 1%, divided 0.5% to the Treasury and 0.5% to the Node Network via Lightning.

2.4 Control Plane (MMI + Core)

The MMI (Management and Monitoring Interface) provides:
• Live network map and node telemetry.
• Full service control (start, stop, restart).
• Local vs Remote Template Provider switching.
• Lightning wallet management and yield settings.
• Key, token, and certificate rotation.
• System updates and upgrades with Core-signed verification.

3. FINANCIAL MODEL (ACTIVE)

Pool Fee: 1%
• 0.5% to Hyper Hash Treasury (Lightning Wallet)
• 0.5% to Node Network Lightning Wallets

Rewards:
• Block Subsidy is shared proportionally among miners by shares.
• Transaction Fees are kept entirely by the winning miner.

Node Earnings:
• Nodes earn Lightning payouts proportional to their share contributions.
• Nodes also earn routing fees for Lightning liquidity provided.

Treasury Earnings:
• Lightning liquidity yield and routing income.
• Compounding channel growth through reinvestment (if enabled).

4. DEPLOYMENT ENVIRONMENTS

Development / Regtest — Local stack used for CI, testing, and integration validation.
Staging / Signet — Public pre-production environment for QA and test miners.
Production / Mainnet — Fully redundant Core cluster, active Treasury node, and public/private Hyper Nodes.

5. REPOSITORY STRUCTURE

hyperhash-core — Core API and backend logic
hh-pool — Stratum V2 Pool Server
hh-tp — Template Provider
hh-translator — Stratum V1 ↔ V2 proxy
hh-node-agent — Node control daemon
hh-hypernode-public — Public Hyper Node deployment package
hh-hypernode-private — Private/partner Hyper Node package
hh-treasury — Lightning Treasury and fee router
hh-ui — Web interface and operator console
hh-protos — Protocol definitions and gRPC contracts
hh-examples — Regtest and Signet harnesses
hh-observability — Metrics, logging, and alerting stack
hh-infra — Terraform and Ansible deployment infrastructure
hh-website — Marketing site and documentation portal

6. NODE DEPLOYMENT PROFILES

Remote Template Provider Mode:
Runs Agent, Translator, and Lightning wallet, connecting to Core TP (default).

Local Template Provider Mode:
Runs Agent, Translator, bitcoind, Template Provider, and Lightning wallet.
This mode provides lower latency and better resilience.

The Node Agent manages Docker services, validates signed commands from Core, and reports telemetry in real time.

7. LIGHTNING WALLET AND YIELD MANAGEMENT

Each Node must run lnd or cln and maintain at least one channel to the Core Treasury for payouts.
All fee settlements and yields occur over Lightning.

Treasury to Node Flow:

Pool closes payout window.

Core calculates each node’s share.

Treasury creates Lightning invoices.

Node Agent verifies channel capacity.

Treasury pays invoice; Node updates telemetry.

MMI Lightning Controls include:
• Channel management (open, close, auto-maintain minimum).
• Yield strategy (reinvest on/off, reinvest percentage).
• Withdrawals (LN invoice or on-chain address).
• Auto-rebalance scheduling.
• Key and macaroon rotation.
• Telemetry display of balances and routing fees.

8. OBSERVABILITY AND METRICS

Prometheus collects:
• Node Lightning balance and reinvestment percent.
• Routing fees total.
• Channel count and status.
• Core Treasury payouts and pending invoices.
• Template latency and pool share statistics.

Grafana dashboards visualize Lightning yields, node uptime, and mining performance.

9. SECURITY MODEL

• All Core to Node communication uses mTLS and JWS signatures.
• Lightning API access uses macaroons with restricted permissions.
• Secrets are managed via SOPS and encrypted at rest.
• All payments are auditable, recording node ID, invoice ID, amount (sats), channel ID, and timestamp.

10. MMI CONFIGURATION SURFACE

Available actions for Node Operators:
• Enable or disable Translator and bitcoind.
• Switch Template Provider between local and remote.
• Reinvest or withdraw Lightning funds.
• Open or close Lightning channels.
• Rotate Lightning keys and authentication tokens.
• Adjust logging verbosity and export logs.
• Update Docker images, apply signed upgrades, or reboot the node.

11. INFRASTRUCTURE AND OPERATIONS

• Infrastructure as Code with Terraform and Ansible.
• Systemd-managed services for all core components.
• Nginx reverse proxy for HTTPS and load balancing.
• Automated backups, retention policies, and monitoring.
• Continuous Integration and Deployment via GitHub Actions.

12. UI AND MARKETING

Public Web UI:
• Network overview, pool statistics, node explorer, miner dashboard, and gamified “best hash” visualization.

Operator Console:
• Node telemetry, logs, wallet balance, yield metrics, and control toggles.

Website:
• Branding hub (hh-website) for whitepaper, onboarding, and announcements.

13. PHASED DELIVERY TIMELINE

Phase 0 – Infrastructure and Repository Setup
Duration: 2 weeks (October 2025)
Target: Working CI/CD and repo standardization.

MVP 1 – Signet Baseline
Goal: Pool, Template Provider, and Lightning Treasury operational.
Duration: 3 weeks (November 2025).

MVP 2 – Mainnet Launch
Goal: Dual-region Core cluster, full MMI, public and private Hyper Nodes.
Duration: 5 weeks (December 2025).

MVP 3 – Overlay Mesh
Goal: Inter-node Lightning routing and fee market.
Duration: 4 weeks (February 2026).

Phase 4 – Ecosystem Expansion
Goal: Community onboarding, documentation hub, and hardware partnerships.
Duration: Ongoing after March 2026.

14. TASK REGISTER

T1xx – Core and API Implementation
T2xx – Pool, Template Provider, Translator
T3xx – Lightning and Treasury
T4xx – MMI, UI, and UX
T5xx – Infrastructure and Deployment
T6xx – Documentation, Marketing, and Community

Each task is tracked in the Changelog file for status updates and completion dates.

15. CHANGELOG CONVENTION

Each changelog entry follows the pattern:

[T340] lnd integration — Completed 2025-10-22
[T311] Fee split logic — In progress

16. MANDATORY LIGHTNING RULES

Every Hyper Node must run a Lightning wallet.

Each Node must maintain at least one open channel to the Core Treasury.

Each Node must report Lightning telemetry to Core.

Pool fee is fixed at 1%, split 0.5% to Core Treasury and 0.5% to the Node Network.

The Block Subsidy is shared proportionally among miners by shares, while the Winning Miner keeps 100% of the Transaction Fees.

Yield management (reinvest or withdraw) is user-configurable via the MMI.

17. LICENSE AND ATTRIBUTION

Documentation © 2025 Hyper Hash Network
Licensed under Creative Commons Attribution 4.0 International (CC BY 4.0).
https://creativecommons.org/licenses/by/4.0/

END OF DOCUMENT — HYPER HASH NETWORK EXECUTION ROADMAP (VERSION 0)
