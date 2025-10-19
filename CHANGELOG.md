HYPER HASH NETWORK — DEVELOPMENT CHANGELOG AND TASK REGISTER (VERSION 0)
1. PURPOSE

This document is the authoritative engineering backlog and progress ledger for the Hyper Hash Network.
It defines every build, configuration, verification, and documentation task required to deploy a fully operational, production-grade mining network across Core, Hyper Nodes, Lightning Treasury, Template Providers, and Web Interfaces.

Each item in this file represents a trackable, testable unit of work.
All tasks begin as 🧩 PLANNED and are updated to 🔄 IN PROGRESS or ✅ COMPLETE as code moves through review and deployment.

This document functions as both:

A CHANGELOG (record of major versions, commits, and milestones).

A TASK REGISTER (comprehensive engineering plan with GitHub-linked tasks).

2. FORMAT AND STATUS CODES

Format Rules:

Each task has a unique ID per subsystem (e.g., C002d or N307.3).

Use concise one-line titles followed by a brief description.

Status options:

🧩 PLANNED — not yet started

🔄 IN PROGRESS — active development

✅ COMPLETE — merged and verified on mainnet

Every PR and Issue must reference its task ID in the title or description.

This file should always reflect the current canonical state of the network.

3. VERSION HISTORY
Date	Version	Milestone	Description
2025-10-21	v0.0	Baseline Reset	Clean rebuild initiated after VM teardown; all components to be rebuilt via GitHub-driven CI/CD with trackable commits.
2025-10-22	v0.1	CHANGELOG Framework	This Version 0 document introduced as the canonical engineering backlog for Hyper Hash.
   
4. TASK REGISTER MATRIX

🔖 CHANGELOG INDEX — VERSION 0 (MASTER BACKLOG SUMMARY)
Section	Subsystem	Description	Task Count	Status
4.1	🧱 Core Plane	Pool, API, Ledger, Payouts, Treasury hooks, and Core services.	≈132	🧩 PLANNED
4.2	⚙️ Template Provider (Sjors Fork)	Upstream Sjors TP integration, wrapper service, telemetry, and Core hooks.	≈60	🧩 PLANNED
4.3	🔁 Translator (SRI Upstream)	Stratum V2 Translator bridge for SV1 miners; compatibility with HH Pool.	≈65	🧩 PLANNED
4.4	🧠 Hyper Node Plane	Node Agent, MMI, local/remote TP, Translator, LN Wallet, eligibility tracking.	≈115	🧩 PLANNED
4.5	⚡ Lightning Treasury	LN payout engine, fee splits, node rewards, reinvestment and reconciliation.	≈90	🧩 PLANNED
4.6	🌐 Web UI / MMI Interfaces	Public pool dashboard and Node MMI (React UI, APIs, charts, telemetry).	≈85	🧩 PLANNED
4.7	🔒 Security & Operations	Hardening, CI/CD signing, observability, keys, DR, incident management.	≈75	🧩 PLANNED
4.8	🚀 Testing & Deployment	CI/CD pipeline, signet → mainnet rollout, regression & performance testing.	≈65	🧩 PLANNED
4.9	📚 Documentation & Brand	Docs, wiki, visuals, governance, community, and media presence.	≈80	🧩 PLANNED

🧩 Status Legend
Icon	Meaning
🧩	PLANNED — Task defined, not yet started.
🔄	IN PROGRESS — Active development or testing.
✅	COMPLETE — Merged, tested, verified on production.

📈 Aggregate Totals
Category	Count
Subsystems	9
Total Atomic Tasks (approx.)	~767
Initial Status	All 🧩 PLANNED

Execution Mode	GitHub-based CI/CD (track via PR references)

Primary Communication Channels	Discord
 + GitHub Issues/PRs

Version	v0 — Foundational baseline, Signet to Mainnet scope.

4.1 CORE PLANE DEVELOPMENT — TASKS (ALL 🧩 PLANNED)
Repository, Build, and Scaffolding

C001 — Core workspace bootstrap — Create monorepo/workspace (core-api, core-svc, core-cli), shared crates/utilities — 🧩 PLANNED

C002 — Language & toolchain pin — Pin Rust/Go/Node versions; add toolchain files; pre-commit hooks — 🧩 PLANNED

C003 — Dockerfiles (multi-stage) — Release and debug images for API and worker; small base — 🧩 PLANNED

C004 — Makefile/Taskfile — One-click build, test, lint, fmt, image publish — 🧩 PLANNED

C005 — GitHub Actions CI (build/lint/test) — Matrix for OS/toolchains; cache deps — 🧩 PLANNED

C006 — GitHub Actions CD (release) — Tag-triggered image publish to GHCR; SBOM attach — 🧩 PLANNED

C007 — Versioning scheme — Semver + vX.Y.Z tags; changelog automation — 🧩 PLANNED

C008 — core-cli bootstrap — Admin ops (migrate DB, rotate keys, seed data) — 🧩 PLANNED

API Surface (REST/gRPC) and Contracts

C010 — API spec repo — Define OpenAPI + gRPC protos (/registerNode, /telemetry, /payouts) — 🧩 PLANNED

C011 — Codegen pipeline — Generate server & client stubs from protos in CI — 🧩 PLANNED

C012 — Health endpoints — /v1/health, /v1/version — 🧩 PLANNED

C013 — Node registration — /v1/nodes:register mTLS + token, persist to DB — 🧩 PLANNED

C014 — Node auth refresh — /v1/nodes/{id}:refreshCert rotate mTLS certs — 🧩 PLANNED

C015 — Telemetry ingest — /v1/telemetry:push (batched), idempotent — 🧩 PLANNED

C016 — Node control bus — /v1/nodes/{id}:command (start/stop/restart/update) — 🧩 PLANNED

C017 — TP directory — /v1/tp/list and /v1/tp/assign (remote/local) — 🧩 PLANNED

C018 — Payout manifest API — /v1/payouts/manifest (sign, publish, revoke) — 🧩 PLANNED

