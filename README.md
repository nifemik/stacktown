
# Stacktown - Name Registry & Marketplace Smart Contract

This Clarity smart contract implements a decentralized domain name registry system on the Stacks blockchain. It allows users to register unique names, extend their validity, and optionally list them on a marketplace (note: marketplace logic is partially defined in the current code version).

---

## 🧠 Features

* ✅ **Name Validation**: Enforces strong constraints on allowed characters, length, and format.
* ✅ **Registration System**: Allows users to register unique names for a specified period (1 to 5 years).
* ✅ **Payment System**: Requires STX payments proportional to the registration/extension period.
* ✅ **Ownership Tracking**: Each name is associated with a single `principal` (user).
* 🚧 **Marketplace Skeleton**: Partial data structures for a buy/sell system are included.

---

## 📐 Rules for Valid Names

* Name must:

  * Be **3 to 64 characters** long.
  * Contain **only lowercase letters**, numbers, and hyphens (`-`).
  * **Not** start or end with a hyphen.

---

## 🗺️ Data Structures

### 🔐 `name-registry` Map

Stores name ownership and metadata:

```clojure
{ name: (string-ascii 255) } => {
  holder: principal,
  validity: uint,
  listing-amount: uint
}
```

### 🏪 `name-marketplace` Map

(Placeholder - not yet fully implemented)

```clojure
{ name: (string-ascii 255) } => {
  buyer: principal,
  purchase-amount: uint
}
```

---

## ⚙️ Variables

* `base-fee` = `u10000000` (0.1 STX/year default)
* `min-validity-period` = `u31536000` (1 year in seconds)
* `max-validity-period` = `u157680000` (5 years)

---

## 🔍 Read-only Functions

* `is-valid-name(name)`
  → Checks all validation rules.

* `get-name-info(name)`
  → Returns metadata about a name if valid and registered.

---

## 🛠️ Public Functions

### `register-name(name, period)`

* Registers a new name if:

  * It is valid.
  * Not already taken.
  * Duration is between 1 and 5 years.
* Transfers required STX fee to contract.
* Stores ownership and expiration time.

### `extend-validity(name, additional-period)`

* Extends registration period.
* Only the current name holder can extend.
* Transfers proportional STX fee.
* Updates expiration time.

---

## ❗ Errors

| Code   | Description                   |
| ------ | ----------------------------- |
| `u100` | Unauthorized                  |
| `u101` | Name already taken            |
| `u102` | Name not found                |
| `u103` | Invalid name format or period |
| `u104` | Transaction failed            |
| `u105` | Payment required              |

---
