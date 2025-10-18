HYPER HASH NETWORK — DEPLOYMENT GUIDE (VERSION 0)
1. PURPOSE

This document describes the complete deployment procedure for the Hyper Hash Network, covering Core Plane servers, Hyper Nodes (public and private), Lightning Treasury configuration, and supporting infrastructure.

It defines environments, dependencies, networking requirements, installation steps, and operational verification commands.

The goal is to ensure every component can be deployed reproducibly on bare metal, virtual machines, or containerized environments.

2. DEPLOYMENT PHILOSOPHY

Reproducibility — Every build and configuration step is scriptable and versioned in Git.

Isolation — Each logical role runs within its own container or VM namespace.

Resilience — All services are supervised by systemd or Docker Compose with automatic restart policies.

Security — Only necessary ports are exposed; all inter-service traffic is encrypted.

Observability — Metrics and logs are exported from day one for health verification.

3. ENVIRONMENTS

Development / Regtest
• Runs entire stack on one machine.
• Used for CI and local testing.

Staging / Signet
• Publicly reachable test deployment mirroring production topology.
• Used for integration, wallet, and payout testing.

Production / Mainnet
• Fully redundant Core cluster, regional public nodes, and Lightning Treasury.
• All payments and telemetry live.

Each environment uses the same Docker Compose templates and configuration schema, differing only by environment variables and network settings.

4. SYSTEM REQUIREMENTS
Core Plane VM Specifications

• 4 vCPU
• 8 GB RAM
• 200 GB SSD storage
• Ubuntu 24.04 LTS (preferred)

Hyper Node VM Specifications

• 2 vCPU
• 4 GB RAM
• 100 GB SSD storage

Treasury Node Specifications

• 4 vCPU
• 8 GB RAM
• 100 GB SSD NVMe
• Static IP with firewall restrictions

Common Requirements

• Outbound HTTPS, SSH, Lightning ports (9735).
• Docker Engine v27+ and Docker Compose v2.
• Git, curl, Python 3, Node.js 18+ (for UI builds).
• Ports 8442 (Core API/MMI) and 34254 (SV2 pool) open to trusted sources.

5. NETWORK TOPOLOGY

Core Cluster — One primary Core API, one secondary replica, one load balancer.

Template Providers — At least two instances in different regions.

Treasury Node — Independent Lightning wallet VM connected via private VPN to Core.

Public Hyper Nodes — Deployed geographically close to miners; connect via TLS to Core.

Private Nodes — Partner-deployed; communicate outbound only.

Observability Stack — Central Prometheus + Grafana + Loki cluster reachable by operators only.

6. PORT MATRIX
Service	Port	Protocol	Direction	Notes
Stratum V2 (Pool)	34254	TCP	Inbound	SV2 miners → Pool
Translator (SV1)	34255	TCP	Inbound	Legacy miners
Core API + MMI	8442	TCP	Inbound HTTPS	Node and operator control
Prometheus Exporter	9100+	TCP	Inbound internal	Metrics
Lightning Node	9735	TCP	Inbound	LN peer connections
SSH Access	22	TCP	Restricted	Ops only
7. INSTALLATION SEQUENCE — CORE PLANE

Provision VMs and create user hyperhash-core.

Install Docker and Compose.

Clone repositories into /opt/hyperhash/:

git clone https://github.com/hyperhash-org/hyperhash-core.git
git clone https://github.com/hyperhash-org/hh-pool.git
git clone https://github.com/hyperhash-org/hh-tp.git
git clone https://github.com/hyperhash-org/hh-translator.git
git clone https://github.com/hyperhash-org/hh-treasury.git


Generate SSL certificates for Core and Nodes.

Configure Bitcoin Core (v30) in /opt/hyperhash/bitcoin/bitcoin.conf:

server=1
rpcuser=hyperhash
rpcpassword=<secure>
txindex=1
blockfilterindex=1


Start bitcoind as systemd service.

Build and launch Template Provider and Pool containers.

Deploy Treasury service with Lightning wallet initialized.

Launch Core API and MMI.

Verify status using curl https://127.0.0.1:8442/v1/health.

8. INSTALLATION SEQUENCE — HYPER NODE

Provision VM with Docker Engine.

Clone repository hh-hypernode-public or hh-hypernode-private.

Copy Core issued certificates into /etc/hyperhash/certs/.

Edit node-config.toml to set:
• Core endpoint and certificate paths
• TP mode (local or remote)
• Lightning daemon (lnd or cln)
• MMI credentials

Initialize Lightning wallet and open channel to Core Treasury:

lncli connect <core_pubkey>@<core_host>
lncli openchannel <core_pubkey> <amount>


Start Docker Compose stack:

docker compose up -d


Confirm Node registration appears in Core UI.

Run smoke test:

curl -s http://localhost:8442/v1/status

