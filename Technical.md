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

# 拓展 ERC-7683

**`erc-7683优势`**

ERC-7683 协议在跨链转移中有以下几个优势：

1. 简化跨链流程：

- 通过标准化的锁仓、映射和解锁流程，ERC-7683 协议简化了跨链交易的操作，无需过多依赖第三方托管，增强了去中心化和安全性。

2. 增强的可扩展性：

- 支持多链之间的资产互通，不局限于一个特定的链。这对于多个 EVM 兼容的区块链生态非常有利。

3. 提高透明度：

- ERC-7683 提供了开放事件和详细的交易记录，使得跨链操作更加透明，用户可以随时查询跨链交易的状态和进度。

4. 安全性和去信任化：

- ERC-7683 协议要求多个验证节点签署交易，避免了单一中介的信任问题。这些验证者按比例参与，并且具有对整个跨链操作的最终审核权限。

5. 灵活性：

- 提供了 fillerData 等可扩展字段，允许开发者根据自己的需求添加额外的数据，提高了协议的灵活性和适应性。

## 与Hetu-bridge的融合

### Order structs - 订单结构体

Order 结构体定义了跨链交易的基本信息。它是一个数据结构，包含跨链资产转移的详细信息。具体来说，Order 结构体可能包括以下字段：

- 源链和目标链的标识：指定这笔交易是在什么链之间进行的。
- 转账的代币类型：指定要跨链转移的资产类型，通常是一个 ERC-20 代币。
- 转账的数量：指定转移的资产数量。
- 发起方和接收方的地址：分别是源链和目标链的用户地址。

**`使用场景：`**

跨链交易订单：用户在发起跨链转账时，Order 结构体可以被用来定义交易的源和目标链、转账资产、数量以及相关的用户地址等信息。

### ResolvedCrossChainOrder struct - 已解析的跨链订单

ResolvedCrossChainOrder 是 Order 结构体的扩展版本，包含跨链交易的详细信息及执行结果。当跨链转账操作完成时，ResolvedCrossChainOrder 会被创建，用来记录跨链交易的状态。

通常它包含以下信息：

- 交易状态：例如“已完成”、“处理中”等状态。
- 源链和目标链的交易哈希：可以用于追踪交易的执行状态。
- 转账结果：成功转移或失败的信息。

**`使用场景：`**

交易结果查询：当跨链操作完成后，用户可以通过 ResolvedCrossChainOrder 查询交易的状态，确保交易的执行结果。
提供交易审计功能：该结构体可以为开发者提供查询跨链交易结果的工具，并记录交易的哈希值，以便进行透明的审计。

### Open event - 跨链交易事件

Open 事件是跨链订单的创建事件。每当用户发起一个新的跨链转账请求时，Open 事件会被触发，表明一笔新的跨链交易已经被创建。

事件内容包括：

- 订单ID（标识该跨链交易的唯一ID）。
- 订单的源链和目标链。
- 转账代币的类型和数量。
- 用户的源链和目标链地址。

**`使用场景：`**

跨链交易开始时触发：当用户在某个链上发起跨链交易时，通过监听 Open 事件，可以及时知道这笔交易是否已成功提交。
事件订阅：开发者可以订阅该事件，在自己的应用中获取跨链交易的创建信息，并及时反馈给用户。

### Settlement interfaces - 结算接口

结算接口是 ERC-7683 协议中的核心部分，它定义了如何处理跨链交易的结算过程。当交易成功执行时，结算接口会确保资产被正确地解锁、销毁或转移。

结算过程通常包括：

- 资产的解锁和转移：源链上的资产会被解锁并转移到目标链上。
- 交易状态更新：在交易完成后，结算接口会触发状态更新，标记该跨链操作为成功或失败。

**`使用场景：`**

跨链交易结算：当目标链上的资产被成功映射时，结算接口确保这笔交易完成并最终确认。
跨链资产的实际转移：结算接口负责与目标链的合约交互，确保资产的最终转移或销毁。

### fillerData - 填充数据

fillerData 是一种用于跨链交易的附加信息，它包含额外的、非必需的、由开发者自定义的数据。这个字段通常是用于提供一些辅助信息，如交易备注、标识符或者其他非核心的数据。

**`使用场景：`**

跨链交易的附加信息：用户可以在发起跨链交易时，附带一些特定的额外信息（如备注），这些信息将被嵌入到交易中。
交易扩展：开发者可以根据需求使用 fillerData 来支持额外功能，如提供交易指令、附加的标识符或用于其他验证。

以上是erc-7683的协议标准的介绍和核心方法的使用

