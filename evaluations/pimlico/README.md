# Pimlico评估报告

## 介绍

Pimlico是一个专注于账户抽象(Account Abstraction)的基础设施平台，提供可靠、高性能的ERC-4337实现。Pimlico的服务主要包括Bundler和Paymaster，旨在帮助开发者轻松实现无Gas交易和改善用户体验。

## 主要产品

1. **Bundler**:
   - 实现了ERC-4337标准的UserOperation处理
   - 提供高性能、可靠的交易打包服务
   - 支持多链部署

2. **Verifying Paymaster**:
   - 允许开发者代替用户支付Gas费用
   - 通过验证机制控制赞助策略
   - 支持条件性赞助

3. **ERC-20 Paymaster**:
   - 允许用户使用ERC-20代币支付Gas费用
   - 支持多种主流ERC-20代币
   - 基于实时汇率进行代币兑换

4. **Singleton Paymaster**:
   - 开源的通用Paymaster合约
   - 可自定义赞助政策
   - 支持多种验证方式

5. **Alto Bundler**:
   - 基于TypeScript的高性能Bundler实现
   - 开源项目，可自行部署
   - 支持完整的ERC-4337规范

## 关键能力

1. **跨链支持**:
   - 支持多条主流EVM兼容区块链
   - 包括Ethereum Mainnet, Polygon, Optimism, Arbitrum, Base等

2. **开发者工具**:
   - 提供完整的SDK和API
   - 支持TypeScript/JavaScript集成
   - 与viem/ethers.js库无缝兼容

3. **赞助政策管理**:
   - 可定制的Gas赞助策略
   - 基于条件的交易赞助
   - Webhook支持进行外部验证

4. **可靠性和性能**:
   - 高可用性基础设施
   - 低延迟交易处理
   - 实时监控和警报系统

## 示例演示

我们通过Pimlico的官方教程实现了一个简单的无Gas交易示例。该示例使用Safe智能合约钱包和Pimlico的Verifying Paymaster，完全无需用户支付Gas费用就能发送交易。

### 主要步骤:

1. 创建Safe智能账户
2. 设置Pimlico客户端(Bundler和Paymaster)
3. 构建交易
4. 通过Paymaster请求Gas赞助
5. 使用Bundler发送UserOperation到链上

### 技术栈:

- TypeScript
- Viem
- Permissionless.js

## 使用方法

### 安装

```bash
npm install permissionless viem
```

### 基本用法

```typescript
// 创建Pimlico客户端
const pimlicoClient = createPimlicoClient({
  transport: http(pimlicoUrl),
  entryPoint: {
    address: entryPoint07Address,
    version: "0.7",
  },
})

// 创建智能账户
const account = await toSafeSmartAccount({
  client: publicClient,
  owners: [privateKeyToAccount(privateKey)],
  entryPoint: {
    address: entryPoint07Address,
    version: "0.7",
  },
  version: "1.4.1",
})

// 创建智能账户客户端
const smartAccountClient = createSmartAccountClient({
  account,
  chain: sepolia,
  bundlerTransport: http(pimlicoUrl),
  paymaster: pimlicoClient,
  userOperation: {
    estimateFeesPerGas: async () => {
      return (await pimlicoClient.getUserOperationGasPrice()).fast
    },
  },
})

// 发送无Gas交易
const txHash = await smartAccountClient.sendTransaction({
  to: targetAddress,
  value: 0n,
  data: "0x1234",
})
```

## 评估总结

### 主要功能和用户场景

Pimlico提供完整的账户抽象基础设施，重点优化了以下用户场景：
- 无Gas onboarding体验
- Web3游戏中的顺畅交易
- DApp中的Gas赞助
- 使用ERC-20代币支付Gas

### 开发者复杂度

- **学习曲线**: 中等
- **文档质量**: 高，提供详细教程和API参考
- **集成难度**: 低到中等，有完善的SDK支持
- **调试工具**: 提供调试工具和详细错误信息

### 第三方ERC20 Gas代币接受度

- 支持多种主流ERC-20代币
- 实时定价机制
- 可扩展支持自定义ERC-20代币

### Gas赞助支付方式

- 开发者API密钥授权
- 可定制的赞助政策
- 基于条件的赞助逻辑
- Webhook验证集成

### 开源状态

- Bundler (Alto): 完全开源
- Singleton Paymaster: 开源
- SDK/客户端库: 开源
- 基础设施和API服务: 闭源，作为服务提供

### SDK核心能力

