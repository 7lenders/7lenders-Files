<p align="center">
<img src="Final%20Files/Logo.png" alt="7Lenders Logo" width="full"/>
</p>

## Workflow diagram, followed by test scenarios, and finally the deployment instructions for the Soroban smart contract.

```
+-----------------+                  +-------------------+
|     Lender      |                  |    Borrower       |
+-----------------+                  +-------------------+
         |                                     |
         |     [Off-chain Fiat Transfer]       |
         |------------------------------------>|
         |                                     |
         |         [Lock BTC Collateral]       |
         |<------------------------------------|
         |                                     |
         |         [Loan Terms Created]        |
         |<----------- Soroban Contract -------|
         |                                     |
         |    [Monitor BTC Price via Oracle]   |
         |<----------- Chainlink Oracle -------|
         |                                     |
         |       [LTV Breach Notification]     |
         |<----------- Notifier (App/SMS) -----|
         |                                     |
         |      [Grace Period Top-Up Check]    |
         |<----------- Soroban Contract -------|
         |                                     |
         |      [Loan Repaid in Fiat]          |
         |<------------------------------------|
         |                                     |
         |  [Collateral Returned or Liquidated]|
         |<----------- Soroban Contract -------|
```

| Test Case                                | Expected Outcome                        |
| ---------------------------------------- | --------------------------------------- |
| Lock sufficient BTC collateral           | Loan contract activates                 |
| LTV stays within threshold               | No notifications sent                   |
| BTC price drops, LTV > threshold         | Grace period begins + notification sent |
| Borrower tops up within grace period     | Collateral updated, loan remains active |
| Borrower fails to top up                 | Contract triggers liquidation           |
| Loan is fully repaid in fiat (off-chain) | BTC collateral released to borrower     |
| Loan not repaid within term              | Collateral liquidated to lender         |


## Deployment Instructions

### Prerequisites
Ensure the following are installed on your system:
- **Rust**  
- **Soroban CLI**  
- **Stellar Development Environment**

---

### Step 1: Create a New Soroban Contract Project

```bash
cargo new --lib btc_lending
cd btc_lending
```

---

### Step 2: Add Soroban Dependencies

Edit `Cargo.toml`:

```toml
[dependencies]
soroban-sdk = "20.0.0"

[lib]
crate-type = ["cdylib", "rlib"]
```

---

### Step 3: Implement the Contract Logic

Write your contract logic in `src/lib.rs`.

> Use the Soroban contract code you have previously generated or written.

---

### Step 4: Build the Contract

```bash
soroban contract build
```

---

### Step 5: Deploy to Stellar Testnet

```bash
soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/btc_lending.wasm \
  --network testnet
```

---

### Step 6: Invoke Contract Functions

Replace `<contract-id>` and other placeholders with actual values:

```bash
soroban contract invoke \
  --network testnet \
  --id <contract-id> \
  --fn initialize_loan \
  --arg borrower=<address> \
  --arg principal=<amount>
```
