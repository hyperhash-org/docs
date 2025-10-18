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
