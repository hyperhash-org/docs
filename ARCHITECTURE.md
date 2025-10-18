HYPER HASH NETWORK — SYSTEM ARCHITECTURE (VERSION 0)
1. OVERVIEW

The Hyper Hash Network is designed as a modular, horizontally scalable Bitcoin mining ecosystem built around Stratum V2, Lightning Network settlement, and distributed Template Providers.
It replaces traditional monolithic mining pool structures with a federated system of interconnected Hyper Nodes managed through a Core control layer.

Each plane of the network — Core, Hyper Node, User, and Control — interacts through well-defined APIs and encrypted channels, ensuring decentralization, transparency, and efficient mining coordination.

2. ARCHITECTURAL PRINCIPLES

Decentralization
Mining is distributed across independent Hyper Nodes that maintain their own Bitcoin and Lightning infrastructure.

Transparency
All share accounting, fee routing, and treasury payments are observable through public APIs and Lightning invoices.

Security
Every connection is authenticated via mutual TLS and JSON Web Signatures (JWS). Sensitive data and keys are managed using SOPS and KMS encryption.

Compatibility
Supports both Stratum V2 (native) and Stratum V1 (legacy) miners via the Translator proxy.

Efficiency
Template Providers minimize propagation delay and improve network-wide hash utilization through localized block-template generation.

Self-Custody
Every Hyper Node maintains its own Lightning wallet and can choose to reinvest or withdraw its earnings autonomously.

3. HIGH-LEVEL LAYER MODEL

The architecture is divided into four primary planes:

Core Plane — The central coordination layer that manages pool logic, fee routing, and network control.

Hyper Node Plane — The decentralized mining edges, each capable of local block-template generation and Lightning settlement.

User Plane — The miners connecting via Stratum V2 or V1, contributing hashrate to the network.

Control Plane — The operational and monitoring layer providing management, observability, and user interfaces.

4. CORE PLANE

The Core Plane is composed of several tightly integrated components:

Bitcoin Node Cluster
Runs a combination of full and pruned nodes. Provides raw block templates to Template Providers through local RPC or IPC connections.

Template Provider (hh-tp)
Converts Bitcoin block templates into Stratum V2 compatible templates. Multiple TPs can run in parallel, load-balanced behind a single virtual IP address for redundancy.

Pool Server (hh-pool)
The central Stratum V2 pool coordinator. Responsible for:

Share validation and difficulty management.

Round tracking and share aggregation.

Block discovery and coinbase transaction formation.

Lightning payout calculation and initiation.

Translator (hh-translator)
A proxy service translating between Stratum V1 and Stratum V2 protocols, enabling compatibility with legacy ASIC miners.

Core API (hyperhash-core)
The control and telemetry hub of the system. Provides REST and gRPC endpoints for:

Node registration and authentication.

MMI commands (service toggles, wallet management, upgrades).

Metrics and telemetry aggregation.

Lightning payout reporting.

Treasury (hh-treasury)
A Lightning-enabled wallet service that:

Receives pool fees.

Splits and routes the 1% fee (0.5% Treasury, 0.5% Nodes).

Pays nodes via Lightning invoices.

Tracks LN liquidity and yields.

Observability Stack
Prometheus for metrics, Grafana for visualization, and Loki for log aggregation.
Provides uptime, hash rate, Lightning channel, and financial telemetry.

Core UI
Web dashboard for operators, exposing system health, pool statistics, node performance, and Lightning wallet balances.

5. HYPER NODE PLANE

Each Hyper Node operates as a self-contained edge that performs several functions locally:

Local Bitcoin Node
Optional but recommended. Used when running in “Local Template Provider” mode to reduce latency.

Template Provider
Generates Stratum V2 templates directly from the local Bitcoin node.
If disabled, the node pulls templates from the Core TP.

Translator (optional)
Provides a local SV1 port for older miners.

Node Agent (hh-node-agent)
The supervisory service that:

Manages all local Docker containers and services.

Authenticates Core-signed commands.

Exposes a local control socket for MMI actions.

Collects and pushes telemetry to Core.

Lightning Daemon (lnd or cln)
Handles wallet operations, Lightning channels, fee routing, and yields.
The node must maintain at least one open channel to the Core Treasury to receive payouts.

MMI Interface
Provides node owners with:

Service management (start, stop, restart).

Lightning wallet control (reinvest, withdraw, rebalance).

Security actions (key rotation, token regeneration).

System updates and logs.

Public vs Private Nodes

Public Nodes participate in the open network and serve third-party miners.

Private Nodes are run by independent operators or partners who connect to the Hyper Hash Core for settlement.

6. USER PLANE (MINERS)

Miners connect through either:

Native Stratum V2 (preferred), or

Stratum V1 via the Translator proxy.

Each miner contributes shares, which are validated and aggregated by the Pool Server.

Reward model:

The block subsidy is distributed among all miners based on share contribution.

The winning miner receives 100% of the transaction fees for that block.

Pool fee is fixed at 1% (0.5% to Treasury, 0.5% to Node Network).

This hybrid reward model combines fairness with performance incentives for finding valid blocks.

7. CONTROL PLANE

