# Zatvoon

### An open network for on-chain credit

Zatvoon is a protocol research and development initiative exploring permissionless credit markets with immutable terms, independent risk curation, and optional deployment of accrued interest into external strategies.

This repository hosts **Infrastructure Note 1**, the initial design narrative for the network.

[Read Infrastructure Note 1 →](./Zatvoon_Infrastructure_Note_1.pdf)

## Research thesis

Credit infrastructure should make market terms explicit and durable while allowing independent participants to exercise judgment over assets, pricing, and capital allocation.

Zatvoon's proposed architecture separates market mechanics from risk selection. Market creators define permanent terms; curators evaluate exposures; depositors choose which markets or curators to use. A shared yield router explores whether accrued interest can support incremental returns within defined exposure and liquidity constraints.

## Architecture

The following components describe the intended system.

| Component | Role |
| --- | --- |
| **Market factory** | Creates and registers isolated markets, enforcing constraints at creation. |
| **Credit markets** | Pair a lending asset with a collateral asset under fixed pricing, interest, borrowing, liquidation, and routing parameters. |
| **Curators** | Select markets and allocate deposits according to independent risk assessments. |
| **Yield router** | Allocates eligible accrued interest across external strategies and coordinates capital recall. |
| **Strategy framework** | Defines accounting, recall, collateral, and performance-measurement requirements for admitted strategies. |

### Market integrity

Market parameters are fixed at creation. The proposed market layer has no administrative override or discretionary pause; revised terms require a new market and voluntary migration.

Each market declares a maximum acceptable oracle age, capped at seven days. Stale prices suspend borrowing and liquidation; repayment and withdrawal do not require a price check. Freshness checks address one oracle failure mode and do not establish price accuracy or market safety.

### Capital allocation

Curator allocations incorporate admission delays, per-market deposit ceilings, and a published withdrawal order. Market registration does not imply endorsement.

Markets independently set a permanent routing share from 0% to 100% of accrued interest. The shared router groups markets by asset pair, draws from lower-utilisation markets first, and recalls capital to higher-utilisation markets first. Utilisation ceilings and distinct recall and redeployment thresholds constrain external deployment.

The design includes capital recall within a withdrawal transaction when available market cash is insufficient. Strategy templates and individual strategies would be subject to governance approval, with audits required for strategy admission. This governance role is separate from the immutable market layer.

## Exposure model

The amount eligible for routing is limited to accrued interest. This accounting boundary does not guarantee principal protection for individual depositors: entrants into an established market acquire exposure to existing external positions, and subsequent losses can reduce their principal.

The design therefore specifies deployment caps and disclosure of current external exposure before deposit. A 0% routing configuration excludes external strategy exposure; other lending-market risks remain.

## Development status

Zatvoon is at the design stage. No system is deployed, and the project is not launched, funded, or audited. This repository publishes research documentation; it contains no protocol implementation or deployment instructions.

The note presents design objectives and proposed mechanisms. Their correctness, economic performance, and behaviour under stress remain to be established through implementation and validation.

## Research discussion

We welcome technical review of the design, particularly:

- Accounting invariants and the treatment of depositor exposure.
- Oracle freshness and failure behaviour.
- Liquidity availability and the feasibility of transaction-level recall.
- Strategy collateral, loss allocation, and measured returns.
- Router stability and net returns after execution costs.

Submit findings through [GitHub Issues](https://github.com/zatvoon-protocol/zatvoon-s-first-narrative/issues), referencing the relevant passage and including assumptions, evidence, or a reproducible counterexample.

## License

See [LICENSE](./LICENSE) for the repository's GNU General Public License v3.0.
