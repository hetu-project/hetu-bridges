graph TD
    A[Start] --> B[Gravity Bridge Overview]
    B --> C[Lock Tokens on Ethereum]
    B --> D[Mint Tokens on Cosmos]
    C --> E[Send Tokens to Gravity Contract]
    E --> F[Specify Cosmos Address]
    F --> G[Lock Tokens in Gravity.sol]
    G --> H[Emit Event to Cosmos Validators]
    H --> I[Mint Tokens on Cosmos]
    I --> J[Use Tokens on Cosmos]
    J --> K[Send Tokens Back to Ethereum]
    K --> L[Send Tokens to Gravity Module]
    L --> M[Specify Ethereum Address]
    M --> N[Burn Tokens on Cosmos]
    N --> O[Send Transaction to Ethereum]
    O --> P[Unlock Tokens on Ethereum]
    P --> Q[End]

    B --> R[Parts of Gravity]
    R --> S[Gravity.sol]
    R --> T[Gravity Cosmos Module]
    R --> U[Orchestrator Program]
    R --> V[Relayer Market]

    S --> W[Lock ERC20 Tokens]
    W --> X[Store Checkpoint]
    X --> Y[Methods Controlled by Validators]
    Y --> Z[updateValset]
    Y --> AA[sendToCosmos]
    Y --> AB[submitBatch]

    T --> AC[Monitor Ethereum Events]
    AC --> AD[Receive Events]
    AD --> AE[Process Events]
    AE --> AF[Mint Tokens]
    AF --> AG[Remove Batches]

    U --> AH[Sign Messages]
    AH --> AI[Submit to Cosmos Chain]

    V --> AJ[Submit Batches to Ethereum]
    AJ --> AK[Check Signatures]
    AK --> AL[Check Nonce]
    AL --> AM[Transfer Tokens]
    AM --> AN[Send Fees to Relayer]

    R --> AO[Ethereum Oracle]
    AO --> AP[Monitor Ethereum Chain]
    AP --> AQ[Receive Events]
    AQ --> AR[Process Events]
    AR --> AS[Update Cosmos State]

    R --> AT[Transaction Batching]
    AT --> AU[Assemble Batches]
    AU --> AV[Choose Transactions]
    AV --> AW[Include in Batch Pool]
    AW --> AX[Sign Batches]
    AX --> AY[Submit to Ethereum]
    AY --> AZ[Remove Batches]
    AZ --> BA[Return Transactions to Pool]

    R --> BB[Validator Set Updates]
    BB --> BC[Request Update]
    BC --> BD[Save Snapshot]
    BD --> BE[Sign Snapshot]
    BE --> BF[Submit to Ethereum]
    BF --> BG[Update Checkpoint]

    R --> BH[Slashing]
    BH --> BI[Liveness Slashing]
    BH --> BJ[Fake Batches/Validator Sets Slashing]
    BI --> BK[Grace Period]
    BK --> BL[Slash Validator]
    BJ --> BM[Submit Evidence]
    BM --> BN[Slash Validator]

    Q --> C

## 流程图的主要部分：
    
- Gravity Bridge Overview：概述 Gravity 桥的功能。

- Lock Tokens on Ethereum 和 Mint Tokens on Cosmos：描述从以太坊到 Cosmos 的代币转移过程

- Send Tokens Back to Ethereum：描述从 Cosmos 返回以太坊的代币转移过程。

## Gravity 的四个组成部分：

- Gravity.sol：负责锁定以太坊上的 ERC20 代币。

- Gravity Cosmos Module：负责在 Cosmos 上铸造代币。

- Orchestrator Program：运行在每个验证者上，用于处理签名和提交消息。

- Relayer Market：负责将交易提交到以太坊。

## 关键功能：

- updateValset：更新验证者集。

- sendToCosmos：将代币从以太坊发送到 Cosmos。

- submitBatch：将 Cosmos 上的交易批量提交到以太坊.

- Ethereum Oracle：监控以太坊链上的事件。

- Transaction Batching：将 Cosmos 到以太坊的交易批量处理。

- Validator Set Updates：更新 Gravity.sol 中的验证者集。

- Slashing：对行为不当的验证者进行惩罚。