要将 ERC-7683 协议融入 Hetu-Bridge 中，核心目标是增强跨链资产的互通性，同时支持 ERC-20 代币 在 EVM 和 Cosmos 生态系统之间的流动。在这个过程中，Hetu-Bridge 需要能够与 ERC-7683 协议集成，从而实现更简化和标准化的跨链交易流程.

**`基本目标`**
跨链资产转移：用户能够在 Ethereum 和 Cosmos 间自由地转移 ERC-20 代币。
简化跨链交互：利用 ERC-7683 简化跨链订单创建、处理和结算过程。

**`改造过程`**

将ERC-7683融入hetu-bridge中主要是修改我们的跨链合约和getu-bridge中对order的支持

1. 创建跨链订单（Order struct）

首先，在 Ethereum 侧用户发起跨链资产转移时，需要创建一个 Order 结构体。这个结构体包含跨链转移的所有关键信息。Hetu-Bridge 会用这个信息来确保在目标链上正确铸造资产。

在 Gravity-Bridge 中的集成：

- 用户在 Ethereum 侧：提交 ERC-20 代币到 Gravity.sol 合约。

在现有的 Gravity.sol 中，添加新的功能以支持 ERC-7683 协议的 Order struct。这将使得用户在 Ethereum 侧提交跨链转账时，能够通过 sendToCosmos 方法生成跨链订单，并将订单信息保存在合约中。

Order struct 需要包含的信息：
  - 源链地址（Ethereum 地址）
  - 目标链地址（Cosmos 地址）
  - ERC-20 代币信息（如 DAI）
  - 转账数量


```solidity
struct Order {
    address userAddress;     // 用户地址
    uint256 amount;          // 转账数量
    address token;           // ERC-20 代币地址
    string destinationChain; // 目标链标识
    bytes fillerData;        // 可选的附加数据
}

event OpenOrder(
    address indexed user,
    address indexed token,
    uint256 amount,
    string destinationChain,
    bytes fillerData
);

```
- sendToCosmos 方法：

  - 修改 sendToCosmos 方法以支持新的 Order struct 格式。
  - 当用户调用 sendToCosmos 转移代币时，确保通过事件 (OpenOrder) 通知 Cosmos 侧，  Cosmos 侧将基于这些信息铸造相应数量的代币

```solidity
function sendToCosmos(address user, uint256 amount, address token, string memory destinationChain, bytes memory fillerData) public {
    // 锁定ERC-20代币
    ERC20(token).transferFrom(user, address(this), amount);
    
    // 触发跨链转移的事件
    emit OpenOrder(user, token, amount, destinationChain, fillerData);
}
```

- submitBatch 方法：

  - 需要确保通过 submitBatch 提交批次交易时包含 ERC-7683 相关的订单数据。
  - 处理批量交易时的逻辑，确保交易记录符合 ERC-7683 协议的要求

- 在 Gravity-Bridge 中：发送到 Ethereum：创建并发送 Order 结构体，触发跨链交易。

需要修改的内容：
- ResolvedCrossChainOrder 结构体：

  - 在 Cosmos 侧创建一个新的 ResolvedCrossChainOrder 结构体，用来表示跨链订单的完成状态。
  - 包括代币类型、数量、用户地址以及交易状态等信息。

```go
type ResolvedCrossChainOrder struct {
    UserAddress string `json:"userAddress"`
    Amount      uint64 `json:"amount"`
    Token       string `json:"token"`
    Success     bool   `json:"success"`
    Status      string `json:"status"`
}
```

- Orchestrator（协调器）的修改

需要修改协调器的事件监听逻辑，确保它能够正确识别并处理 ERC-7683 相关的事件，并将这些事件传递到正确的hetu-bridge模块。

- Relayers 模块 的修改

- 支持 ERC-7683 的批次提交：
  - 修改 Relayers 模块以支持处理 ERC-7683 相关的批次交易。
  - Relayers 需要能够识别不同的交易批次，并确保它们符合 ERC-7683 的要求。
- 签名和验证：
  - 在 Relayers 进行跨链交易时，必须确保交易信息的签名和验证符合 ERC-7683 的规定。

2.  触发跨链交易（Open event）

当用户提交跨链交易时，会触发 Open 事件，标志着一个跨链订单的开始。在 Gravity-Bridge 中，你需要监听这个事件，并在 hetu-bridge 上的 Gravity 模块中生成相应的映射资产。

- 在 Gravity-Bridge 中的集成：
  - 当 Gravity.sol 收到 ERC-20 代币后，锁定代币并触发 Open 事件。
  - 事件中会包含跨链转移的基本信息（如 ERC-20 代币类型和数量）