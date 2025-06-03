<p align="center">
  <img src="https://i.ibb.co/RpK337XQ/Logo.png" alt="7Lenders Logo" width="100%"/>
</p>


Integrating a BTC price oracle into 7lenders Soroban smart contract is essential for accurate Loan-to-Value (LTV) monitoring and risk management. Below is a comprehensive documentation on setting up and integrate an oracle for BTC price feeds within the Soroban ecosystem.

---

## 🔗 Oracle Integration Options for Soroban

### 1. **DIA Oracles on Soroban Testnet**

DIA has deployed a price oracle on Stellar's Soroban testnet, providing developers with access to real-world data feeds. ([diadata.org][1])

**Steps to Integrate:**

1. **Access DIA's Oracle Contract:**

   * Obtain the contract ID for DIA's BTC/USD price feed deployed on the Soroban testnet.([diadata.org][2])

2. **Invoke the Oracle Contract:**

   * Use Soroban's cross-contract invocation to call the `get_price` function from the lending contract.

3. **Implement LTV Monitoring:**

   * Utilize the fetched BTC price to calculate the current LTV ratio and trigger necessary actions (e.g., notifications, collateral top-ups).

### 2. **Custom Oracle Using `soroban-kit::oracle` Module**

The `soroban-kit::oracle` module provides a flexible framework for building custom oracles using a pub/sub pattern for asynchronous communication. ([dev.to][3])

**Steps to Implement:**

1. **Set Up Oracle Broker and Subscriber:**

   * Define the smart contract as an oracle subscriber using the `oracle_subscriber` macro.
   * Implement an off-chain oracle broker that fetches BTC prices from external APIs and publishes them to the blockchain.([dev.to][3])

2. **Establish Communication:**

   * Use the pub/sub pattern to allow the lending contract to receive real-time BTC price updates from the oracle broker.

3. **Handle Price Updates:**

   * Upon receiving new price data, update the LTV calculations and enforce any necessary contract logic (e.g., margin calls).

### 3. **Chainlink Integration via Relink Protocol**

The Relink Protocol offers a trustless method to integrate Chainlink's decentralized oracle network into Soroban smart contracts. ([github.com][4])

**Steps to Integrate:**

1. **Deploy Relink Contracts:**

   * Use the Relink Protocol's Rust-based contracts to bridge Chainlink's BTC price feeds into the Soroban environment.

2. **Fetch BTC Price Data:**

   * Invoke the appropriate functions within the Relink contracts to retrieve the latest BTC/USD price.

3. **Incorporate into Lending Logic:**

   * Use the obtained price data to monitor LTV ratios and manage loan conditions accordingly.

---

## 🧪 Testing Scenarios

To ensure the reliability of the oracle integration, we consider the following test cases:

* **Price Feed Accuracy:**

  * Verify that the oracle provides accurate and timely BTC price data.([en.wikipedia.org][5])

* **LTV Threshold Breach:**

  * Simulate a drop in BTC price to test if the contract correctly identifies LTV breaches and triggers notifications.

* **Collateral Top-Up:**

  * Test the contract's response to collateral top-ups after an LTV breach, ensuring it resets the grace period and updates the LTV ratio.

* **Oracle Downtime:**

  * Assess the contract's behavior when the oracle fails to provide data, ensuring it handles such scenarios gracefully.

---

## 🚀 Deployment Instructions

1. **Set Up Development Environment:**

   * Install Rust and the Soroban CLI.
   * Initialize a new Soroban project using `soroban new`.([medium.com][6])

2. **Implement Oracle Integration:**

   * Incorporate the chosen oracle method into the smart contract code.([medium.com][6])

3. **Build and Deploy Contract:**

   * Compile the contract with `soroban build`.
   * Deploy to the Soroban testnet using `soroban deploy`.

4. **Test Contract Functionality:**

   * Invoke contract functions to simulate loan creation, collateral locking, and LTV monitoring.

5. **Monitor and Iterate:**

   * Continuously monitor contract performance and make necessary adjustments based on test results.

---
