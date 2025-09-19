
#  STX-BlockBloom

A programmable, STX-integrated **vesting NFT smart contract** with support for staking, leveling, metadata, and secure economic logic.

This Clarity-based smart contract enables minting NFTs with programmable vesting logic, level upgrades, metadata customization, and staking infrastructure — all backed by native STX payments and enhanced safety checks.

---

## 🔧 Features

* ✅ **NFT Minting**: Users can mint non-fungible tokens by paying STX.
* 🕒 **Vesting Mechanism**: Each NFT has a `vesting-period`, and levels up over time.
* 🪙 **STX-Powered**: Transactions like minting and leveling require STX payments.
* 🧠 **Level-Based Utility**: Admin can assign utilities to different token levels.
* 🧾 **Metadata Support**: Tokens can have name, description, and image URI.
* 🔐 **Access Control**: Admin-only methods for pricing, utility, and fund withdrawal.
* 🧰 **Safety Checks**: Defensive coding practices including overflow checks and balance verification.
* 💸 **STX Withdrawal**: Admin can withdraw STX accumulated in the contract.

---

## 📜 Smart Contract Functions

### 📥 Public Functions

#### 1. `mint (vesting-period uint)`

Mint a new NFT with a defined `vesting-period`. Requires STX payment (`mint-price`).

**Validations:**

* `vesting-period > 0`
* Sufficient STX balance
* Token ID uniqueness

#### 2. `update-token-level (token-id uint)`

Allows NFT owner to upgrade token level based on elapsed `block-height` and `vesting-period`. Requires STX payment (`level-up-price`).

**Validations:**

* Token ownership
* Positive `vesting-period`
* Actual level-up available
* Sufficient STX balance

#### 3. `set-level-utility (token-id uint, level uint, utility string)`

Sets a custom utility string for a given token and level.

**Access:** Contract Owner only
**Validations:** Token existence, utility string not empty

#### 4. `set-mint-price (new-price uint)`

Updates the price for minting new NFTs.

**Access:** Contract Owner only
**Validation:** `new-price > 0`

#### 5. `set-level-up-price (new-price uint)`

Updates the price for leveling up tokens.

**Access:** Contract Owner only
**Validation:** `new-price > 0`

#### 6. `withdraw-stx (amount uint)`

Withdraw STX from the contract balance to the contract owner's address.

**Access:** Contract Owner only
**Validations:**

* `amount > 0`
* Contract balance ≥ amount

---

## 🗃️ Data Structures

### 🔢 Variables

| Name             | Type   | Description                    |
| ---------------- | ------ | ------------------------------ |
| `token-id-nonce` | `uint` | Auto-incremented token counter |
| `mint-price`     | `uint` | Cost to mint an NFT            |
| `level-up-price` | `uint` | Cost to level up an NFT        |
| `reward-rate`    | `uint` | Reserved for staking rewards   |

---

### 🗺️ Maps

* `tokens`: Stores core NFT data (owner, vesting details, level)
* `token-levels`: Stores per-level utility string
* `token-metadata`: Stores name, description, image-uri
* `staking-info`: (future use) Tracks staking timestamps

---

### ❗ Errors

| Code | Message                |
| ---- | ---------------------- |
| 100  | Owner only             |
| 101  | Not token owner        |
| 102  | Invalid token          |
| 103  | Insufficient funds     |
| 104  | Invalid parameters     |
| 105  | Zero amount            |
| 106  | Unauthorized           |
| 107  | Already staked         |
| 108  | Not staked             |
| 109  | Staking error          |
| 110  | Fusion failed          |
| 111  | Cannot fuse same token |

---

## ⚙️ Example Flow

1. 🛠️ User calls `mint` with vesting period (e.g. `u1000`) → Pays `mint-price`
2. ⌛ Waits for time to pass (measured in blocks)
3. 🚀 Calls `update-token-level` → Pays `level-up-price` → Level increases
4. 🧑‍💼 Admin sets special utility strings for higher levels
5. 💵 Admin can withdraw accumulated STX

---

## 🧪 Testing Ideas

* Mint NFT with zero and non-zero vesting-period
* Try updating token level before and after vesting
* Set utilities and verify updates
* Adjust prices and validate their enforcement
* Attempt withdrawals under and over balance

---

## 🔐 Admin Controls

These functions require contract owner (initial deployer) permissions:

* `set-mint-price`
* `set-level-up-price`
* `set-level-utility`
* `withdraw-stx`

Ensure to safeguard the deployer's key and use secure deployment practices.

---
s