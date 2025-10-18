HYPER HASH NETWORK — FINANCIAL MODEL AND TREASURY SPECIFICATION (VERSION 0)
1. PURPOSE

This document defines the complete financial structure of the Hyper Hash Network:
how mining rewards are distributed, how fees are managed, how Lightning payments are executed, and how yield compounding is achieved.
It establishes the economic foundation of the network for operators, miners, and node owners.

2. OBJECTIVES

Guarantee transparent, mathematically verifiable reward distribution.

Maintain decentralized custody of Lightning funds.

Ensure sustainable liquidity growth through controlled reinvestment.

Provide open-ledger accountability between the Hyper Hash Treasury and all Nodes.

3. POOL FEE STRUCTURE

• Total Pool Fee: 1 percent of every block subsidy.
• Fee Distribution:

0.5 percent → Hyper Hash Treasury (Core Wallet).

0.5 percent → Node Network (distributed evenly among eligible Hyper Nodes).
• The transaction fees contained within a block are not included in the pool fee;
they belong entirely to the winning miner.

4. MINER REWARD MODEL

The network uses a Proportional Share Distribution (PPLNS) model.

Miners submit shares during each block-finding round.

When a valid block is discovered, the block subsidy is divided proportionally among all participating miners according to their valid shares.

The winning miner keeps 100 percent of the transaction fees contained in that block.

The Core automatically calculates share weights and issues payout records to the Treasury.

This hybrid design rewards consistent hashrate while incentivizing rapid block discovery.

5. NODE REWARD MODEL

Every registered Hyper Node receives Lightning payments from the Treasury as part of the Node Network Reward Pool.
This pool represents 0.5 percent of the block subsidy (the Node Network’s half of the pool fee).

Distribution Logic:

• The Node Reward Pool (0.5%) is split evenly among all registered Hyper Nodes that meet operational and uptime requirements.
• Eligibility conditions for each payout cycle (default: 7 days):

Node uptime of at least 95 percent.

At least one open Lightning channel connected to the Treasury.

A fully synchronized and responsive Bitcoin node.
• Nodes failing these requirements forfeit their reward for that cycle. Their portion is redistributed evenly among qualifying nodes.
• All payments are made via Lightning invoices issued by each node and settled by the Treasury.
• Each node also earns additional routing-fee income from forwarding Lightning payments across the network.

This structure ensures fairness, rewards reliability, and maintains predictable network expenditure.

6. TREASURY DESIGN

The Hyper Hash Treasury is a dedicated Lightning Network node operated by Hyper Hash.
It performs the following roles:

Collects the total 1% pool fee from each block.

Splits and routes payouts (0.5% Hyper Hash / 0.5% Node Network).

Executes Lightning payments to eligible nodes.

Manages routing liquidity, rebalancing, and yield tracking.

Provides public transparency through telemetry and signed payout records.

The Treasury operates from a secured VM with limited external exposure, multi-signature key control, and cold backups of its wallet seed.

7. PAYMENT FLOW

Pool Server notifies Core when a block is found or a payout window closes.

Core computes share-based miner payouts and Node Network rewards.

Core generates a signed payout manifest and transmits it to the Treasury.

Treasury requests invoices from each eligible node.

Nodes issue invoices via Lightning.

Treasury verifies, pays, and logs every transaction.

Confirmation data is transmitted back to Core and displayed on the UI.

Average Lightning payout latency: under 30 seconds per transaction.

8. YIELD MANAGEMENT

Each Hyper Node controls how its Lightning wallet income is handled:

Reinvest Mode (ON):
• Automatically adds routing fees and received payouts to channel liquidity.
• Increases future routing income and network liquidity capacity.
• Managed by Node Agent via the MMI interface.

Withdraw Mode (OFF):
• Accumulated income remains in the Lightning wallet for manual withdrawal.
• Withdrawals can be made via Lightning invoice or on-chain address.
• Daily withdrawal limits are enforced by the Core Treasury.

Default for new nodes: Reinvest Mode ON at 50% allocation.

9. LIGHTNING CHANNEL POLICY

Treasury Channel Settings:
• Base fee: 1 sat per kB
• Fee rate: 0.002% per sat transferred
• Minimum HTLC size: 100 sats
• Maximum HTLC size: 16 million sats
• Auto-rebalance interval: 1 hour

Node operators can customize their own local routing policies to increase yield,
but payout channels connected to the Treasury must remain within safe liquidity thresholds.

10. ACCOUNTING FRAMEWORK

The Core maintains a PostgreSQL ledger storing:

• Block height and hash
• Block subsidy and total transaction fees
• Miner share weights and payout amounts
• Node eligibility and uptime metrics
• Lightning invoice IDs, payment hashes, timestamps, and confirmations
• Treasury balances, routing income, and reinvestment records

