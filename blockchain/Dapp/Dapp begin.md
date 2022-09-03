# 以太坊Dapp入门

## 1.Dapp原理

客户端通过**web3js**与**区块链**交互（数据防篡改）

## 2.Dapp开发环境介绍

- geth+js
- ganache+js

## 3.Geth私钥环境搭建

安装geth

```
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```

创建创始区块genesis.json

```json
{
  "config": {
    "chainId": 6500,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip155Block": 0,
    "eip158Block": 0,
    "byzantiumBlock": 0,
    "constantinopleBlock": 0,
    "petersburgBlock": 0,
    "istanbulBlock": 0,
    "berlinBlock": 0,
    "londonBlock": 0
  },
  "alloc": {},
  "coinbase": "0x0000000000000000000000000000000000000000",
  "difficulty": "0x200",
  "extraData": "0x",
  "gasLimit": "0x2fefd8",
  "nonce": "0x0000000000000042",
  "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp": "0x00"
}
```

| 参数       | 描述                                                                                           |
| ---------- | ---------------------------------------------------------------------------------------------- |
| alloc      | 可以预置账号以及账号的以太币数量                                                               |
| coinbase   | 区块受益者地址，可以设置成已存在的账户。后面挖出的区块的受益者将是挖掘出那个区块的账户（矿工） |
| difficulty | 代表当前区块的难度等级，后面生成的区块难度根据前一个区块的难度等级和时间戳计算得到             |
| extraData  | 一个包含这个区块相关数据的字节数组，任意填写。必须是32位以内                                   |
| gasLimit   | 执行这个事务应该被使用的gas的最大量。这个在任何计算被做之前是预付的，并且在之后不会增加        |
| nonce      | 代表从这个地址发送的事务数目，或者在关联代码的账户情况下，代表这个账户创造的合约数目           |
| mixhash    | 一个256位的hash，由nonce合并，证明在这个区块上已经执行足够量的计算                             |
| parentHash | 前一个父区块的header的keccak256算法hash                                                        |
| timestamp  | 这个区块开始的Unix的time()的合理输出                                                           |

- 初始化

> geth --datadir "chain" init genesis.json

- 启动geth

```
geth --identity "myetherum" --rpc --rpccorsdomain "*" --datadir "chain" --port "30303" --http.api "db, eth, net, web3, personal, miner" --networkid 6500 console 2>1.txt

geth --identity "myetherum" --http --http.corsdomain "*" --datadir "chain" --port "30303" --http.api "db, eth, net, web3, personal, miner" --networkid 6500 console 2>1.txt
```

| 旧版本                | 改变                    |
| --------------------- | ----------------------- |
| --rpc                 | --http                  |
| --rpcaddr value       | --http.addr value       |
| --rpcport value       | --http.port value       |
| --rpcapi value        | --http.api value        |
| --rpc.gascap value    | --http.rpcprefix value  |
| --rpccorsdomain value | --http.corsdomain value |
| --rpcvhosts value     | --http.vhosts value     |