The Control Plane consists of:

Core API (command interface)

MMI (Management and Monitoring Interface)

Observability tools

It governs every operational aspect:
• Node registration and authentication
• Lightning channel management
• Service control and upgrades
• Real-time telemetry streaming
• Operator dashboards and notifications

Communication between Core and Nodes uses:

mTLS for authentication

JSON Web Signatures (JWS) for command integrity

Encrypted telemetry payloads over gRPC streams

8. NETWORK FLOWS

Mining Flow

Miners connect → Pool issues jobs → Shares returned → Pool validates → Core tracks round → Block found → Pool notifies Core → Coinbase formed and submitted → Rewards calculated.

Template Flow

Bitcoin node builds candidate template → Template Provider converts → Stratum V2 template broadcast to miners via Pool or Node.

Lightning Flow

Pool fee and block rewards collected → Core Treasury calculates node payouts → Nodes submit LN invoices → Treasury pays invoices → Node updates telemetry and optionally reinvests.

Telemetry Flow

Node Agent aggregates metrics → Pushes to Core API → Core aggregates → Prometheus scrapes → Grafana displays → Alerts via Alertmanager.

Control Flow

Operator action (via UI or MMI) → Signed Core command → Node Agent executes → Returns status update.

9. SECURITY AND TRUST MODEL

Authentication

mTLS mutual authentication between all nodes and the Core.

Node identities issued via registration tokens signed by Core.

Integrity

All control commands signed using JSON Web Signatures (JWS).

Encryption

TLS 1.3 for all external communication.

SOPS + KMS for stored secrets and credentials.

Access Control

Fine-grained permissions per role: Miner, Node Operator, Core Operator.

Auditing

All Lightning payments logged with full traceability.

Immutable audit log for all critical actions.

10. DEPLOYMENT ARCHITECTURE

Core Deployment

Multiple redundant Core servers behind a load balancer.

Stateless API services with persistent PostgreSQL backend for accounting.

Treasury node runs on a dedicated VM with secure key storage.

Template Providers distributed geographically.

Node Deployment

Each node runs in Docker Compose or systemd-managed containers.

MMI and API exposed on port 8442 (local).

Optional auto-update using Core-signed releases.

Network Segmentation

Core services on private network with only Stratum and HTTPS ports exposed.

Nodes communicate outbound only (pull-based control channel).

Treasury node segregated in its own VLAN for Lightning isolation.

High Availability

Core replicated in at least two data centers.

Treasury redundancy through Lightning multi-node topology.

Observability components in separate monitoring subnet.

11. DATA STRUCTURE OVERVIEW

Key data domains:

• Miner Shares — Job ID, nonce, timestamp, difficulty, validity.
• Node Metrics — Hashrate, latency, template version, uptime.
• Lightning Accounting — Invoice ID, sats amount, payment hash, node ID, timestamp.
• Treasury State — Channel balances, pending HTLCs, routing fees earned.
• Template Cache — Serialized SV2 templates for reuse across jobs.
• System Logs — Structured JSON logs ingested into Loki.

12. OBSERVABILITY AND ALERTING

Metrics include:

Hashrate per node and per lane.

Template generation latency.

Lightning channel health.

Pool payout latency.

Node uptime and software version.

Alerts trigger for:

Offline nodes.

Failed Lightning payments.

Outdated node software.

Excessive template latency.

Hashrate drop exceeding threshold.

13. SCALABILITY STRATEGY

Horizontal Scaling

Template Providers and Pool Servers can scale out without downtime.

Regional Sharding

Public Hyper Nodes grouped by region for low latency and redundancy.

Load Balancing

Core API and Pool Servers load-balanced with sticky sessions.

Caching

Template and share caches reduce RPC overhead.

Async Processing

Event-driven Lightning settlements and telemetry pipelines.

14. EXTENSIBILITY

Future integrations may include:

Edge relay nodes to improve propagation.

Miner-side SV2 firmware (Bitaxe, Braiins OS integration).

Dynamic lane assignment for optimized header distribution.

Treasury mesh with multi-node LN liquidity.

Plug-in system for node analytics and visualizations.

All components expose APIs and hooks for expansion.

15. SUMMARY OF KEY INTERFACES

Stratum V2 Port: Mining job exchange.

Core REST/gRPC API: Node control, telemetry, and payouts.

Lightning API: Channel and invoice operations.

MMI Interface: WebSocket/REST interface for operator control.

Telemetry Exporter: Prometheus metrics endpoints.

Web UI: User dashboard and visualization.

16. PERFORMANCE TARGETS

• Template generation latency: < 50 ms average.
• Block propagation time: < 500 ms between Core and all nodes.
• Node telemetry update interval: 5 seconds.
• Lightning payout completion: < 30 seconds average.
• Node uptime SLA: 99.9%.

17. LICENSE AND ATTRIBUTION

Documentation © 2025 Hyper Hash Network
Licensed under Creative Commons Attribution 4.0 International (CC BY 4.0)
https://creativecommons.org/licenses/by/4.0/

END OF DOCUMENT — HYPER HASH NETWORK SYSTEM ARCHITECTURE (VERSION 0)
