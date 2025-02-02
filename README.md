# 🌉 hetu-bridges

A collection of bridges for cross-chain channels of different types of messages.

Hetu Bridge is a decentralized cross-chain bridge designed to enable seamless asset transfers between Ethereum and other leading blockchain ecosystems. Leveraging innovative cross-chain technology and Causality-Based Hyperscale Finality, Hetu Cross-Chain Bridge offers an efficient, secure, and scalable solution for cross-chain interoperability.
## 🚀 Core Features

- 🌐 Cross-Chain Asset Transfers: Facilitates token transfers between Ethereum and other popular blockchains such as Cosmos, Polkadot, and Solana.

- 🔗 ERC-7683 Compatibility: Supports the ERC-7683 standard, providing enhanced flexibility and scalability for cross-chain transactions.

- 💸 Efficiency and Low Cost: Optimized protocol design reduces transaction costs and processing times.

- 🔒 Decentralization and Security: Maintained by a network of validators, ensuring decentralization and robust security.

- 🤖 AI-Powered Use Cases: Provides infrastructure support for decentralized AI (DeAI) and decentralized science (DeSci) applications.

## 🛠️ Cross-Chain Technology Overview

Hetu Bridge integrates Causality-Based Hyperscale Finality and ERC-7683 compatibility to deliver a robust cross-chain solution. Below is an overview of the core technical components and processes:
1. 🌐 Cross-Chain Communication Protocol

    User-Initiated Transactions: Users initiate cross-chain transactions on the source chain (e.g., Ethereum).

    Event Monitoring and Validation: Relayers monitor events on the source chain and relay transaction data to the target chain.

    Merkle Proof Verification: The target chain verifies transaction validity using Merkle Proofs.

    Asset Locking and Minting: Assets are locked on the source chain, and corresponding assets are minted on the target chain.

2. ⚙️ Causality-Based Hyperscale Finality

    Causal Ordering: Ensures logical consistency of cross-chain transactions using causality-based technology.

    Hyperscale Finality: Supports high-throughput transactions with a theoretical TPS exceeding 160,000.

3. 🔗 ERC-7683 Compatibility

    Flexible Transaction Interface: Enables seamless integration with multiple token standards and cross-chain protocols.

    Multi-Chain Interoperability: Facilitates interoperability with other blockchains through ERC-7683.

4. 🔒 Trusted Execution Environment (TEE)

    Verifiable Logical Clocks: Utilizes TEE to implement verifiable logical clocks, ensuring transparent and secure timestamping.

5. ⏱️ Time Stamp:

    **Smart Contract Layer (Ethereum Side)**

    In Ethereum smart contracts, each block has a built-in timestamp `block.timestamp`, which can be directly referenced by the `Gravity.sol` contract to record the occurrence time of events.

    ```solidity
    event TransactionExecuted(address indexed sender, uint256 amount, uint256 timestamp);

    function executeTransaction(address sender, uint256 amount) external {
           emit TransactionExecuted(sender, amount, block.timestamp);
    }
    
    **Effect:**

- Ensure to record the time of cross chain transactions on the Ethereum chain.

- Convenient to use this timestamp during Cosmos chain replay verification.

    **hetu-bridge**

    In Hetu bridge, when modules handle cross chain messages, they can obtain timestamps through the transaction context ctx.

```go
// Import the necessary Cosmos SDK libraries
import (
    sdk "github.com/cosmos/cosmos-sdk/types"
)

func handleMessage(ctx sdk.Context, msg types.MsgSendToEthereum) error {
    timestamp := ctx.BlockTime()
    fmt.Printf("Received message at timestamp: %s\n", timestamp)
    // Process message logic
    return nil
}

**Effect:**

- Record the processing time of each cross chain request for logging and debugging purposes.

- During the verification process, the timestamp is compared with the Ethereum side to ensure the validity of the data synchronization sequence.

**Summary**

- Ethereum uses block.timestamp, Cosmos uses ctx BlockTime() uniformly records time.

- Ensure that the relay node records the timestamp in the transaction data for replay verification on the target chain.

- Avoid relying directly on timestamps for transaction sorting, and can be verified with block height.

## 🛠️ How to Use
Step-by-Step Guide:

    🔗 Connect Wallet:

        Connect your Ethereum or other compatible blockchain wallet.

    🪙 Select Token:

        Choose the token you wish to transfer and the target chain.

    🚀 Initiate Transaction:

        Enter the recipient address and amount, then initiate the cross-chain transfer.

    ✅ Confirm Transaction:

        Confirm the transaction in your wallet and wait for the cross-chain bridge to process it.

    🎉 Completion:

        Once the transaction is complete, the tokens will appear on the target chain.

📚 Additional Resources

    Hetu Bridge Documentation

    ERC-7683 Standard

    Cross-Chain Interoperability Guide