
[中文](#内容介绍) | [English](#introduction)
## 内容介绍
在这部分代码中，我们构建一个 ERC721 的合约，让这个合约可以被从 Sepolia 区块链被跨链跨到 Amoy 区块链。<br>

完成整个过程需要先在 Sepolia 区块链部署合约：
- ERC-721合约 MyToken：这个合约是我们需要用到的 NFT
- NFTPoolLockAndRelease：用来锁定用户合约，并且执行跨链操作，在 Amoy 区块链上铸造一个新的 NFT

在 Amoy 区块链上部署合约
- 基于 ERC-721 合约的包装合约 WrappedMyToken：这个合约会用来铸造和燃烧 NFT，因为 NFT 的主合约在 Sepolia 上，所在 Amoy 的 NFT 合约需要先进行铸造。
- NFTPoolMintAndBurn：通过 `ccipReceive` 来接受跨链消息，然后基于消息内容铸造 NFT，同时在 Amoy 中的 NFT 跨链回到 Sepolia 的时候，将 NFT 进行燃烧。

## 如何使用
1. 将 repo clone到本地：
`git clone https://github.com/Jonyth-an/cross-chain.git`

1. 设置npm源
   
2. 安装 npm package：`npm install`

3. 测试合约：`npx hardhat test`，此过程使用到了 chainlink-local，会在链下模拟 ccip 行为

4. 通过 env-enc 添加配置信息：
```
npx env-enc set-pw
npx env-enc set
```
依次加入环境变量：
```
PRIVATE_KEY
SEPOLIA_RPC_URL
AMOY_RPC_URL
```

1. 在 source chain 部署合约：`npx hardhat deploy --tags sourcechain --network sepolia`，如果你在上一步使用的不是 sepolia 和 amoy，那么请相应调整 network 名字

2. 在 dest chain 部署合约：`npx hardhat deploy --tags destchain --network amoy` 如果你在上一步使用的不是 sepolia 和 amoy，那么请相应调整 network 名字

3. 铸造 nft：`npx hardhat mint-nft --network sepolia`

4. 查看 nft 状态：`npx hardhat check-nft --network sepolia`

5.  锁定并且跨链 nft：`npx hardhat lock-and-cross --tokenid 0 --network sepolia`

6.  查看 wrapped NFT 状态：`npx hardhat check-wrapped-nft --tokenid 0 --network amoy`

7.  燃烧并且跨链 wnft：`npx hardhat burn-and-cross --tokenid 0 --network amoy`

8.  再次查看 nft 状态：`npx hardhat check-nft --network sepolia`




## Introduction
In this part of the code, we build an ERC721 contract that allows the NFT to be cross-chained from the Sepolia blockchain to the Amoy blockchain.<br>

To complete the entire process, it is necessary to deploy contracts on the Sepolia blockchain first:

- ERC-721 contract MyToken: This contract is the NFT we need.
- NFTPoolLockAndRelease: Used to lock user contracts and perform cross-chain operations, minting a new NFT on the Amoy blockchain.

Deploy contracts on the Amoy blockchain:

- WrappedMyToken, a wrapper contract based on the ERC-721 contract: This contract is used to mint and burn NFTs. Because the main NFT contract is on Sepolia, the NFT contract on Amoy needs to be minted first.
- NFTPoolMintAndBurn: Used to receive cross-chain messages through ccipReceive, mint NFTs based on message content, and burn NFTs when they are cross-chained back to Sepolia from Amoy.
## Getting started
1. Clone the repo to your local machine:
`git clone https://github.com/Jonyth-an/cross-chain.git`

1. Install npm packages: `npm install`

2. Test contracts: `npx hardhat test`. This process uses chainlink-local to simulate ccip behavior on the chain.

3. Add configuration information through env-enc:
```
npx env-enc set-pw
npx env-enc set
```
Add environment variables in sequence:
```
PRIVATE_KEY
SEPOLIA_RPC_URL
AMOY_RPC_URL
```

1. Deploy contracts on the source chain: `npx hardhat deploy --tags sourcechain --network sepolia`. If you did not use sepolia and Amoy in the previous step, adjust the network name accordingly.

2. Deploy contracts on the dest chain: `npx hardhat deploy --tags destchain --network amoy`. If you did not use sepolia and Amoy in the previous step, adjust the network name accordingly.

3. Mint nft: `npx hardhat mint-nft --network sepolia`

4. Check nft status: `npx hardhat check-nft --network sepolia`

5.  Lock and cross nft: `npx hardhat lock-and-cross --tokenid 0 --network sepolia`

6.  Check wrapped NFT status: `npx hardhat check-wrapped-nft --tokenid 0 --network amoy`

7.  Burn and cross wnft: `npx hardhat burn-and-cross --tokenid 0 --network amoy`

8.  Check nft status again：`npx hardhat check-nft --network sepolia`