- **SDK名称**: permissionless.js (基于viem构建)
- **安装**: `npm install permissionless viem`
- **主要功能**: 
  - 智能账户创建和管理
  - UserOperation构建和签名
  - Bundler交互
  - Paymaster集成
  - Gas估算

## 优缺点分析

### 优点

1. 优秀的开发者体验和文档
2. 高性能和可靠的基础设施
3. 强大的跨链支持
4. 灵活的Gas赞助机制
5. 开源核心组件
6. 与现有智能合约钱包的良好集成

### 缺点

1. API密钥方式需要中心化管理
2. 作为服务使用时存在依赖单一提供商风险
3. 对高频交易的成本可能较高
4. 基础设施层面的去中心化程度有限

## 整体评价

Pimlico为账户抽象实现提供了非常全面和专业的解决方案，特别适合注重用户体验的DApp开发者。他们的工具链成熟且易于集成，文档和教程质量高，对开发者非常友好。在Gas赞助和无Gas交易方面的能力尤为突出，可以显著改善Web3应用的用户体验。

虽然作为中心化服务存在一定依赖性，但开源核心组件使开发者有自行部署的选择。总体而言，Pimlico是目前账户抽象领域最成熟和完善的解决方案之一。 


## Run
git submodule add  https://github.com/pimlicolabs/tutorial-template.git evaluations/pimlico/pimlico-verifying-tutorial

git submodule add https://github.com/pimlicolabs/tutorial-template.git evaluations/pimlico/pimlico-tutorial-example

replate index.ts with two file outside:toutorial-index.ts,verifying-index.ts
then pnpm start,got below:

===
pnpm start

> pimlico-tutorial-template@1.0.0 start /Users/nicolasshuaishuai/dev/aastar/AAStar-Demo/pimlico-tutorial-example
> tsx index.ts

Hello world!
Public and Pimlico clients created.
Using private key: ********** (loaded)
Pimlico API Key used: ********** (loaded)
Smart account (Safe) generated with address: 0x2D2145fd3429922e16771E9DA4AF6062E7EabD45
Smart account client created for address: 0x2D2145fd3429922e16771E9DA4AF6062E7EabD45
View your smart account on Base Sepolia: https://sepolia.basescan.org/address/0x2D2145fd3429922e16771E9DA4AF6062E7EabD45
Smart account USDC balance: 9.998463 USDC

Attempting to send a transaction from the smart account...
Approving Pimlico Verifying Paymaster (0x777777777777AeC03fd955926DbF81597e66834C) to spend USDC...
Transaction hash: 0xe6643d7c1e0e3854df827b795cfdffd8330c12116c63b8b61eac57f3591b1bbe
User operation included: https://sepolia.basescan.org/tx/0xe6643d7c1e0e3854df827b795cfdffd8330c12116c63b8b61eac57f3591b1bbe

Tutorial completed successfully!

===
pnpm start

> pimlico-tutorial-template@1.0.0 start /Users/nicolasshuaishuai/dev/aastar/AAStar-Demo/pimlico-verifying-tutorial
> tsx index.ts

Starting Pimlico Verifying Paymaster Tutorial (Tutorial 1 adapted)...
Private key loaded/generated.
Public client for Sepolia created.
Pimlico client for Sepolia created.

Starting main execution for Verifying Paymaster tutorial...
Smart account address (Sepolia): https://sepolia.etherscan.io/address/0x2D2145fd3429922e16771E9DA4AF6062E7EabD45
SmartAccountClient created for address: 0x2D2145fd3429922e16771E9DA4AF6062E7EabD45
Attempting to send a transaction to 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 using Verifying Paymaster...
Transaction hash: 0x23c1152b349630f1654e3baf0d430965d3a0bcde2e315b0610b1a8c6853cd7ca
User operation included: https://sepolia.etherscan.io/tx/0x23c1152b349630f1654e3baf0d430965d3a0bcde2e315b0610b1a8c6853cd7ca

Verifying Paymaster tutorial script completed successfully!

====


(base) ➜  pimlico-tutorial-example git:(main) ✗ pnpm start

> pimlico-tutorial-template@1.0.0 start /Users/nicolasshuaishuai/dev/aastar/EvaluationAllPaymaster/evaluations/pimlico/pimlico-tutorial-example
> tsx index.ts

Hello world!
Public and Pimlico clients created.
Using private key: ********** (loaded)
Pimlico API Key used: ********** (loaded)
Smart account (Safe) generated with address: 0x2D2145fd3429922e16771E9DA4AF6062E7EabD45
Smart account client created for address: 0x2D2145fd3429922e16771E9DA4AF6062E7EabD45
View your smart account on Base Sepolia: https://sepolia.basescan.org/address/0x2D2145fd3429922e16771E9DA4AF6062E7EabD45
Smart account USDC balance: 9.997819 USDC

