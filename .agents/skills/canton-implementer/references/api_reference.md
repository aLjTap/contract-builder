# Canton Network API & Daml Models Reference

## Table of Contents

- [Ledger API](#ledger-api)
- [Scan API](#scan-api)
- [Validator API](#validator-api)
- [Splice Daml APIs](#splice-daml-apis)
- [Splice Daml Models](#splice-daml-models)
- [Canton Network Token Standard](#canton-network-token-standard)
- [Key Daml Concepts](#key-daml-concepts)

## Ledger API

The primary API for interacting with the Canton ledger. Available via **gRPC** and **JSON API**.

### When to Use

- Submit transactions on behalf of parties hosted on a Validator Node
- Read the ledger view as seen by those hosted parties
- Stream ledger events for real-time updates

### Access

Provided by the Canton Participant node running within a Validator Node.

### Key Endpoints

- **Command Submission**: Submit Daml commands (create, exercise choices)
- **Transaction Stream**: Subscribe to transaction events for specific parties
- **Active Contracts**: Query currently active contracts for parties
- **Party Management**: Allocate and manage parties

### Documentation

Full reference: https://docs.sync.global/app_dev/ledger_api/index.html

---

## Scan API

Provides a **global view** of ledger state and infrastructure visible to all Super Validator nodes.

### When to Use

- Access publicly visible ledger data (DSO party state)
- Query network infrastructure status
- Read governance decisions and parameters

### Important Note

The Scan API shows data visible to the **DSO party only**. It does **not** include private data belonging to parties hosted on individual Validator Nodes. Use the Ledger API for party-private data.

### Documentation

Full reference: https://docs.sync.global/app_dev/scan_api/index.html

---

## Validator API

Higher-level HTTP API provided by the **Splice Validator App** that runs alongside the Canton Participant node.

### When to Use

- Access wallet functionality
- Manage validator operations
- Use simplified high-level abstractions over the Ledger API

### Documentation

Full reference: https://docs.sync.global/app_dev/validator_api/index.html

---

## Splice Daml APIs

Splice implements decentralized applications whose on-ledger state is written in Daml. The public APIs are defined as **Daml Interfaces**.

### Why Use Interfaces

- **Stability**: Interfaces are the stable public API layer
- **Decoupling**: Developing against Interfaces means your app won't break when Splice upgrades its Templates
- **Interoperability**: Standards like the Canton Network Token Standard use Interfaces

### Documentation

Full reference: https://docs.sync.global/app_dev/daml_api/index.html

---

## Splice Daml Models

The internal implementation of the Splice decentralized applications.

### Key Packages

| Package | Description | Docs |
|---------|-------------|------|
| `splice-amulet` | Canton Coin implementation, transfer logic | [API docs](https://docs.sync.global/app_dev/api/splice-amulet/) |
| `splice-amulet-name-service` | Canton Name Service (CNS) for party discovery | [API docs](https://docs.sync.global/app_dev/api/splice-amulet-name-service/) |
| `splice-wallet` | Wallet workflows and user automation | [API docs](https://docs.sync.global/app_dev/api/splice-wallet/) |
| `splice-dso-governance` | DSO governance: voting, config changes | [API docs](https://docs.sync.global/app_dev/api/splice-dso-governance/) |

### Study Recommendations

Templates and Choices are internal implementation details, but studying them is highly recommended to understand how Canton Coin transfers and governance work under the hood.

### Documentation

Full reference: https://docs.sync.global/app_dev/daml_models/index.html

---

## Canton Network Token Standard

The public API for working with tokens, including Canton Coin.

- Built on top of `AmuletRules_Transfer` choice (for backwards compatibility)
- Provides a stable Interface-based API for token operations
- Recommended way to integrate token functionality into apps

### Documentation

Full reference: https://docs.sync.global/app_dev/daml_api/index.html#app-dev-token-standard-overview

---

## Key Daml Concepts

When building Canton Network applications, understand these Daml fundamentals:

| Concept | Description | Reference |
|---------|-------------|-----------|
| **Templates** | Define contract types with data fields, signatories, observers | https://docs.digitalasset.com/build/3.4/reference/daml/templates.html |
| **Choices** | Actions that can be exercised on contracts, defining state transitions | https://docs.digitalasset.com/build/3.4/reference/daml/choices.html |
| **Interfaces** | Stable public APIs that abstract over Template implementations | https://docs.digitalasset.com/build/3.4/reference/daml/interfaces.html |
| **Parties** | Identities on the ledger that can own contracts and exercise rights | https://docs.digitalasset.com/build/3.4/ |
| **Signatories** | Parties that must authorize contract creation; guarantee data integrity | https://docs.digitalasset.com/build/3.4/ |
