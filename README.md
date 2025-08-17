## STX-DonorBridge
This Clarity smart contract enables a decentralized Donor Platform. It allows users to contribute STX tokens, administrators to manage recipients and settings, and recipients to withdraw allocated funds on a timed basis. The contract also supports emergency controls and refund mechanisms, making it both transparent and secure for all parties involved.

---

## 📋 Features

* ✅ **Public Donations:** Anyone can donate within set minimum and maximum limits.
* 🧾 **Recipient Management:** Admin can add, update, or remove relief recipients.
* 💸 **Withdrawals:** Recipients can withdraw allocated funds every 24 hours.
* 🔁 **Refunds:** Donors can request a refund within 24 hours.
* 🛑 **Emergency Shutdown:** Admin can pause contract operations in emergencies.
* 🔐 **Admin Controls:** Admin can update donation limits, pause/unpause the contract, and change admin rights.
* 📊 **Auditing & Logging:** Built-in event logging and audit support.

---

## ⚙️ Contract Details

### Constants

| Constant Name             | Value      | Description                                   |
| ------------------------- | ---------- | --------------------------------------------- |
| `withdrawal-cooldown`     | `86400`    | 24 hours in seconds (for withdrawals/refunds) |
| `ERR_UNAUTHORIZED`        | `(err u1)` | Caller is not the admin                       |
| `ERR_INVALID_AMOUNT`      | `(err u2)` | Amount must be positive                       |
| `ERR_UNCHANGED_STATE`     | `(err u9)` | New value must differ from the old            |
| `ERR_RECIPIENT_EXISTS`    | `(err u5)` | Recipient already exists                      |
| `ERR_RECIPIENT_NOT_FOUND` | `(err u4)` | Recipient does not exist                      |
| `ERR_NOT_CONFIRMED`       | `(err u8)` | Confirmation required                         |
| `ERR_INSUFFICIENT_FUNDS`  | `(err u3)` | Not enough funds for transaction              |

---

## 🧑‍💻 Usage

### 🪙 Donating

```clojure
(donate u500)
```

* Must fall within `min-donation` and `max-donation` limits.
* Fails if contract is paused.

### 👤 Adding a Recipient (Admin only)

```clojure
(add-recipient 'ST3...XYZ u1000)
```

* Adds recipient with a fund allocation.
* Fails if recipient already exists or allocation is invalid.

### 🏦 Withdrawing Funds (Recipient only)

```clojure
(withdraw u500)
```

* Can withdraw once per 24 hours.
* Amount must be ≤ allocated amount.

### 💰 Requesting a Refund

```clojure
(request-refund u200)
```

* Must be within 24 hours of the donation.

### 🛑 Emergency Shutdown

```clojure
(emergency-shutdown)
```

* Pauses all non-admin operations.

---

## 🔐 Admin Functions

### Set New Admin

```clojure
(set-admin 'ST3...ABC)
```

### Update Donation Limits

```clojure
(set-donation-limits u10 u10000)
```

### Pause/Unpause Contract

```clojure
(set-paused true) ;; or false
```

### Update Recipient Allocation

```clojure
(update-recipient-allocation 'ST3...XYZ u2000)
```

### Remove Recipient

```clojure
(confirm-remove-recipient 'ST3...XYZ true)
```

---

## 🔎 Read-Only Functions

* `get-donation` — View donations from a specific user.
* `get-total-funds` — View total funds in the contract.
* `get-recipient-allocation` — View allocation for a recipient.
* `get-user-history` — View both donation and withdrawal timestamps.
* `is-paused` — Check if the contract is paused.
* `get-admin` — Returns current admin.

---

## 📦 Data Structures

### Data Variables

| Variable       | Type        | Description                     |
| -------------- | ----------- | ------------------------------- |
| `total-funds`  | `uint`      | Total STX donated               |
| `admin`        | `principal` | Admin of the contract           |
| `min-donation` | `uint`      | Minimum accepted donation       |
| `max-donation` | `uint`      | Maximum accepted donation       |
| `paused`       | `bool`      | Pauses most functions when true |

### Maps

| Map Name          | Key         | Value  | Description                        |
| ----------------- | ----------- | ------ | ---------------------------------- |
| `donations`       | `principal` | `uint` | Total amount donated by user       |
| `recipients`      | `principal` | `uint` | Amount allocated to each recipient |
| `last-withdrawal` | `principal` | `uint` | Timestamp of last withdrawal       |

---

## 📘 Example Flow

1. **Admin deploys the contract** and sets donation limits.
2. **Users donate** STX tokens.
3. **Admin adds recipients** and assigns allocations.
4. **Recipients withdraw** funds once every 24 hours.
5. **Donors may request refunds** within the 24-hour window.
6. **In emergencies**, admin pauses the contract.

---