Attempting to send a transaction from the smart account...
Approving Pimlico Verifying Paymaster (0x777777777777AeC03fd955926DbF81597e66834C) to spend USDC...
Transaction hash: 0xdd38d39bbe9d84478a6bea3bdf8e6d85968f45abcb47113baabfe5e1aa520b32
User operation included: https://sepolia.basescan.org/tx/0xdd38d39bbe9d84478a6bea3bdf8e6d85968f45abcb47113baabfe5e1aa520b32

Tutorial completed successfully!
^C ELIFECYCLE  Command failed with exit code 130.
(base) ➜  pimlico-tutorial-example git:(main) ✗ cd 。。
cd: no such file or directory: 。。
(base) ➜  pimlico-tutorial-example git:(main) ✗ ls    
LICENSE           README.md         index.ts          node_modules      package-lock.json package.json      pnpm-lock.yaml    tsconfig.json
(base) ➜  pimlico-tutorial-example git:(main) ✗ cd 。。
cd: no such file or directory: 。。
(base) ➜  pimlico-tutorial-example git:(main) ✗ CD ..
(base) ➜  pimlico-tutorial-example git:(main) ✗ LS
LICENSE                 README.md               index.ts                node_modules            package-lock.json       package.json            pnpm-lock.yaml          tsconfig.json
(base) ➜  pimlico-tutorial-example git:(main) ✗ ls
LICENSE           README.md         index.ts          node_modules      package-lock.json package.json      pnpm-lock.yaml    tsconfig.json
(base) ➜  pimlico-tutorial-example git:(main) ✗ pwd
/Users/nicolasshuaishuai/dev/aastar/EvaluationAllPaymaster/evaluations/pimlico/pimlico-tutorial-example
(base) ➜  pimlico-tutorial-example git:(main) ✗ cd 。。
cd: no such file or directory: 。。
(base) ➜  pimlico-tutorial-example git:(main) ✗ cd ..
(base) ➜  pimlico git:(main) ✗ cd pimlico-verifying-tutorial 
(base) ➜  pimlico-verifying-tutorial git:(main) ✗ pnpm install
Lockfile is up to date, resolution step is skipped
Packages: +25
+++++++++++++++++++++++++
Progress: resolved 25, reused 25, downloaded 0, added 25, done

dependencies:
+ dotenv 16.5.0
+ permissionless 0.2.46
+ viem 2.29.2

devDependencies:
+ @types/node 20.17.46
+ tsx 3.14.0

╭ Warning ───────────────────────────────────────────────────────────────────────────────────╮
│                                                                                            │
│   Ignored build scripts: esbuild.                                                          │
│   Run "pnpm approve-builds" to pick which dependencies should be allowed to run scripts.   │
│                                                                                            │
╰────────────────────────────────────────────────────────────────────────────────────────────╯

Done in 1s using pnpm v10.9.0
(base) ➜  pimlico-verifying-tutorial git:(main) ✗ ls
LICENSE           README.md         index.ts          node_modules      package-lock.json package.json      pnpm-lock.yaml    tsconfig.json
(base) ➜  pimlico-verifying-tutorial git:(main) ✗ pnpm start

> pimlico-tutorial-template@1.0.0 start /Users/nicolasshuaishuai/dev/aastar/EvaluationAllPaymaster/evaluations/pimlico/pimlico-verifying-tutorial
> tsx index.ts

Starting Pimlico Verifying Paymaster Tutorial (Tutorial 1 adapted)...
Private key loaded/generated.
Public client for Sepolia created.
Pimlico client for Sepolia created.

Starting main execution for Verifying Paymaster tutorial...
Smart account address (Sepolia): https://sepolia.etherscan.io/address/0x2D2145fd3429922e16771E9DA4AF6062E7EabD45
SmartAccountClient created for address: 0x2D2145fd3429922e16771E9DA4AF6062E7EabD45
Attempting to send a transaction to 0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045 using Verifying Paymaster...
Transaction hash: 0x7ed9e066abd1870a27607ff86ac05eafd535355e1cf7e0ca2b298d0f3c0e4ae3
User operation included: https://sepolia.etherscan.io/tx/0x7ed9e066abd1870a27607ff86ac05eafd535355e1cf7e0ca2b298d0f3c0e4ae3

Verifying Paymaster tutorial script completed successfully!
^C ELIFECYCLE  Command failed with exit code 130.
(base) ➜  pimlico-verifying-tutorial git:(main) ✗ 


