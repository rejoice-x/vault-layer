# 🏗️ VaultLayer Protocol

**Unlock Bitcoin’s dormant value through verifiable collateral vaults.**
Borrow against your BTC holdings without surrendering ownership or missing upside gains — all secured by Stacks' Bitcoin settlement finality.

---

## 📜 Overview

**VaultLayer** is a decentralized protocol enabling Bitcoin holders to mint synthetic liquidity by locking BTC as collateral. The system introduces **overcollateralized vaults** that maintain exposure to Bitcoin appreciation while ensuring solvency and trust through on-chain verification and Bitcoin anchoring.

Built on **Stacks**, VaultLayer leverages Bitcoin’s immutability and Stacks’ smart contract capabilities to enable trust-minimized borrowing and liquidation flows — entirely without intermediaries.

---

## ⚙️ System Architecture

```
+----------------------------+
|        Bitcoin Layer       |
|   (Base Settlement Layer)  |
+-------------+--------------+
              |
         Settlement Finality
              |
+-------------v--------------+
|          Stacks Layer      |
|   (Clarity Smart Contract) |
|                            |
|   +---------------------+  |
|   |  Vault Contract     |  |
|   |---------------------|  |
|   |  Collateral Logic   |  |
|   |  Loan Management    |  |
|   |  Liquidation Rules  |  |
|   |  Oracle Integration |  |
|   +---------------------+  |
|                            |
+-----------------------------+
              |
       Synthetic Liquidity
              |
+-------------v--------------+
|         User Interface     |
| (Vault Manager / dApp)     |
+-----------------------------+
```

**Core Layers**

* **Bitcoin Layer:** Provides the ultimate settlement and security anchor.
* **Stacks Layer:** Hosts smart contracts governing collateralization, borrowing, and liquidation.
* **Application Layer:** Enables users to interact with vaults, monitor collateral ratios, and manage loans.

---

## 🧩 Contract Architecture

| Component               | Description                                                                                                        |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **VaultLayer.clar**     | Core protocol contract managing vault lifecycle, collateral deposits, loan requests, repayments, and liquidations. |
| **Collateral Registry** | Maintains supported assets and price feeds via `collateral-prices` map.                                            |
| **Loan Registry**       | Tracks each vault’s parameters, including borrower, collateral amount, loan value, and status.                     |
| **User Loan Index**     | Associates users with active loan positions for efficient retrieval.                                               |
| **Platform Config**     | Holds adjustable system parameters like collateral ratio, liquidation threshold, and fees.                         |

---

## 🏦 Core Functions

### 🔹 Administrative

* `initialize-platform` – Bootstraps protocol configuration.
* `update-collateral-ratio` – Adjusts required collateralization.
* `update-liquidation-threshold` – Updates safety thresholds.
* `update-price-feed` – Pushes new oracle prices (restricted to contract owner).

### 🔹 Core Operations

* `deposit-collateral(amount)` – Locks collateral into the protocol.
* `request-loan(collateral, loan-amount)` – Opens a new overcollateralized position.
* `repay-loan(loan-id, amount)` – Settles a position and releases collateral.

### 🔹 Read-Only Accessors

* `get-loan-details(loan-id)` – Returns details of a specific loan.
* `get-user-loans(user)` – Fetches active loans for a user.
* `get-platform-stats()` – Provides global metrics like total BTC locked.
* `get-valid-assets()` – Lists supported collateral assets.

---

## 🔄 Data Flow

1. **Collateral Deposit**

   * User deposits BTC or supported asset → increases `total-btc-locked`.
2. **Loan Request**

   * Protocol checks price feed and collateral ratio → issues loan if sufficient.
3. **Repayment**

   * User repays principal + accrued interest → vault marked as *repaid*, collateral released.
4. **Liquidation**

   * When collateral ratio ≤ threshold → position auto-liquidated, recorded as *liquidated*.

---

## 🧮 Key Parameters

| Variable                   | Description                         | Default |
| -------------------------- | ----------------------------------- | ------- |
| `minimum-collateral-ratio` | Required overcollateralization      | `150%`  |
| `liquidation-threshold`    | Ratio at which liquidation triggers | `120%`  |
| `platform-fee-rate`        | Protocol fee on operations          | `1%`    |

---

## ⚖️ Error Codes

| Code   | Description             |
| ------ | ----------------------- |
| `u100` | Not authorized          |
| `u101` | Insufficient collateral |
| `u102` | Below minimum           |
| `u103` | Invalid amount          |
| `u107` | Loan not found          |
| `u108` | Loan not active         |
| `u111` | Invalid asset           |

---

## 🧠 Design Principles

* **Bitcoin-native Collateralization:** BTC remains the settlement anchor.
* **Deterministic Interest Accrual:** Interest computed per block with consistent time accounting.
* **Verifiable Risk Controls:** On-chain enforcement of collateral ratios and liquidation thresholds.
* **Permissioned Admin Controls:** Limited to configuration and oracle management only.

---

## 🧰 Development

### Prerequisites

* [Stacks CLI](https://docs.stacks.co/docs/build/smart-contracts/clarity-cli)
* Clarity compiler v2.x
* Local Stacks node or Clarinet environment

### Setup

```bash
git clone https://github.com/<org>/VaultLayer.git
cd VaultLayer
clarinet check
clarinet test
```

### Deployment

```bash
clarinet deploy --network=testnet
```

---

## 📈 Future Extensions

* Integration with decentralized oracles (e.g., ALEX or Chainlink BTC feed).
* Multi-collateral vaults (support for wrapped BTC derivatives).
* Dynamic liquidation mechanisms with auction-based recovery.
* Frontend dashboard for vault management and analytics.

---

## 🛡️ Security & Auditing

All state transitions are fully deterministic under Clarity’s static analysis guarantees.
Formal verification and independent audit phases are recommended before mainnet deployment.

---

## 📄 License

**MIT License** – open for community contribution and extension under standard open-source terms.
