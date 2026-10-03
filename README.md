<h1 align="center">pQT Protocol ($pQT)</h1>

<p align="center">
  <b>The First Fully Immutable, Automated Quantitative Tightening Protocol on Solana</b>
</p>

<p align="center">
  <a href="https://solana.com"><img src="https://img.shields.io/badge/Solana-Token--2022-3772FF?style=for-the-badge&logo=solana" alt="Solana Token-2022"></a>
  <a href="https://www.anchor-lang.com"><img src="https://img.shields.io/badge/Anchor-v0.30+-C9A227?style=for-the-badge" alt="Anchor"></a>
  <a href="#-tokenomics--fee-distribution"><img src="https://img.shields.io/badge/Transfer_Fee-0.05%25_Fixed-FF7A59?style=for-the-badge" alt="Transfer Fee"></a>
  <a href="#-security--governance-matrix"><img src="https://img.shields.io/badge/Governance-100%25_Immutable-82E6AB?style=for-the-badge" alt="Immutable"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="License"></a>
</p>

---

## 🏛️ Macroeconomic Narrative: Algorithmic Quantitative Tightening

In traditional fiat macroeconomics, Central Banks utilize **Quantitative Easing (QE)** to flood markets with liquidity, debasing currency purchasing power and driving long-term monetary inflation. Conversely, **Quantitative Tightening (QT)** is the deliberate contraction of a central bank's balance sheet—reducing overall money supply to restore monetary scarcity and stability.

**pQT Protocol ($pQT)** translates this Central Bank QT framework into a **fully automated, decentralized, and immutable smart contract engine** on the Solana blockchain. 

Unlike conventional tokens that rely on manual buybacks or centralized team decisions, $pQT enforces relentless monetary contraction at the protocol layer. Every transfer triggers a micro-fee that is immediately channeled into a permissionless burn engine. $pQT is monetary policy written in code—predictable, mathematical, and unalterable.

---

## ⚡ Core Protocol Value Propositions

1. **100% Immutable & Trustless:** All administrative privileges (`Mint Authority`, `Freeze Authority`, and `Fee Config Authority`) are permanently revoked. No backdoor mints, no blacklisting, and no arbitrary tax hikes.
2. **Zero-Key Fee Management:** Fee collection rights (`withdraw_withheld_authority`) are assigned to a **Program Derived Address (PDA)**. The team holds no keys to collect or access accumulated fees.
3. **High-Frequency DEX Compatibility:** The ultra-low **0.05% (5 bps)** transfer fee is engineered specifically for high-frequency DEX trading, liquidity provision, and arbitrage without bottlenecking volume or Compute Units (CU).
4. **Self-Sustaining MEV Game Theory:** A **15% public bounty** is instantly awarded to whoever triggers the fee harvest. This creates an autonomous, bot-driven ecosystem where external actors pay gas to contract the supply.
5. **Native Token-2022 Architecture:** Leverages Solana's native Token-2022 extensions (`Transfer Fee`), eliminating heavy custom Transfer Hooks for maximum composability across protocols like Jupiter, Raydium, and Orca.

---

## 💎 Comprehensive Token Attribute Breakdown

| Token Element | On-Chain Parameter / Mechanism | Economic & Security Impact |
| :--- | :--- | :--- |
| **Token Standard** | Solana `Token-2022` | Native protocol-level extension. Fast execution, low transaction footprint, seamless DEX integration. |
| **Fixed Transfer Fee** | `0.05%` (5 basis points) | Frictionless for trading while generating massive cumulative deflation on high volume. |
| **Fee Authority** | PDA (`seeds = [b"vault-authority"]`) | No private key exists. Fee withdrawal is strictly bound to public smart contract logic. |
| **Supply Contraction (Burn)** | `80%` of collected fees | Directly destroys $pQT from circulating supply via CPI `burn` instructions. |
| **Permissionless Bounty** | `15%` of collected fees | Instant payout to the caller (user/bot) triggering the harvest instruction. |
| **Dev Treasury** | `5%` of collected fees | Dedicated reserve for ongoing infrastructure, RPC nodes, and ecosystem expansion. |
| **Mint Authority** | **Revoked (Permanently Disabled)** | Hard supply cap guaranteed. Zero additional tokens can ever be created. |
| **Freeze Authority** | **Revoked (Permanently Disabled)** | Complete censorship resistance. User accounts can never be frozen. |
| **Fee Config Authority** | **Revoked (Permanently Disabled)** | Fee rate is locked at 0.05% forever. Cannot be raised or modified. |

---

## 🔄 Protocol Architecture & Harvest Lifecycle

### 1. Passive On-Chain Accrual
Whenever $pQT is transferred across wallets, DEX pools, or programs, the Token-2022 program automatically withholds **0.05%** directly inside the recipient's token account (`withheld_amount`).

### 2. Autonomous Triggering (`harvest_and_distribute`)
The protocol exposes a single public, permissionless instruction: `harvest_and_distribute`. Anyone—ranging from retail users to automated MEV arbitrage bots—can execute this instruction.

### 3. Atomic CPI Execution Sequence
Within a **single atomic transaction**, the Anchor program executes the following steps:

1. **Harvest to Mint:** Aggregates withheld tokens from target user accounts to the Mint level (`harvest_withheld_tokens_to_mint`).
2. **Withdraw to PDA Vault:** Transfers accumulated fees from the Mint into the program's Vault token account (`withdraw_withheld_tokens_from_mint`), authorized via PDA seeds (`[b"vault-authority"]`).
3. **🔥 CPI Burn (80%):** Permanently burns 80% of the vault's collected balance, contracting total supply.
4. **⚡ CPI Bounty Payout (15%):** Instantly transfers 15% of the collected balance to the transaction caller's wallet.
5. **🛡️ CPI Treasury Top-up (5%):** Sends 5% of the collected balance to the developer treasury.

---

## 📊 Sequence Diagram

```mermaid
sequenceDiagram
    autonumber
    actor Caller as Public Caller / MEV Bot
    participant Program as pQT Smart Contract (Anchor PDA)
    participant Token2022 as Solana Token-2022 Program
    participant UserAccounts as User Token Accounts
    participant Vault as Program Vault (PDA ATA)
    participant Treasury as Dev Treasury Account

    Note over UserAccounts, Token2022: 0.05% withheld automatically on every transfer
    Caller->>Program: Call harvest_and_distribute(accounts)

    rect rgb(20, 25, 40)
        Note over Program, Token2022: Phase 1: Aggregate & Withdraw (Signed by PDA)
        Program->>Token2022: harvest_withheld_tokens_to_mint(accounts)
        Token2022-->>Program: Fees aggregated at Mint
        Program->>Token2022: withdraw_withheld_tokens_from_mint() [Signed by PDA]
        Token2022->>Vault: Move total fees into Vault
    end

    rect rgb(40, 20, 20)
        Note over Program, Token2022: Phase 2: Supply Contraction (80%)
        Program->>Token2022: CPI burn(80% from Vault)
        Token2022-->>Vault: Tokens permanently destroyed
    end

    rect rgb(20, 40, 25)
        Note over Program, Caller: Phase 3: Instant Bounty Payout (15%)
        Program->>Token2022: CPI transfer_checked(15% Vault -> Caller)
        Token2022-->>Caller: Bounty rewarded directly
    end

    rect rgb(25, 20, 45)
        Note over Program, Treasury: Phase 4: Protocol Reserve (5%)
        Program->>Token2022: CPI transfer_checked(5% Vault -> Treasury)
        Token2022-->>Treasury: Dev reserve updated
    end
