# BitForge Collateral Protocol

## Overview

**BitForge Collateral Protocol** is a **Bitcoin-collateralized stablecoin system** built on the **Stacks blockchain**, designed to mint and manage **BFUSD**, a USD-pegged stablecoin.
By locking Bitcoin as collateral, users can **create decentralized, trustless vaults** to mint BFUSD while preserving custody of their BTC.

The protocol ensures solvency through **over-collateralization**, **multi-oracle price feeds**, and an **automated liquidation engine**. Governance parameters allow dynamic updates to collateralization ratios, fees, and limits, ensuring adaptability to market conditions.

This protocol bridges **Bitcoin’s store-of-value** with **DeFi’s programmability**, unlocking a new class of Bitcoin-native financial services.

---

## Key Features

* **Bitcoin-Collateralized Vaults**
  Users deposit BTC (via wrapped BTC on Stacks) into permissionless vaults to mint BFUSD.

* **Over-Collateralization Safety**
  Vaults require minimum collateralization ratios, enforced at mint and monitored continuously.

* **Multi-Oracle Price Feeds**
  Aggregated BTC price data ensures reliability against manipulation or failure.

* **Automated Liquidations**
  Under-collateralized vaults can be liquidated by third parties to protect protocol solvency.

* **Governance-Driven Parameters**
  Protocol owner (DAO or governance entity in future iterations) can update critical parameters like collateralization ratio and mint limits.

* **Permissionless & Trustless**
  Anyone can open and manage vaults without centralized approval.

---

## System Architecture

### High-Level Flow

1. **Vault Creation**
   A user deposits BTC collateral into a vault. Each vault is uniquely identified by `(owner, vault-id)`.

2. **Stablecoin Minting**
   Based on collateral value and system-defined collateralization ratio, the user can mint BFUSD.

3. **Redemption**
   Users can burn BFUSD to unlock their collateral, subject to redemption fees.

4. **Liquidation**
   If the collateralization ratio falls below the liquidation threshold, third parties can liquidate the vault. Minted BFUSD is burned, and collateral is released.

5. **Oracle Updates**
   Authorized oracles continuously update BTC/USD price feeds to ensure accurate collateral valuations.

---

## Contract Architecture

### Traits

* **`sip-010-token`**
  Implements SIP-010 interface for fungible tokens (used for BFUSD stablecoin compliance).

### Core Components

* **Error Codes**
  Predefined for vault, oracle, and governance operations.

* **Security Constants**
  Guardrails against invalid BTC price data and timestamp overflows.

* **Protocol Configuration**
  Defines stablecoin metadata, collateralization ratios, liquidation thresholds, and total supply.

* **Vault System**

  * `create-vault`
  * `mint-stablecoin`
  * `redeem-stablecoin`
  * `liquidate-vault`

* **Oracle System**

  * `add-btc-price-oracle`
  * `update-btc-price`
  * `get-latest-btc-price`

* **Governance System**

  * `update-collateralization-ratio`
  * Additional parameters (fees, mint limits) managed via governance.

---

## Data Flow

1. **Price Oracle Updates**

   * Oracles push BTC/USD prices into `last-btc-price`.
   * Protocol functions read this data to compute vault health.

2. **Vault Lifecycle**

   * Creation → Collateral deposit recorded in `vaults` map.
   * Minting → Checks collateral ratio before minting BFUSD.
   * Redemption → Burns BFUSD, releases collateral proportionally.
   * Liquidation → Deletes undercollateralized vaults, burns BFUSD supply.

3. **Supply Tracking**

   * `total-supply` reflects net BFUSD minted across all vaults.
   * Updated on mint, redeem, and liquidation.

---

## Governance Parameters

* **Collateralization Ratio (`collateralization-ratio`)**
  Minimum ratio required for minting (e.g., 150%).

* **Liquidation Threshold (`liquidation-threshold`)**
  Below this ratio, vaults become liquidatable (e.g., 125%).

* **Mint & Redemption Fees (`mint-fee-bps`, `redemption-fee-bps`)**
  Applied during stablecoin issuance and redemption.

* **Maximum Mint Limit (`max-mint-limit`)**
  Caps system-wide exposure.

---

## Security Considerations

* **Oracle Validation**
  Only whitelisted oracles can update BTC prices.
  Data is validated against maximum thresholds to prevent malicious manipulation.

* **Over-Collateralization**
  Ensures that BFUSD remains solvent even in volatile BTC markets.

* **Permissionless Liquidation**
  Any actor can liquidate unhealthy vaults, incentivizing system stability.

* **Governance Control**
  Restricted to protocol owner (DAO migration in roadmap).

---

## Roadmap

* Transition governance to on-chain DAO.
* Support for multiple collateral types (beyond BTC).
* Advanced liquidation mechanisms (partial liquidation, auction model).
* Integration with Stacks DeFi ecosystem (DEXs, lending markets).

---

## License

MIT License. Open for community-driven improvements and audits.
