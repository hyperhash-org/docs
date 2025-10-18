HYPER HASH NETWORK — OPERATIONS AND MAINTENANCE MANUAL (VERSION 0)
1. PURPOSE

This document defines the operational standards for running the Hyper Hash Network—covering Core, Treasury, and Hyper Node maintenance, monitoring, backup, and incident response.
It ensures every instance remains secure, stable, and performant.

2. OPERATIONAL PHILOSOPHY

Reliability First – Maintain 99.9 % uptime.

Security by Default – Least-privilege access and encryption everywhere.

Automation Everywhere – Systemd, Docker Compose, and GitHub Actions reduce manual intervention.

Observability – All metrics and logs are centralized and retained.

Transparency – Every operational event is logged and auditable.

3. ROLES AND RESPONSIBILITIES

Hyper Hash (Core Operator) – Maintains Core servers, Pool, Template Providers, Treasury, and observability stack; signs node registrations and software updates; publishes uptime and audit reports.
Hyper Node Operators – Maintain uptime, Lightning wallets, Bitcoin nodes, and apply signed updates.
Miners – Connect to preferred Hyper Node or Pool endpoint and maintain stable SV2 sessions.

4. SERVICE SUPERVISION

All services run under systemd or Docker Compose with auto-restart policies.
Typical service names:
hh-core, hh-pool, hh-tp, hh-translator, hh-treasury, hh-node-agent, hh-bitcoind, hh-lnd, hh-observability.

Check status with sudo systemctl status hh-* or sudo docker ps.

5. MONITORING AND ALERTS

Stack: Prometheus + Grafana + Alertmanager + Loki.
Key metrics include node uptime, share rate, template latency, Lightning balances, CPU/memory, disk I/O, and latency.

Alert thresholds:
• Node offline > 5 min
• Channel capacity < 0.1 BTC
• Template latency > 500 ms
• CPU > 90 % > 10 min
• Disk usage > 85 %
• Hashrate drop > 20 % from 24 h avg

Alerts feed to Discord #alerts and email fallback for Core ops.

6. LOGGING POLICY

Logs reside in /var/log/hyperhash/ or Docker volumes.
Rotation daily; 14 days local, 90 days in Loki.
Sensitive keys never logged. Audit events mirrored to Core ledger.
Inspect with journalctl -u hh-pool -n 200 or docker logs hh-node-agent.

7. BACKUP AND RESTORE

Core – PostgreSQL ledger, Treasury wallet seed, TLS certs, SOPS keys, configs.
Nodes – Lightning seed, node config, bitcoind data, logs.
Backups daily; wallet seed after each change.
Restore by deploying fresh VM, restoring files, restarting services, and verifying channels.

8. SOFTWARE UPGRADES

Hyper Hash publishes signed release manifest.

Node Agent polls hourly.

Verify signature → apply update → health check → rollback on failure.
Manual: sudo hh-node-agent upgrade --verify-signature.

9. SECURITY OPERATIONS

SSH keys only; Fail2ban and UFW enabled; TLS rotation 90 days; Lightning macaroons 30 days; 3-strike login lockout; Core Treasury in offline multisig.
Incident steps: isolate → snapshot → rotate credentials → notify Discord #operations → publish report within 48 h.

10. ROUTINE MAINTENANCE SCHEDULE
Task	Frequency	Responsible
System updates	Weekly	Node Op
Docker refresh	Weekly	Node Op
Log rotation check	Weekly	Node Op
TLS renewal	Quarterly	Hyper Hash
Seed backup verify	Monthly	Both
LN rebalance	Daily	Treasury
DB vacuum	Weekly	Core Ops
Dashboard audit	Monthly	Hyper Hash
11. HEALTH CHECK COMMANDS

Core → curl https://core:8442/v1/health systemctl status hh-* bitcoin-cli getblockcount lncli getinfo
Node → curl http://localhost:8442/v1/status docker ps lncli listchannels bitcoin-cli getblockchaininfo

12. INCIDENT CLASSIFICATION

• Severity 1 – Critical: Mining or payout halted.
• Severity 2 – Major: Multiple nodes offline > 30 min.
• Severity 3 – Minor: UI glitch or delayed telemetry.
All incidents logged with timestamp, cause, and resolution on GitHub.

13. CAPACITY MANAGEMENT

Monthly review of hashrate, liquidity, node count, storage, and alerts.

80 % utilization → infrastructure expansion triggered.

14. OPERATOR CHECKLISTS

Daily – Check Grafana, Treasury channels, payout success, system resources.
Weekly – Review logs, verify autopilot, validate share latency.
Monthly – Wallet seed check, backup restore test, audit trail review.

15. DISASTER RECOVERY PLAN

Core – Promote standby instance, redirect DNS/LB, verify pool connectivity.
Treasury – Promote standby Treasury with synced channels.
Node – Redeploy from backup, re-open channel, re-register.
RTO = 1 h, RPO = 10 min.

16. PERFORMANCE BASELINES

Template latency < 50 ms; RTT < 250 ms; telemetry ≤ 5 s; payout ≤ 30 s; uptime ≥ 99.9 %.
Deviation triggers alert.

17. DOCUMENTATION AND CHANGE CONTROL

All Ops changes committed to /ops/changes/.
Include description, risk, rollback plan. Emergency fixes logged within 24 h.
ID format: Ops-YYYY-MM-DD-##.

18. TRAINING AND ACCESS CONTROL

Annual security and Lightning training mandatory.
Roles: Core Ops, Node Ops, Treasury Ops, Observer.
Access review quarterly; credentials expire after 90 days or personnel change.

19. COMMUNICATION CHANNELS ✅ (REVISED)

All official operations and incident communications occur exclusively through Discord and GitHub.

Discord (Primary Operations Hub)
Used for live coordination, alerts, and support.
Official link: https://discord.com/channels/1409546566116839490/1410138982204838009

Key channels:
• #operations – real-time ops and incident response
• #alerts – automated uptime and service events
• #dev-log – release and CI/CD updates
• #miners – general support

GitHub (Issue and Change Tracking)
All tickets, code updates, and documentation changes tracked via
https://github.com/hyperhash-org

Operators must log incidents and link commits or PRs to resolution records.
No other communication platforms are recognized for official operations.

20. CONTINUOUS IMPROVEMENT

Hyper Hash maintains a continuous feedback loop between operators, developers, and miners.
Post-incident reviews feed into development sprints. Quarterly uptime and performance reports are published to GitHub and Discord.
Documentation is revised immediately after major events.

21. LICENSE AND ATTRIBUTION

Documentation © 2025 Hyper Hash Network
Licensed under Creative Commons Attribution 4.0 International (CC BY 4.0)
https://creativecommons.org/licenses/by/4.0/

END OF DOCUMENT — HYPER HASH NETWORK OPERATIONS AND MAINTENANCE MANUAL (VERSION 0)
