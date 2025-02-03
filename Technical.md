![流程图](./mermaid.png "Hetu Bridge Flowchart")
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

## 核心接口

### 1. Gravity.sol Contract (Ethereum 端)

#### 核心接口

- **`sendToCosmos(address recipient, uint256 amount)`**
  - **作用**：
    - 锁定资产：将用户在 Ethereum 上的 ERC-20 代币锁定在 Gravity Bridge 的智能合约中。
    - 触发跨链事件：生成一个跨链事件，通知 Cosmos 链上的验证者（Validators）有资产需要转移到指定的 Cosmos 地址。
    - 启动跨链流程：通过该接口，用户可以将 Ethereum 上的资产安全地转移到 Cosmos 生态系统中。
  - **参数说明**：
    - `address recipient`：目标 Cosmos 链上的接收地址（以 Ethereum 地址格式表示）。
    - `uint256 amount`：要转移的代币数量（以代币的最小单位表示，例如 Wei 或 Token 的最小精度单位）。

- **`submitBatch()`**
  - **作用**：负责批量提交从 Cosmos 到 Ethereum 的交易请求。
  - **参数**：
    - `amounts (uint256[] memory)`：包含所有批次交易的金额的数组。
    - `recipients (address[] memory)`：包含所有批次交易的目标地址的数组。
    - `fees (uint256[] memory)`：包含每笔交易的手续费的数组。
    - `batchNonce (uint256)`：交易批次的唯一标识符。
    - `signatures (bytes[] memory)`：包含 Cosmos 验证者签署的每个交易批次的签名的字节数组。

- **`updateValset(address[] memory validators, uint256[] memory powers)`**
  - **作用**：用于更新 Cosmos 验证者集（validator set）的信息。该操作只能由 Cosmos 验证者签名，并确保以太坊合约中的验证者集信息与 Cosmos 验证者一致。
  - **参数**：
    - `address[] memory validators`：验证者的地址。
    - `uint256[] memory powers`：对应验证者的权重。

### 2. Gravity Cosmos Module (Cosmos 端)

#### 核心接口

- **`mintTokens(address to, uint256 amount)`**
  - **作用**：根据 Gravity.sol 合约的事件，铸造对应数量的代币，并发送到指定的 Cosmos 地址。
  - **参数**：
    - `to sdk.AccAddress`：目标地址。
    - `amount sdk.Int`：铸造的代币数量。

- **`submitToEthereum(address ethAddress, uint256 amount)`**
  - **作用**：将代币从 Cosmos 转移到以太坊，首先从 Cosmos 账户扣除代币，然后将交易信息提交到 Ethereum 网络。
  - **参数**：
    - `ethAddress string`：Ethereum 网络对应的地址。
    - `amount sdk.Int`：转移的代币数量。

### 3. Orchestrator (协调程序)

#### 核心接口

- **`observeEthereumEvent(eventType string, eventData string)`**
  - **作用**：Orchestrator 用于监听和处理来自 Ethereum 的事件，例如 `SendToCosmosEvent` 或 `TransactionBatchExecutedEvent`，并将其转发到 Cosmos 网络的 Gravity 模块。
  - **参数**：
    - `eventType string`：事件类型。
    - `eventData string`：事件数据。

- **`submitToEthereum(batchData string)`**
  - **作用**：Orchestrator 协助提交交易批次到 Ethereum 合约，通过中继交易数据来实现跨链传输。
  - **参数**：
    - `batchData string`：交易批次数据。

### 4. Relayers (中继者)

#### 核心接口

- **`submitBatch(batchData string)`**
  - **作用**：Relayer 向 Ethereum 合约提交一个批次的交易数据。这个批次数据包含从 Cosmos 网络转移到 Ethereum 的代币信息。
  - **参数**：
    - `batchData string`：交易批次数据。

- **`observeBatchSubmission(batchNonce uint64)`**
  - **作用**：Relayer 观察交易批次的提交情况，并确认批次的执行。
  - **参数**：
    - `batchNonce uint64`：交易批次的 nonce 值。