9. TEMPLATE PROVIDER CONFIGURATION

For Local Template Provider mode:

bitcoind IPC socket: /opt/hyperhash/bitcoin/ipc.sock

sv2 bind address: 127.0.0.1

sv2 port: 8442

logging flags: -debug=sv2 -debug=ipc

Example startup:

./build/bin/sv2-tp -chain=main \
 -ipcconnect=unix:/opt/hyperhash/bitcoin/ipc.sock \
 -sv2bind=127.0.0.1 -sv2port=8442 \
 -debug=sv2 -debug=ipc -printtoconsole

10. LIGHTNING TREASURY CONFIGURATION

Install lnd or cln on Treasury VM.

Generate Core Treasury node keys and macaroons.

Fund wallet with on-chain Bitcoin.

Open channels to active Hyper Nodes.

Set Treasury split logic:

0.5% of pool revenue → Treasury self

0.5% → Node Lightning wallets

Enable auto-rebalance interval (1 hour default).

Verify connectivity with lncli listchannels.

11. LIGHTNING WALLET SETUP ON NODE

Install lnd or cln container as part of node stack.

Create wallet seed and set password.

Connect to Core Treasury and open channel (min 1M sats).

Verify channel state with lncli listchannels.

Enable auto-reinvest or manual withdraw through MMI.

Confirm telemetry appears on Core dashboard.

12. SECURITY HARDENING

• Restrict SSH to key-based authentication.
• Rotate TLS certificates quarterly.
• Use fail2ban or UFW for brute-force protection.
• Isolate Lightning wallet from public access.
• Encrypt all configuration files with SOPS.
• Run container images with read-only filesystems.
• Limit Docker API socket to root and Node Agent only.

13. OBSERVABILITY SETUP

Deploy Prometheus and Grafana from hh-observability.

Add Core and Node exporter targets for:

Node Lightning balances

Pool share counts

Template latency

System CPU and memory usage

Import dashboard templates from /grafana/dashboards/.

Configure Alertmanager to notify Discord or email.

Validate metrics reach Grafana UI.

14. VALIDATION AND HEALTH CHECKS

Run after each deployment:

Check Core and Pool ports:
sudo ss -ltnp | grep 34254

Verify TP and Translator logs show no errors.

Ensure node registration appears in Core API:
curl https://core/v1/nodes

Check Lightning channel balances.

Submit test shares from miner and observe pool acceptance.

Confirm payout invoice settles over Lightning.

15. AUTOMATED UPGRADES

• Each service runs a watchdog that checks for new Git releases.
• Core issues signed release manifest (JWS).
• Node Agent validates signature and updates containers.
• Rollback is automatic if health check fails.
• MMI provides “Apply Upgrade” and “Rollback” buttons for operators.

16. BACKUP AND RECOVERY

Core
• PostgreSQL dump every 24 hours.
• Configuration and TLS certs archived to off-site storage.

Nodes
• Lightning wallet seed and channel state backed up securely.
• Node configuration snapshots taken daily.

Disaster Recovery
• Spin up replacement VM.
• Restore configs and wallet files.
• Re-register node via MMI.

17. POST-DEPLOYMENT CHECKLIST

☑ Bitcoin node synced 100%.
☑ Pool and Template Provider online.
☑ Nodes registered and reporting telemetry.
☑ Lightning Treasury funded and channels open.
☑ Metrics visible in Grafana.
☑ Alerts functional.
☑ Backups verified.

18. PERFORMANCE TUNING

• Enable CPU affinity for hash intensive tasks.
• Use SSD storage for blockchain and template cache.
• Tune bitcoind mempool and dbcache parameters.
• Increase Docker logging limits to retain 24 hours of history.
• Adjust Prometheus scrape interval to 5 seconds for real-time stats.

19. DECOMMISSION PROCEDURE

Gracefully stop miners and translator.

Close Lightning channels with Treasury.

Stop Docker services.

Backup wallet seeds and configs.

Remove certificates and revoke from Core.

Delete VM once final settlement confirmed.

20. TROUBLESHOOTING REFERENCE

• Logs stored under /var/log/hyperhash/.
• Common issues:

"StartLogging failed" → Check permissions on log directory.

"Channel unavailable" → Verify Lightning peer connection.

"No template received" → Confirm bitcoind IPC socket exists.

"API unauthorized" → Check certificates and registration token.
• Restart service: systemctl restart hh-*
• Review last 100 log lines: sudo journalctl -u hh-pool -n 100

21. LICENSE AND ATTRIBUTION

Documentation © 2025 Hyper Hash Network
Licensed under Creative Commons Attribution 4.0 International (CC BY 4.0)
https://creativecommons.org/licenses/by/4.0/

END OF DOCUMENT — HYPER HASH NETWORK DEPLOYMENT GUIDE (VERSION 0)