All transactions are cross-verified between Core and Treasury before being finalized.

11. REPORTING AND TRANSPARENCY

• Each block payout produces a signed CSV report with:
Node ID, Miner Address, Share Weight, Reward Amount, TX Hash, and Invoice ID.
• Reports are published to the public dashboard.
• Prometheus exporters expose Treasury metrics for Grafana visualization.
• Historical data retained for a minimum of ten years.

12. SECURITY AND AUDIT

• All manifests and payment instructions are signed by Hyper Hash’s private key (JWS).
• Nodes verify signatures before accepting payments.
• Multi-signature cold backups secure Treasury wallet seeds.
• Continuous anomaly detection monitors for discrepancies in invoice totals or routing volume.
• Quarterly audits verify pool-fee allocation accuracy and Lightning balance reconciliation.

13. TREASURY YIELD COMPOUNDING

The Treasury earns routing fees through its Lightning channels.
Yield accumulation process:

Routing profits are tallied hourly.

Profits are automatically reallocated into liquidity to compound growth.

10 percent of yield is re-invested automatically to expand capacity.

The remaining 90 percent supports operational and infrastructure funding.

Target annualized Lightning yield: 3–6 percent in BTC equivalent.

14. NODE FINANCIAL TELEMETRY

Each Hyper Node reports telemetry to Core every five seconds, including:

• Lightning wallet balance
• Routing fees earned
• Reinvestment ratio
• Uptime percentage
• Last payout timestamp
• Channel capacity and status

This information powers the Hyper Hash dashboard and enables reward validation and payout auditing.

15. FEES AND COST RECOVERY

• Hyper Hash: Receives 50 percent of the 1 percent pool fee (0.5%). This covers Treasury operations, infrastructure, maintenance, and ongoing network development.
• Node Network: Collectively receives the remaining 0.5 percent of the pool fee, distributed evenly among eligible Hyper Nodes (see Section 5).
• Miners: Receive their proportional share of the full block subsidy (based on submitted shares). The winning miner additionally keeps 100 percent of transaction fees.
• Treasury Operations: Funded by Hyper Hash’s 0.5 percent Core share and routing income earned through Treasury channel liquidity.

This balanced distribution aligns incentives across all network participants — Hyper Hash ensures stability and growth, Hyper Nodes uphold decentralization and reliability, and miners secure Bitcoin blocks.

16. FINANCIAL GOVERNANCE

• Treasury keys controlled by a three-signature multisig wallet (2-of-3 quorum).
• Payout algorithms and fee rates reviewed quarterly.
• Annual report of revenue, yield performance, and expenses published publicly.
• Governance actions logged and archived for transparency.

17. INCENTIVE ALIGNMENT

The Hyper Hash economic model ensures:

• Miners are rewarded consistently based on performance.
• Nodes are rewarded equally for uptime and connectivity.
• Treasury benefits from Lightning routing yield.
• No entity can unilaterally alter payouts or distributions without public audit visibility.

18. FAILURE SCENARIOS AND SAFEGUARDS

Node Offline:
Rewards paused; Lightning invoice retried for 72 hours.

Channel Full or Closed:
Node flagged; payout deferred until next cycle.

Treasury Downtime:
Secondary Treasury node takes over using synchronized ledger state.

Ledger Mismatch:
Triggers reconciliation and manual review before next payout batch proceeds.

19. FUTURE FINANCIAL EXTENSIONS

Regional Treasury nodes for geographic redundancy.

Node-to-node micro-settlement routing (LN mesh payouts).

Integration with Lightning liquidity marketplaces for external yield.

Smart-contract based payout proofs recorded on-chain.

Extended Treasury analytics dashboard with liquidity forecasting.

20. PERFORMANCE METRICS

• Average Lightning payout time: under 30 seconds
• Treasury reconciliation interval: 1 hour
• Channel rebalance latency: under 5 minutes
• Node telemetry frequency: every 5 seconds
• Treasury uptime target: 99.9 percent

21. SUMMARY

The Hyper Hash Network’s financial system merges Bitcoin mining economics with Lightning Network liquidity management.
It guarantees instant, auditable, and decentralized payouts, while compounding network liquidity to grow the Treasury.
Every participant — Hyper Hash, Hyper Nodes, and miners — has aligned economic incentives tied directly to network stability and uptime.

22. LICENSE AND ATTRIBUTION

Documentation © 2025 Hyper Hash Network
Licensed under Creative Commons Attribution 4.0 International (CC BY 4.0)
https://creativecommons.org/licenses/by/4.0/

END OF DOCUMENT — HYPER HASH NETWORK FINANCIAL MODEL AND TREASURY SPECIFICATION (VERSION 0)