C019 — LN treasury bridge — /v1/treasury/* status, request invoices, settle — 🧩 PLANNED

C020 — RBAC middleware — Roles: admin, treasury, node, viewer — 🧩 PLANNED

C021 — Rate limiting & quotas — Per-IP and per-Node ID — 🧩 PLANNED

C022 — API pagination & filtering — Standard list endpoints w/ cursors — 🧩 PLANNED

Authentication, mTLS, and Signing

C030 — Internal CA bootstrap — Root + intermediate, issuance workflow — 🧩 PLANNED

C031 — mTLS enforcement — All Core↔Node channels; cert pinning — 🧩 PLANNED

C032 — JWS signing service — Sign payout manifests & node commands — 🧩 PLANNED

C033 — JWT/OIDC admin auth — Admin/ops login integration — 🧩 PLANNED

C034 — Token lifecycle — Registration tokens, expiry, revocation list — 🧩 PLANNED

C035 — Secrets provider — SOPS/KMS integration for runtime decrypt — 🧩 PLANNED

Database and Ledger

C040 — Schema v1 — Tables: nodes, miners, shares, rounds, payouts, invoices, audit — 🧩 PLANNED

C041 — Migrations tooling — Up/down scripts; core-cli migrate — 🧩 PLANNED

C042 — Share ledger writer — Append-only shares ingestion pipeline — 🧩 PLANNED

C043 — Round aggregator — Track start/stop, valid shares, difficulty — 🧩 PLANNED

C044 — Payout calculator — Subsidy split (miners by shares), fee routing — 🧩 PLANNED

C045 — Node reward ledger — Even split among eligible nodes per 7d cycle — 🧩 PLANNED

C046 — Invoice store — LN invoice requests/responses, states — 🧩 PLANNED

C047 — Audit log — Immutable events: who/what/when/signature — 🧩 PLANNED

C048 — Indexes & tuning — Hot-path indices, partitioning for shares — 🧩 PLANNED

C049 — DB connection pool — Limits, retries, telemetry — 🧩 PLANNED

Pool, Template Provider, and Translator Integration

C060 — Pool ingest adapter — SV2 share validation event feed into ledger — 🧩 PLANNED

C061 — Block found hook — Coincident trigger → coinbase build → manifest — 🧩 PLANNED

C062 — Coinbase builder — Deterministic coinbase (miner fees to winner) — 🧩 PLANNED

C063 — TP selector — Route nodes/miners to optimal TP (latency/region) — 🧩 PLANNED

C064 — Translator stats — SV1→SV2 bridge health import to Core — 🧩 PLANNED

C065 — Miner identity mapping — Session→miner address, aliasing — 🧩 PLANNED

Fee Split, Treasury, and Lightning Hooks

C070 — 1% fee split engine — 0.5% Hyper Hash, 0.5% Node Network — 🧩 PLANNED

C071 — Node eligibility checker — ≥95% uptime, open LN channel, synced BTC — 🧩 PLANNED

C072 — Node-even-split calculator — Equal division of node reward pool — 🧩 PLANNED

C073 — Manifest signer — JWS signer seals payout manifest — 🧩 PLANNED

C074 — Treasury invoice collector — Request/validate node invoices — 🧩 PLANNED

C075 — Payment reconciler — Confirm payments, retry/backoff, mark settled — 🧩 PLANNED

C076 — Dispute & reissue flow — Handle stale/mismatched invoices — 🧩 PLANNED

Telemetry, Metrics, and Logging

C080 — Prometheus metrics — /metrics for API/DB/payout queues — 🧩 PLANNED

C081 — Business metrics — shares/sec, rounds, payout amounts, latency — 🧩 PLANNED

C082 — LN metrics bridge — treasury balances, failed HTLCs, yield — 🧩 PLANNED

C083 — Structured logging — JSON logs, request IDs, sampling — 🧩 PLANNED

C084 — Log redaction — Prevent secrets/macaroons in logs — 🧩 PLANNED

C085 — Alert rules set — Payout backlog, API error rate, DB lag — 🧩 PLANNED

Configuration & Ops Interfaces

C090 — Config schema — ENV + TOML w/ priority, sane defaults — 🧩 PLANNED

C091 — Hot-reload config — SIGHUP watcher, safe reload — 🧩 PLANNED

C092 — Feature flags — Gradual rollouts for riskier features — 🧩 PLANNED

C093 — Admin CLI ops — rotate-certs, rotate-keys, drain-node, replay-manifest — 🧩 PLANNED

C094 — Read-only mode — Maintenance gates for payouts/API — 🧩 PLANNED

Packaging, Deployment, and HA

C100 — Systemd units — hh-core.service, hardening opts — 🧩 PLANNED

C101 — K8s manifests (optional) — Statefulset/Deployment, secrets, HPA — 🧩 PLANNED

C102 — Nginx/HAProxy — TLS offload, rate-limit, mTLS pass-through — 🧩 PLANNED

C103 — Blue/green deploy — Zero-downtime API promotion — 🧩 PLANNED

C104 — DB migrations in CI — Auto-migrate on boot w/ lock & backup — 🧩 PLANNED

C105 — Backup & restore scripts — DB dumps, verify, time-to-restore — 🧩 PLANNED

C106 — Read replica config — Replica for heavy read/reporting — 🧩 PLANNED

Security Hardening

C110 — TLS policy — TLS1.3 only, strong ciphers, OCSP stapling — 🧩 PLANNED

C111 — AuthZ tests — RBAC unit/integration tests — 🧩 PLANNED

C112 — Input validation — Strict schema validation on all endpoints — 🧩 PLANNED

C113 — DoS protections — Global/per-entity limiters and circuit-breakers — 🧩 PLANNED

C114 — Secrets at rest — KMS-backed SOPS for all sensitive configs — 🧩 PLANNED

C115 — Supply-chain scans — SBOM, vulnerability gates in CI — 🧩 PLANNED

Verification & Testing (Core)

C120 — Unit tests: API handlers — Success/error paths, schema checks — 🧩 PLANNED

C121 — Unit tests: fee engine — 1% split, rounding, boundary cases — 🧩 PLANNED

C122 — Unit tests: eligibility — uptime, LN channel, BTC sync logic — 🧩 PLANNED

C123 — Unit tests: payouts — miners-by-shares + winner-tx-fees — 🧩 PLANNED

C124 — Integration: TP flow — template request→pool job→share ingest — 🧩 PLANNED

C125 — Integration: translator — SV1 shares accepted via bridge — 🧩 PLANNED

C126 — Integration: treasury — manifest→invoice→settle→reconcile — 🧩 PLANNED

C127 — Load test: API RPS — target throughput & p95 latency — 🧩 PLANNED

C128 — Fault test: DB outage — degraded mode, queue & recovery — 🧩 PLANNED

C129 — Fault test: LN down — payout deferral and auto-resume — 🧩 PLANNED

C130 — Security tests — mTLS enforcement, RBAC bypass attempts — 🧩 PLANNED

C131 — Observability tests — metrics & alerts fire under faults — 🧩 PLANNED

C132 — Backup restore drill — full DB restore, data integrity — 🧩 PLANNED

Documentation (Core)

C140 — API reference — Generated OpenAPI & gRPC docs — 🧩 PLANNED

C141 — Runbook — Core bootstrapping, rotating keys, DR — 🧩 PLANNED

C142 — Config guide — All env vars & examples — 🧩 PLANNED

C143 — Upgrade guide — Blue/green steps, rollback — 🧩 PLANNED

C144 — Troubleshooting — Common errors, remedies — 🧩 PLANNED

4.2 TEMPLATE PROVIDER (TP) — USING SJORS’ UPSTREAM (ALL 🧩 PLANNED)
Upstream Strategy & Repo Management

T001 — Fork upstream TP — Create hh-tp fork from <UPSTREAM_URL>; document license & attribution. — 🧩 PLANNED

T002 — Pin baseline commit — Select a known-good tag/commit; record SHA in /VERSION. — 🧩 PLANNED

T003 — Patch queue setup — Create patches/ with numbered series; apply via git am in CI. — 🧩 PLANNED

T004 — Upstream sync workflow — Weekly rebase job; open “Update to <tag>” PR automatically. — 🧩 PLANNED

T005 — GHCR mirror — Build and publish upstream TP image under ghcr.io/hyperhash-org/tp:<tag>-hh1. — 🧩 PLANNED

T006 — License compliance — Add NOTICE, preserve upstream headers, SPDX scanning in CI. — 🧩 PLANNED

Build & Packaging (Wrapper Only)

T010 — Minimal wrapper binary — hh-tp-wrapper to load env/TOML, exec upstream TP, expose health. — 🧩 PLANNED

T011 — Dockerfile (multi-stage) — Stage 1: build wrapper; Stage 2: copy upstream TP binary + wrapper. — 🧩 PLANNED

T012 — Entry-point script — Start wrapper → validate bitcoind IPC → exec upstream TP; safe signals. — 🧩 PLANNED

T013 — Systemd unit — hh-tp.service with Restart=on-failure, hardening options. — 🧩 PLANNED

T014 — Config adapter — Map Hyper Hash env/TOML to upstream TP flags/paths. — 🧩 PLANNED

Integration with Core & Nodes

T020 — Core heartbeat shim — Wrapper posts /v1/tp/heartbeat (status, tip height, latency). — 🧩 PLANNED

T021 — Template telemetry — Push template build time, tx count, mempool size to Core metrics. — 🧩 PLANNED

T022 — Local/Remote TP toggle — Support env switch; wrapper enforces one active mode. — 🧩 PLANNED

T023 — Translator feed check — Verify upstream TP emits artifacts compatible with our Translator. — 🧩 PLANNED

T024 — TP directory registration — Announce TP endpoint to Core directory with mTLS. — 🧩 PLANNED

T025 — Command channel hooks — Implement reload/rotate/debug via wrapper (Core-signed). — 🧩 PLANNED

Bitcoin Core Plumbing

T030 — IPC path validation — Detect bitcoind socket/host; backoff + jitter reconnect. — 🧩 PLANNED

T031 — Chain mode flags — mainnet/signet/regtest support via env; audit upstream support. — 🧩 PLANNED

T032 — Reorg handling audit — Confirm upstream behavior; add wrapper alarms on ≥2-block reorg. — 🧩 PLANNED

Observability & Alerts

T040 — Prometheus exporter — Wrapper exposes /metrics (template_latency_ms, tip_lag_blocks). — 🧩 PLANNED

T041 — Structured logs — Route upstream stdout to Loki; add TP source labels. — 🧩 PLANNED

T042 — Alert rules — Stale template > 30s, RPC error rate, reorgs, wrapper restarts. — 🧩 PLANNED

Security & Hardening

T050 — mTLS to Core — Wrapper uses node certs; verify CA pin; rotate certs. — 🧩 PLANNED

T051 — Drop privileges — Run as tp user; read-only FS; CAP_NET_BIND_SERVICE only if needed. — 🧩 PLANNED

T052 — Supply-chain scan — SBOM + vulnerability scan of upstream releases in CI. — 🧩 PLANNED

Verification & Testing (with Upstream)

T060 — Smoke test — Bring up bitcoind (signet), run upstream TP via wrapper; health OK. — 🧩 PLANNED

T061 — Core integration test — Heartbeat visible at Core; metrics scraped; directory entry created. — 🧩 PLANNED

T062 — Translator E2E — SV1 miner → Translator → upstream TP → Pool → share accepted. — 🧩 PLANNED

T063 — Latency benchmark — Template build < 50 ms average; tip lag < 1 block. — 🧩 PLANNED

T064 — Reorg simulation — Force reorg; wrapper logs event; Core alert fires. — 🧩 PLANNED

T065 — Config reload — Change TOML; SIGHUP; no process exit; settings applied. — 🧩 PLANNED

T066 — Failure injection — Kill bitcoind; upstream TP recovers; wrapper backoff respected. — 🧩 PLANNED

T067 — Upgrade test — Rebase to new upstream tag; patch queue applies; no regressions. — 🧩 PLANNED

T068 — Security test — mTLS required; reject invalid CA; clock skew tolerance. — 🧩 PLANNED

Documentation (Upstream + Wrapper)

T070 — README (Hyper Hash TP) — How to run upstream TP via wrapper (local/remote). — 🧩 PLANNED

T071 — Config mapping table — Our env/TOML → upstream flags reference. — 🧩 PLANNED

T072 — Ops runbook — Logs, metrics, restarts, safe rollback to prior tag. — 🧩 PLANNED

T073 — Upgrade procedure — Rebase flow, patch queue conflicts, validation checklist. — 🧩 PLANNED

T074 — Troubleshooting — IPC failures, stale templates, version mismatches. — 🧩 PLANNED

4.3 TRANSLATOR (SV1 ↔ SV2) — USING SRI TRANSLATOR (ALL 🧩 PLANNED)

This section assumes the Stratum V2 Reference Implementation (SRI) Translator is our upstream. We package, pin, and integrate it with hh-pool (SV2), Sjors’ TP, and our Core/MMI. Focus is on version pinning, compatibility, observability, security, and golden tests.

Upstream, Versioning, and Packaging

X001 — Fork or pin SRI Translator repo — Create hh-translator wrapper; record upstream tag/SHA; add NOTICE. — 🧩 PLANNED

X002 — Patch queue scaffolding — patches/ with numbered git am series; CI job applies and fails on drift. — 🧩 PLANNED

X003 — Dockerfile (multi-stage) — Build upstream translator; minimal runtime; non-root user. — 🧩 PLANNED

X004 — GHCR publish — ghcr.io/hyperhash-org/translator:<sri-tag>-hh1 on tags; attach SBOM. — 🧩 PLANNED

X005 — License/SPDX scan — CI step to verify upstream licensing and headers. — 🧩 PLANNED

Runtime Configuration & Profiles

X010 — Config schema — ENV/TOML to set: upstream SV2 pool addr, local SV1 listen addr, vardiff toggle, agg/non-agg channels, timeouts. — 🧩 PLANNED

X011 — Profiles — regtest, signet, mainnet presets with safe defaults. — 🧩 PLANNED

X012 — Vardiff alignment — Ensure translator vardiff is OFF by default; pool controls difficulty to avoid dueling policies. — 🧩 PLANNED

X013 — Channel mode flags — Start non-aggregated (debug) profile; enable aggregated once stable. — 🧩 PLANNED

X014 — Standard Channels enable — Ensure pool & translator both use Standard Channels capability bit. — 🧩 PLANNED

X015 — Noise/TLS pass-through — Do not terminate TLS/noise on LB; document network path. — 🧩 PLANNED

X016 — Health endpoints — Sidecar /healthz HTTP and Prometheus /metrics exporter. — 🧩 PLANNED

Integration with hh-pool (SV2) and Sjors’ TP

X020 — Pool handshake compat — Validate SetupConnection, JobNegotiator, and share submit messages against hh-pool. — 🧩 PLANNED

X021 — Target/difficulty mapping — Ensure translator→pool maps SV1 difficulty to SV2 target identically; add unit vectors. — 🧩 PLANNED

X022 — Share validation parity — Confirm share acceptance semantics (nTime/nonce/extra nonce) match hh-pool rules. — 🧩 PLANNED

X023 — Template path audit — With Sjors’ TP, verify coinbase/merkle branches expectations; TX fee remains assigned to winning miner. — 🧩 PLANNED

X024 — Reconnect semantics — Translator auto-reconnect to pool; exponential backoff; session resume if supported. — 🧩 PLANNED

X025 — Latency budget — Measure translator→pool p95 RTT; set alert threshold. — 🧩 PLANNED

Core & Node Control Hooks

X030 — Core directory entry — Register translator endpoint (SV1 port) with Core over mTLS. — 🧩 PLANNED

X031 — MMI toggles — Start/stop translator; switch agg/non-agg; vardiff toggle; persist in node config. — 🧩 PLANNED

X032 — Telemetry push — Export connected miners, share rate, reject rate, avg difficulty to Core. — 🧩 PLANNED

X033 — Signed commands — Accept Core-signed restart/update commands; verify JWS signature. — 🧩 PLANNED

X034 — Config hot-reload — Apply changes without dropping all sessions where possible. — 🧩 PLANNED

Observability and Logging

X040 — Prometheus metrics — translator_sessions, shares_valid, shares_rejected, rtt_ms, agg_channel_on, vardiff_on. — 🧩 PLANNED

X041 — Structured logs — JSON with session IDs; redact secrets; Loki labels component=translator. — 🧩 PLANNED

X042 — Alert rules — Reject rate > 5 %, RTT > 250 ms, reconnect storm, session auth failures. — 🧩 PLANNED

X043 — Grafana panels — Translator health board and miner intake funnel. — 🧩 PLANNED

Security & Hardening

X050 — mTLS to Core — Translator sidecar posts metrics/heartbeats via mTLS; CA pinning. — 🧩 PLANNED

X051 — Non-root runtime — Drop caps, read-only FS, seccomp profile; only bind SV1 port. — 🧩 PLANNED

X052 — Rate limiting — Per-IP connection limit and handshake flood protection. — 🧩 PLANNED

X053 — DoS guardrails — SYN cookies + LB connection limits; document settings. — 🧩 PLANNED

X054 — Supply-chain scan — Image scanned for CVEs in CI; block on high severity. — 🧩 PLANNED

Verification & Testing (Translator)

X060 — Unit tests: diff/target — Validate SV1 difficulty ↔ SV2 target conversion (boundary cases). — 🧩 PLANNED

X061 — Unit tests: share parser — SV1 share → SV2 submit message encoding/endianness. — 🧩 PLANNED

X062 — Integration: SV1 miner (CGMiner) — CGMiner → Translator → hh-pool → share accepted on Signet. — 🧩 PLANNED

X063 — Integration: Antminer S9/S19 — Real hardware test through translator to hh-pool Signet. — 🧩 PLANNED

X064 — End-to-end: block found — Ensure translator sessions survive round close and payout manifest generation. — 🧩 PLANNED

X065 — Aggregated channel test — Enable aggregated mode; confirm stable share acceptance and latency. — 🧩 PLANNED

X066 — Vardiff off/on test — Verify no conflict with pool difficulty controller; keep OFF by default. — 🧩 PLANNED

X067 — Reorg behavior — Simulate reorg in Sjors’ TP path; verify no share invalidation bug. — 🧩 PLANNED

X068 — Fault injection: pool down — Drop pool; translator reconnects; miners keep sessions alive. — 🧩 PLANNED

X069 — Load test — 1k concurrent SV1 connections on Signet; chart CPU/mem/latency. — 🧩 PLANNED

X070 — Security tests — mTLS to Core enforced; reject invalid CA; log & alert on failed auth. — 🧩 PLANNED

Deployment & Ops

X080 — Systemd unit — hh-translator.service; Restart=on-failure; sane limits. — 🧩 PLANNED

X081 — Compose profile — translator service with env file, healthcheck, depends_on. — 🧩 PLANNED

X082 — K8s (optional) — Deployment with PDB/HPA; ConfigMap for profiles; Service for SV1 port. — 🧩 PLANNED

X083 — Upgrade path — Tag pinning; blue/green upgrade; rollback to prior image. — 🧩 PLANNED

X084 — Runbook — Start/stop, logs, health, common errors, rollback. — 🧩 PLANNED

Documentation (Translator)

X090 — README (Hyper Hash Translator) — How to connect SV1 miners to Hyper Hash via SRI Translator. — 🧩 PLANNED

X091 — Config reference — All env/TOML keys with defaults and examples. — 🧩 PLANNED

4.4 HYPER NODE PLANE — ENGINEERING-LEVEL TASKS (ALL 🧩 PLANNED)

This section defines everything needed to build, configure, run, and verify Hyper Nodes (Public and Private).
A Hyper Node bundles: Node Agent, optional local bitcoind, optional local TP (Sjors wrapper), optional Translator, and a mandatory Lightning wallet (lnd or cln).
Nodes must meet eligibility (≥95% uptime over 7 days, ≥1 open LN channel to Treasury, synced Bitcoin node) for even node-pool rewards.

Repositories, Bundles, and Build System

N301 — Create hh-hypernode-public repo — Compose bundle for public nodes; LICENSE/NOTICE. — 🧩 PLANNED

N302 — Create hh-hypernode-private repo — Partner/operator bundle with extra hooks. — 🧩 PLANNED

N303 — Node Agent repo hh-node-agent — Source layout (agent/, mmi/, telemetry/). — 🧩 PLANNED

N304 — Toolchain pins — Pin Go/Rust/Node versions; pre-commit hooks; fmt/lint. — 🧩 PLANNED

N305 — Dockerfiles (multi-stage) — Agent, lnd/cln sidecars, optional bitcoind, TP-wrapper, Translator. — 🧩 PLANNED

N306 — Makefile/Taskfile — build, test, package, compose up -d. — 🧩 PLANNED

N307 — GitHub Actions CI — Build, unit tests, image push to GHCR; SBOM attach. — 🧩 PLANNED

N308 — Release pipeline — Tag images per component; compat matrix doc. — 🧩 PLANNED

Node Agent (Control, Commands, Updates)

N310 — Agent bootstrap — gRPC/REST local API on localhost:8442; auth token. — 🧩 PLANNED

N311 — Core handshake — mTLS client; register node; fetch signed config seed. — 🧩 PLANNED

N312 — Signed command executor — Verify Core JWS; actions: start/stop/restart/update. — 🧩 PLANNED

N313 — Service supervisor — Manage Docker services; healthcheck orchestration. — 🧩 PLANNED

N314 — Upgrade engine — Pull signed manifest; drain → update → verify → uncordon; rollback on fail. — 🧩 PLANNED

N315 — Config manager — Load from /etc/hyperhash/node-config.toml + ENV; schema validation. — 🧩 PLANNED

N316 — Secrets loader — SOPS/KMS decrypt for macaroons, TLS, RPC creds; in-memory only. — 🧩 PLANNED

N317 — Audit trail — Append-only JSON log for every command (actor, sig, result). — 🧩 PLANNED

N318 — Local auth — MMI bearer token rotation; rate-limit control API. — 🧩 PLANNED

Bitcoin Node (Optional Local)

N320 — bitcoind container — v30 image, pruned or archival selectable. — 🧩 PLANNED

N321 — Config templating — bitcoin.conf generation (rpc, txindex opt-in, blockfilter). — 🧩 PLANNED

N322 — Data volumes — Bind mounts with permissions; fast SSD recommendation doc. — 🧩 PLANNED

N323 — Sync monitor — Track IBD progress; expose blocks vs tip; report to Core. — 🧩 PLANNED

N324 — Health checks — IPC socket probe; mempool stats; reorg watcher. — 🧩 PLANNED

Template Provider (Local/Remote Switch)

N330 — TP wrapper integration — Use Sjors TP wrapper container; share bitcoind socket. — 🧩 PLANNED

N331 — Mode toggle — tp.mode = local|remote in config; enforce single active path. — 🧩 PLANNED

N332 — Remote TP directory — Query Core for nearest TP; fallback list; latency probe. — 🧩 PLANNED

N333 — Hot-switch TP — Seamless swap with minimal job loss; warn miners via pool. — 🧩 PLANNED

N334 — TP telemetry — Template latency, tip lag, mempool size → Core metrics. — 🧩 PLANNED

Translator (Optional, SV1 Support)

N340 — Translator container — SRI translator image; SV1 port 34255; non-root. — 🧩 PLANNED

N341 — Profiles — non-aggregated debug, aggregated prod; vardiff OFF default. — 🧩 PLANNED

N342 — Pool endpoint wiring — Ensure HH pool address & channel flags match. — 🧩 PLANNED

N343 — Translator telemetry — Sessions, shares valid/reject, RTT → Agent → Core. — 🧩 PLANNED

Lightning Wallet (Mandatory)

N350 — Wallet selector — lnd or cln selectable; uniform Agent API. — 🧩 PLANNED

N351 — Seed/init flow — Create wallet; backup seed; store encrypted; ops checklist. — 🧩 PLANNED

N352 — Treasury peering — Connect to Treasury pubkey@host:9735; persist. — 🧩 PLANNED

N353 — Open channel — Enforce min capacity; tag “payout”; save chan ID. — 🧩 PLANNED

N354 — Invoice server — Generate invoices for node reward payouts (idempotent). — 🧩 PLANNED

N355 — Reinvest/withdraw — Implement policy engine; % slider; hard caps. — 🧩 PLANNED

N356 — Auto-rebalance — Periodic rebalance with fee ceiling; configurable cadence. — 🧩 PLANNED

N357 — LN telemetry — Balances, pending HTLCs, routing fees, chan states → Core. — 🧩 PLANNED

N358 — Key/macaroon rotation — Scheduled regeneration; Agent reload without restart. — 🧩 PLANNED

Eligibility Tracker (Reward Qualification)

N360 — Uptime sampler — Sliding window (7 days) with 5-sec samples; ≥95% rule. — 🧩 PLANNED

N361 — Channel check — Verify ≥1 open channel to Treasury; emit state. — 🧩 PLANNED

N362 — Bitcoin sync rule — Require “synced” flag if TP = local; else skip. — 🧩 PLANNED

N363 — Eligibility exporter — Single boolean + reasons; pushed to Core hourly & on change. — 🧩 PLANNED

N364 — Edge cases — Grace period after updates; maintenance windows. — 🧩 PLANNED

Telemetry, Metrics, and Logging

N370 — Prometheus exporter — /metrics for agent: cpu/mem, svc health, RTTs. — 🧩 PLANNED

N371 — Business metrics — share rate (if translator), LN yield, reinvest %, uptime. — 🧩 PLANNED

N372 — Structured logs — JSON with request IDs; redact secrets; Loki labels. — 🧩 PLANNED

N373 — Alerting hints — Local rules: LN balance low, chan closed, TP stale. — 🧩 PLANNED

MMI (Local Management Interface) — API Side

N380 — MMI API surface — Endpoints: /services, /lightning, /eligibility, /updates, /logs. — 🧩 PLANNED

N381 — Actions — Start/stop services; TP mode switch; set reinvest %; withdraw (LN invoice); rotate keys. — 🧩 PLANNED

N382 — AuthZ — Local token + optional mTLS; rate-limit & lockout on failures. — 🧩 PLANNED

N383 — Telemetry pages — Summaries for LN, TP, Translator, bitcoind, system. — 🧩 PLANNED

N384 — Debug bundle — Zip logs + metrics snapshot; download securely. — 🧩 PLANNED

Security Hardening

N390 — mTLS to Core — Cert pinning; renewals 90 days; clock skew tolerance. — 🧩 PLANNED

N391 — Filesystem perms — 600 for secrets; read-only images; tmpfs for sensitive dirs. — 🧩 PLANNED

N392 — Network policy — UFW rules; only SV1, SV2 (if needed), HTTPS, LN ports; no inbound Core. — 🧩 PLANNED

N393 — Rate limits — API & MMI throttles; DoS guard for translator port. — 🧩 PLANNED

N394 — Supply-chain scans — CI CVE gates; base image pinning. — 🧩 PLANNED

Deployment (Compose, Systemd, Optional K8s)

N400 — Compose files — Profiles: public, private, local-tp, remote-tp, translator. — 🧩 PLANNED

N401 — Env templates — .env.example with sane defaults and comments. — 🧩 PLANNED

N402 — Healthchecks — curl localhost:8442/v1/status, lncli ping, tp health, translator health. — 🧩 PLANNED

N403 — Systemd units — hh-node-agent.service; restart/backoff; dependencies. — 🧩 PLANNED

N404 — Host tuning — Sysctl for file descriptors, TCP buffers; clock sync. — 🧩 PLANNED

N405 — Backup hooks — LN seed backup reminder; optional export to encrypted volume. — 🧩 PLANNED

Verification & Testing (Node)

N410 — Unit: config loader — Validate TOML/ENV precedence, defaults, errors. — 🧩 PLANNED

N411 — Unit: signed commands — JWS verify; bad sig rejection; replay protection. — 🧩 PLANNED

N412 — Unit: eligibility rules — Uptime/window math; LN channel; BTC sync toggles. — 🧩 PLANNED

N413 — Int: Core register — Node appears in Core; telemetry flow OK. — 🧩 PLANNED

N414 — Int: Local TP mode — bitcoind + TP produce templates; Core sees metrics. — 🧩 PLANNED

N415 — Int: Remote TP mode — Switch at runtime; minimal job loss. — 🧩 PLANNED

N416 — Int: Translator path — SV1 miner → Translator → Pool → Share accepted. — 🧩 PLANNED

N417 — Int: LN payout — Treasury manifest → invoice → payment → telemetry update. — 🧩 PLANNED

N418 — Fail: treasury down — Payout deferred; auto-resume on recovery. — 🧩 PLANNED

N419 — Fail: reboot node — Services recover; eligibility window intact. — 🧩 PLANNED

N420 — Load: 500 miners — Validate CPU/mem, reject rate, RTT thresholds. — 🧩 PLANNED

N421 — Security: mTLS — Reject wrong CA; expired cert; clock skew. — 🧩 PLANNED

N422 — Observability: alerts — Fire on LN low balance, TP stale >30s, translator reconnect storm. — 🧩 PLANNED

Documentation (Node)

N430 — README (Public) — One-hour quickstart; compose up; connect SV1/SV2 miners. — 🧩 PLANNED

N431 — README (Private) — Partner deployment, peering, support contacts. — 🧩 PLANNED

N432 — Config reference — All keys with examples; local vs remote TP tables. — 🧩 PLANNED

N433 — MMI guide — Screens/flows for wallet, reinvest, withdrawals, updates. — 🧩 PLANNED

N434 — Troubleshooting — Common errors: LN channel, stale templates, auth failures. — 🧩 PLANNED

N435 — Security checklist — Hardening steps; backup/restore; key rotation. — 🧩 PLANNED

X092 — Troubleshooting guide — High reject rate, latency spikes, version mismatch tips. — 🧩 PLANNED

X093 — Compatibility matrix — Supported SV1 miner firmware versions and best-known settings. — 🧩 PLANNED

4.5 LIGHTNING TREASURY — ENGINEERING-LEVEL TASKS (ALL 🧩 PLANNED)

The Treasury is a dedicated Lightning node cluster operated by Hyper Hash.
It receives 0.5 % of every block subsidy (pool fee), distributes the other 0.5 % evenly to all eligible Hyper Nodes, and processes miner payouts through manifests received from Core.
All payouts occur over LN channels; on-chain fallback is available for failsafe settlement.

Repository, Build, and Environment

L001 — Create repo hh-treasury — Source layout (core/, bridge/, scripts/, docs/). — 🧩 PLANNED

L002 — Choose backend — Default to lnd 0.18 or later; pluggable cln adapter. — 🧩 PLANNED

L003 — Toolchain pins — Go/Rust versions; CI caches; make lint test. — 🧩 PLANNED

L004 — Dockerfile — Build lnd + bridge daemon; minimal Alpine runtime. — 🧩 PLANNED

L005 — GH Actions CI — Unit/integration tests; image publish → GHCR. — 🧩 PLANNED

L006 — Systemd unit — hh-treasury.service; Restart=on-failure; hardening opts. — 🧩 PLANNED

Channel & Liquidity Management

L010 — Treasury wallet init — Seed creation, encrypted backup, ops runbook. — 🧩 PLANNED

L011 — Core peering — Establish persistent channels with Hyper Nodes. — 🧩 PLANNED

L012 — Hyper Hash channel — 0.5 % fee share accumulation; route liquidity to corporate wallet. — 🧩 PLANNED

L013 — Channel manager — Monitor capacities, auto-rebalance, fee limits, CLTV delta. — 🧩 PLANNED

L014 — Multi-node cluster — Optional active-active Treasury instances with static channels. — 🧩 PLANNED

L015 — On-chain reserve — Maintain buffer ≥ 0.02 BTC per node for emergency closure. — 🧩 PLANNED

L016 — Liquidity alarms — Trigger if outbound < 20 % or inbound < 15 %. — 🧩 PLANNED

Payout Engine & Ledger Integration

L020 — Payout manifest parser — Consume Core-signed JSON manifest. — 🧩 PLANNED

L021 — Signature verification — Validate JWS; reject altered or expired manifests. — 🧩 PLANNED

L022 — Payment dispatcher — Generate LN invoices; queue payments concurrently. — 🧩 PLANNED

L023 — Batch controller — Group ≤ 50 payments per round; concurrency limits; retry/back-off. — 🧩 PLANNED

L024 — Fee split calculator — 1 % total: 0.5 % → Hyper Hash, 0.5 % → eligible nodes (equal split). — 🧩 PLANNED

L025 — Node eligibility filter — Pull from Core API; apply uptime & channel criteria. — 🧩 PLANNED

L026 — Miner payouts — Distribute block subsidy shares per ledger; attach TX fee to winner miner. — 🧩 PLANNED

L027 — Confirmation reconciler — Verify settlement; mark paid; retry failed invoices. — 🧩 PLANNED

L028 — Reissue flow — Detect stale invoices; request fresh invoice; sign new batch. — 🧩 PLANNED

L029 — Treasury journal DB — Tables: invoices, batches, failures, reconciliations, metrics. — 🧩 PLANNED

L030 — Report exporter — Daily CSV/JSON of payouts and fees for audit. — 🧩 PLANNED

Core Bridge & APIs

L040 — Core Webhook listener — /v1/manifest endpoint for payout notifications. — 🧩 PLANNED

L041 — Treasury status API — /v1/treasury/status returns balances, liquidity, queue. — 🧩 PLANNED

L042 — Payment query API — /v1/payments/{id} shows state and proof. — 🧩 PLANNED

L043 — Admin control API — Pause/resume payouts, rebalance, channel policy update. — 🧩 PLANNED

L044 — RBAC tokens — Roles: treasurer, auditor, ops; rotate weekly. — 🧩 PLANNED

Reinvestment & Yield Controls

L050 — Policy engine — Percent allocation of LN yield → reinvest vs withdraw. — 🧩 PLANNED

L051 — Auto-compounder — Sweep routing fees into Treasury wallet daily. — 🧩 PLANNED

L052 — Node policy hooks — Accept MMI preferences; apply per-node reinvest setting. — 🧩 PLANNED

L053 — Withdrawal queue — Honor partial withdraw requests within caps. — 🧩 PLANNED

L054 — Audit trail — Log every policy change with signature and timestamp. — 🧩 PLANNED

Monitoring & Telemetry

L060 — Prometheus metrics — payouts_total, invoices_failed, rebalance_events, ln_fees_earned. — 🧩 PLANNED

L061 — Grafana dashboards — Payout latency, success rate, liquidity heatmap. — 🧩 PLANNED

L062 — Alert rules — Payment queue > 100, failures > 2 %, liquidity alarm. — 🧩 PLANNED

L063 — Structured logs — JSON w/ txid, invoice, route; redact seeds. — 🧩 PLANNED

L064 — Audit export job — Push signed report daily to secure bucket. — 🧩 PLANNED

Security & Hardening

L070 — mTLS to Core — Rotate certs every 90 days; enforce TLS 1.3. — 🧩 PLANNED

L071 — Key vault — HSM integration for seed and invoice macaroons. — 🧩 PLANNED

L072 — Database encryption — AES-GCM on sensitive fields. — 🧩 PLANNED

L073 — Access controls — Ops login MFA; RBAC scopes enforced per endpoint. — 🧩 PLANNED

L074 — DoS guards — Payment rate limiter; batch size cap. — 🧩 PLANNED

L075 — Supply-chain scan — CI SBOM audit for base images and deps. — 🧩 PLANNED

Verification & Testing (Treasury)

L080 — Unit: fee split math — 1 % split; rounding tests; boundary cases. — 🧩 PLANNED

L081 — Unit: eligibility filter — Only eligible nodes receive reward. — 🧩 PLANNED

L082 — Int: Core manifest flow — Receive manifest → verify → batch → settle → reconcile. — 🧩 PLANNED

L083 — Int: LN payment path — Treasury → Node → settle; simulate fee routes. — 🧩 PLANNED

L084 — Int: Reissue loop — Expired invoice → re-request → retry → success. — 🧩 PLANNED

L085 — Load: 1000 invoices — Validate parallel batch throughput & memory footprint. — 🧩 PLANNED

L086 — Fault: lnd offline — Queue payouts; auto-resume when back online. — 🧩 PLANNED

L087 — Fault: Core down — Cache manifest; re-pull on reconnect. — 🧩 PLANNED

L088 — Security: JWS tamper — Reject bad signature; log alert. — 🧩 PLANNED

L089 — Observability: alerts fire — Simulate liquidity drop; verify Grafana alarm. — 🧩 PLANNED

L090 — Backup restore test — Full DB restore; data integrity check. — 🧩 PLANNED

Documentation (Treasury)

L100 — README — Overview, build, run, connect to Core. — 🧩 PLANNED

L101 — Config guide — TOML/ENV keys for LN paths and fee policies. — 🧩 PLANNED

L102 — Ops manual — Channel open/close, rebalance, payout monitoring. — 🧩 PLANNED

L103 — Audit guide — Ledger export format and verification procedure. — 🧩 PLANNED

L104 — Troubleshooting — Stuck invoice, liquidity shortage, fee calc mismatch. — 🧩 PLANNED

4.6 WEB UI & MMI INTERFACES — ENGINEERING-LEVEL TASKS (ALL 🧩 PLANNED)

The web front-end layer contains two surfaces:

Public UI — hosted at hh-pool.org for miners and observers (Pool stats, Network, Nodes, Info pages).

Node MMI (Management Interface) — served locally on each Hyper Node for configuration and Lightning control.

Both share a common design system and API layer.

Repository, Framework, and Build System

W001 — Create repo hyperhash-ui — Base Quasar/React workspace; license, CI bootstrap. — 🧩 PLANNED

W002 — Front-end framework — Choose React + Vite + Tailwind; enable shadcn/ui + Recharts. — 🧩 PLANNED

W003 — TypeScript baseline — tsconfig.json, ESLint, Prettier, husky pre-commit. — 🧩 PLANNED

W004 — Storybook setup — Component isolation + snapshot testing. — 🧩 PLANNED

W005 — GitHub Actions CI — Lint, build, test, deploy to staging bucket. — 🧩 PLANNED

W006 — Dockerfile — Multi-stage build → Nginx runtime image. — 🧩 PLANNED

W007 — Systemd unit — hh-ui.service; serve static from /var/www/hyperhash. — 🧩 PLANNED

Global Design System

W010 — Theme tokens — Dark mode default; lane colours; rainbow “Bifrost” accent. — 🧩 PLANNED

W011 — Layout framework — Responsive grid; 16 px base spacing; rounded 2xl. — 🧩 PLANNED

W012 — Icon set — Lucide-React base; mining lane, lightning, bitcoin, node glyphs. — 🧩 PLANNED

W013 — Typography — Inter + Space Mono; weights 400–700; consistent across views. — 🧩 PLANNED

W014 — Animation presets — Framer Motion transitions for miner streams & node links. — 🧩 PLANNED

W015 — Accessibility pass — ARIA roles, keyboard navigation, contrast ≥ 4.5. — 🧩 PLANNED

Public Web UI — Pages & Features

W020 — Index page — Hero animation of Hyper Hash Network; live stats summary. — 🧩 PLANNED

W021 — Network page — Topology diagram: Core ↔ Nodes ↔ Miners; latency heatmap. — 🧩 PLANNED

W022 — Pool page — Real-time hashrate, blocks, payouts, lane colour breakdown. — 🧩 PLANNED

W023 — Miner page — Table of miners (by pubkey/IP); best-hash “game” display. — 🧩 PLANNED

W024 — Node page — List of public Hyper Nodes; uptime, region, TP mode, LN channel state. — 🧩 PLANNED

W025 — Info pages — /info-(network|pool|miner|node|financials) static markdown render. — 🧩 PLANNED

W026 — Search & filter — Fuzzy search across miners/nodes; debounce 300 ms. — 🧩 PLANNED

W027 — Live data polling — WebSocket subscribe to Core metrics; 5 s refresh fallback. — 🧩 PLANNED

W028 — Chart suite — Recharts line/bar for hashrate, block times, payouts. — 🧩 PLANNED

W029 — Error handling — Offline banner; API timeout retry; skeleton loaders. — 🧩 PLANNED

Hyper Node MMI (Local Interface)

W040 — Node dashboard — Summary: uptime, TP mode, LN balance, eligibility. — 🧩 PLANNED

W041 — Lightning panel — Channel list, balances, open/close/rebalance buttons. — 🧩 PLANNED

W042 — Reinvest slider — 0–100 %; immediate API update; visual yield projection. — 🧩 PLANNED

W043 — Withdraw flow — Enter sats → invoice → submit → success toast. — 🧩 PLANNED

W044 — TP/Translator switch — Toggle local ↔ remote; confirmation dialog. — 🧩 PLANNED

W045 — Service controls — Start/stop/restart node components via Agent API. — 🧩 PLANNED

W046 — Update checker — Poll Core for new release; “Update” button; spinner state. — 🧩 PLANNED

W047 — Eligibility tracker — Visual bar: uptime %, LN channel status, sync OK. — 🧩 PLANNED

W048 — Log viewer — Paginated stream; filters: agent, ln, tp, translator. — 🧩 PLANNED

W049 — Settings export — Download full node config + keys summary (redacted). — 🧩 PLANNED

APIs & Data Integration

W060 — API client SDK — Typed wrapper for /v1/core, /v1/telemetry, /v1/treasury. — 🧩 PLANNED

W061 — Auth flow — JWT or mTLS session; refresh tokens; local storage. — 🧩 PLANNED

W062 — Rate-limit back-off — Exponential retry with cap; error toasts. — 🧩 PLANNED

W063 — Metrics aggregation — Client-side smoothing for chart continuity. — 🧩 PLANNED

W064 — Time-series caching — IndexedDB local cache; 24 h expiry. — 🧩 PLANNED

Telemetry, Logging, and Analytics

W070 — Prometheus scrape — Static exporter for Nginx metrics. — 🧩 PLANNED

W071 — Front-end telemetry — Send page-load times and WebSocket latency to Core. — 🧩 PLANNED

W072 — Structured logs — JSON log of user actions (non-PII) for debugging. — 🧩 PLANNED

W073 — Alert banner integration — Show Core-pushed alerts (maintenance, payouts). — 🧩 PLANNED

Security & Compliance

W080 — HTTPS enforcement — HSTS, CSP, no-sniff, referrer-policy headers. — 🧩 PLANNED

W081 — CSRF protection — Double-submit cookie or SameSite =strict. — 🧩 PLANNED

W082 — Input validation — Client-side + server schema for all forms. — 🧩 PLANNED

W083 — Content signing — Verify signed data from Core before render. — 🧩 PLANNED

W084 — Supply-chain audit — npm-audit + Snyk scan; lockfile review in CI. — 🧩 PLANNED

Verification & Testing (Web UI)

W090 — Unit tests — Components, utils, reducers; > 80 % coverage. — 🧩 PLANNED

W091 — Integration tests — API mock → UI render → state update. — 🧩 PLANNED

W092 — E2E tests — Cypress flows: login, payout view, TP toggle, withdraw. — 🧩 PLANNED

W093 — Load test — k6 scenario 500 users; p95 < 300 ms. — 🧩 PLANNED

W094 — Accessibility audit — Axe CLI + manual screen-reader pass. — 🧩 PLANNED

W095 — Cross-browser — Chrome, Edge, Firefox latest; Safari ≥ 17. — 🧩 PLANNED

W096 — Security tests — XSS, CSRF, open-redirect scans. — 🧩 PLANNED

Documentation (Web UI)

W100 — README — Build, run, deploy steps; local + prod examples. — 🧩 PLANNED

W101 — API reference — SDK usage, pagination, errors. — 🧩 PLANNED

W102 — Theme guide — Lane colours, fonts, icons. — 🧩 PLANNED

W103 — MMI manual — Screenshots + control explanations. — 🧩 PLANNED

W104 — Troubleshooting — Common API errors, reconnects, config mismatches. — 🧩 PLANNED
