---
name: canton-implementer
description: Guide for implementing applications on the Canton Network using Daml, Splice packages, and Global Synchronizer APIs. Use this skill when a user asks for help building, designing, deploying, or integrating with the Canton Network, Global Synchronizer, Canton Coin, or explicitly mentions Daml smart contracts, Splice packages (splice-amulet, splice-wallet, splice-dso-governance, splice-amulet-name-service), Ledger API, Scan API, Validator API, Super Validators, or Canton Name Service (CNS).
---

# Canton Implementer

Enable building applications on the Canton Network — a privacy-enabled, interoperable blockchain network for regulated real-world assets, powered by the Global Synchronizer (Splice).

## Architecture Overview

### Canton Network

A "network of networks" of sovereign blockchains that can interoperate atomically via the Global Synchronizer without sacrificing privacy or control.

### Global Synchronizer

- Decentralized interoperability/synchronization infrastructure
- Uses **2/3 BFT** (Byzantine Fault Tolerant) consensus for message ordering/confirmation
- Open-source implementation: [Splice](https://github.com/hyperledger-labs/splice)
- Governed by the **Global Synchronizer Foundation (GSF)** in partnership with the Linux Foundation

### Key Participants

| Role | Responsibilities |
|------|-----------------|
| **Super Validators (SV)** | Run core infrastructure, sequence transactions, validate Canton Coin transfers, participate in governance via on-chain DSO party |
| **Validators** | Validate transactions, record activity, facilitate connectivity for users/apps, coordinate upgrades |
| **DSO Party** | Decentralized Daml party hosted across all SV nodes; requires 2/3 confirmation threshold for governance actions |

### Canton Coin (CC) — Tokenomics

Utility token with **Burn-Mint Equilibrium**:
- **Burn**: Users pay fees (USD-denominated, paid by burning CC) for transaction sequencing
- **Mint**: Validators/SVs/app providers earn minting rights for infrastructure, liveness, and usage
- **Equilibrium**: Burning ≈ minting over time, stabilizing conversion rate

## Application Development

### Technology Stack

- **Smart Contracts**: Written in **Daml** (v3.4+). Learn at https://docs.digitalasset.com/build/3.4/
- **On-chain state & workflows**: Implemented via Splice Daml packages
- **APIs**: HTTP and gRPC APIs from Global Synchronizer and Validator Nodes

### Splice Daml Packages

| Package | Purpose |
|---------|---------|
| `splice-amulet` | Canton Coin (CC) implementation. Key choice: `AmuletRules_Transfer` |
| `splice-amulet-name-service` | Canton Name Service (CNS) — decentralized party directory |
| `splice-wallet` | Wallet workflows and automation |
| `splice-dso-governance` | Decentralized governance (DSO party management) |

Develop against **Daml Interfaces** (stable public APIs) rather than Templates/Choices directly, to decouple from Splice upgrades. See the [Canton Network Token Standard](https://docs.sync.global/app_dev/daml_api/index.html#app-dev-token-standard-overview).

### API Integration

| API | Use For | Docs |
|-----|---------|------|
| **Ledger API** (gRPC/JSON) | Submit transactions, read ledger view for hosted parties | [Ledger API guide](https://docs.sync.global/app_dev/ledger_api/index.html) |
| **Scan API** | Read global ledger/infrastructure view (DSO party data, no private data) | [Scan API guide](https://docs.sync.global/app_dev/scan_api/index.html) |
| **Validator API** | Higher-level functionality from Splice Validator App | [Validator API guide](https://docs.sync.global/app_dev/validator_api/index.html) |

### Workflow: Building a Canton Network App

1. **Design Daml models** — Define templates, choices, and interfaces for your business logic
2. **Integrate Splice packages** — Use `splice-amulet` for payments, `splice-wallet` for wallet flows, `splice-amulet-name-service` for identity
3. **Connect via APIs** — Use Ledger API for party-specific reads/writes, Scan API for global state, Validator API for high-level operations
4. **Deploy** — Run alongside a Validator Node; the Splice Validator App provides the runtime
5. **Get featured** — Submit your app at https://sync.global/featured-app-request/

### Community Resources

- **Daml docs**: https://docs.digitalasset.com/build/3.4/
- **Splice docs**: https://docs.sync.global/
- **Splice repo**: https://github.com/hyperledger-labs/splice
- **Featured apps**: https://sync.global/featured-apps/
- **Tokenomics mailing list**: https://lists.sync.global/g/tokenomics/topics
- **Slack channel**: `#gsf-global-synchronizer-appdev` (request access via operations@sync.global)
- **White papers**: [Canton Network](https://www.digitalasset.com/hubfs/Canton/Canton%20Network%20-%20White%20Paper.pdf), [Canton Coin](https://www.digitalasset.com/hubfs/Canton%20Network%20Files/Documents%20(whitepapers%2c%20etc...)/Canton%20Coin_%20A%20Canton-Network-native%20payment%20application.pdf)

## References

- **API details & Daml models**: See [references/api_reference.md](references/api_reference.md) for detailed API patterns, Splice Daml model structures, and integration examples
