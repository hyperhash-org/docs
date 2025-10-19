HYPER HASH NETWORK — REPOSITORY DIRECTORY
Version 0 (Canonical Reference)
October 2025

This document lists every repository maintained under the Hyper Hash Network organization on GitHub.
It defines each repository’s role, visibility, license, and purpose within the overall system architecture.
All repository links in official documentation and changelogs should refer back to this file as the master index.

INDEX

Core Components
1.1 core
1.2 pool
1.3 tp
1.4 translator

Node Layer
2.1 node-agent
2.2 hyper-node-public
2.3 hyper-node-private

Support and Infrastructure
3.1 ci
3.2 overlay
3.3 treasury
3.4 bitcoin

User Experience and Documentation
4.1 ui
4.2 docs

CORE COMPONENTS

1.1 REPOSITORY: core
Description: Central coordination layer of the Hyper Hash Network. Handles pool orchestration, share accounting, node directory services, payout manifests, Lightning Treasury linkage, and telemetry aggregation.
Visibility: Public
License: AGPL-3.0
Maintainers: @caldefenwycke, @ops-team
Notes: This repository represents the network’s “brain” and manages all authenticated communication with pool, TP, and node layers.

1.2 REPOSITORY: pool
Description: Implements Stratum v2 pool logic. Connects ASIC miners, validates shares, aggregates hashrate, and interacts with the Template Provider for block template construction.
Visibility: Public
License: AGPL-3.0
Maintainers: @caldefenwycke, @stratum-team
Notes: All miner statistics, pool rewards, and payout logic are derived from this module.

1.3 REPOSITORY: tp
Description: Fork and wrapper of Sjors Provoost’s Template Provider implementation. Interfaces with Bitcoin Core for template generation, connects upstream to the Hyper Hash Core, and supports both local and remote configurations for nodes.
Visibility: Public
License: AGPL-3.0
Maintainers: @caldefenwycke, @tp-team
Notes: Acts as the block template engine for both the public and private Hyper Nodes.

1.4 REPOSITORY: translator
Description: Fork and wrapper of the SRI Stratum Translator (SV1 to SV2). Provides backwards compatibility for legacy ASICs and connects them into the Hyper Hash Network via the Stratum v2 pool.
Visibility: Public
License: AGPL-3.0
Maintainers: @caldefenwycke, @sv2-team
Notes: Optional module; included in Hyper Node deployments to support older miners.

NODE LAYER

2.1 REPOSITORY: node-agent
Description: The on-node daemon responsible for orchestrating all node services (Template Provider, Translator, Lightning wallet, and Bitcoin node). Exposes the local MMI API and enforces eligibility for node rewards.
Visibility: Public
License: AGPL-3.0
Maintainers: @node-team
Notes: Required for all Hyper Node types (both public and private).

2.2 REPOSITORY: hyper-node-public
Description: Fleet definitions for Hyper Hash–operated nodes distributed worldwide to minimize miner latency. Includes deployment manifests, runtime policies, observability hooks, and configuration for internal monitoring.
Visibility: Private
License: Hyper Hash Proprietary License
Maintainers: @ops-team
Notes: Operated directly by Hyper Hash. These nodes are not user-deployable and form the production network’s public entry layer.

2.3 REPOSITORY: hyper-node-private
Description: Self-hostable Hyper Node package for individuals, partners, and institutions. Docker-based installation including Node Agent, Template Provider, Translator, and Lightning wallet integration. Offers configurable local or remote TP and full MMI interface.
Visibility: Public
License: MIT
Maintainers: @caldefenwycke, @community
Notes: Used by community operators to participate in the Hyper Hash Network and earn Lightning-based node rewards.

SUPPORT AND INFRASTRUCTURE

3.1 REPOSITORY: ci
Description: Shared GitHub Actions, build workflows, signing keys, SBOM generation, and release automation templates. Used across all other Hyper Hash repositories for continuous integration.
Visibility: Private
License: Hyper Hash Proprietary License
Maintainers: @ci-team
Notes: This repository must remain private to protect internal signing secrets.

3.2 REPOSITORY: overlay
Description: Reserved future module for mesh and edge relay nodes forming the “Overlay Network”. Designed for propagation optimization and long-term scalability.
Visibility: Public
License: AGPL-3.0
Maintainers: @overlay-team
Notes: Not yet active in MVP0; planned for future expansion.

3.3 REPOSITORY: treasury
Description: Lightning Treasury and financial logic module. Manages LN channels, node yield distribution, fee splits, and treasury wallet reporting to Core.
Visibility: Private
License: Hyper Hash Proprietary License
Maintainers: @finance-team
Notes: Interfaces directly with Core for all Lightning transactions and payout accounting.

3.4 REPOSITORY: bitcoin
Description: Bitcoin Core v30 fork configured for Template Provider compatibility and operational parity. Used for both Core and Hyper Node deployments.
Visibility: Public
License: MIT
Maintainers: @bitcoin-team
Notes: Maintains patch-level synchronization with upstream Bitcoin Core while supporting Template Provider RPC extensions.

USER EXPERIENCE AND DOCUMENTATION

4.1 REPOSITORY: ui
Description: Unified web interface for both the Pool Dashboard and the Hyper Node Management Interface (MMI). Built using React + Tailwind + Framer Motion. Connects to Core, Treasury, and Node-Agent APIs.
Visibility: Public
License: MIT
Maintainers: @ui-team
Notes: Also used for public-facing pool statistics and miner dashboards.

4.2 REPOSITORY: docs
Description: Canonical documentation hub for the Hyper Hash Network. Contains architecture diagrams, execution roadmap, financial model, deployment guides, and changelogs.
Visibility: Public
License: MIT
Maintainers: @docs-team
Notes: All other repos link here for official technical documentation and governance references.

GOVERNANCE AND POLICY

• All public repositories must reference this file in their README footer.
• Internal proprietary repositories (ci, treasury, hyper-node-public) follow restricted access controls and require key-based authentication.
• Repository creation or deletion requires approval by the Core Maintainers group.
• Any repository affecting financial logic or payout distribution must undergo formal audit before merging to main.
• Documentation updates referencing repositories should modify both this file and the master ROADMAP.

END OF DOCUMENT
REPOSITORIES.MD — Hyper Hash Network
Version 0 — Canonical Reference
Maintainers: @caldefenwycke, @ops-team
Last Updated: October 2025
