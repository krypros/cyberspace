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

## 4. ganache-cli使用
- 安装（要求Node.js>=v8  npm ）
在目录dapp/prepare/ganache  
`sudo npm -g install npm@5.6.0`  
`sudo npm install -g ganache-cli`
> 可能出现的问题：npm与node版本不对应Node@8.10.0； npm@5.6.0


输入启动:`ganache-cli`，显示分配账户与对应私钥  
![ganache.jpg](./image/Dapp%20begin/ganache-cli.jpg)
(在内存中模拟区块链)

- 使用web3js进行测试  
https://web3js.readthedocs.io/en/v1.7.5/

创建新文件夹用于测试：ganacheTest
```
1、安装web3js
npm install webs -save

2、测试文件web3js与ganache交互
  var Web3 = require('web3')
  console.log("test")
  var web3 = new Web3(new Web3.provider.HttpProvider('https://localhost:8545'));
  console.log(web3.version);
  web3.eth.getAccounts().then(console.log);
```
> 可能出现问题，404 -> 
> 全局安装n管理器(用于管理nodejs版本) 
> `sudo npm install n -g`  
> 安装最新的nodejs（stable版本） -> `sudo n stable`; `sudo node -v`
> 安装web3.js -> `sudo npm install web3`; `sudo npm install web3@^0.20.0`
> 更新npm -> `sudo npm i npm to update`

`vim test.js`  # 创建test
`node test.js`  # 启动test

## 5. 使用web3js与区块链进行交互
使用express来创建工程
- express安装
`sudo npm install express -g`
> 4.x版本以后，generator被分离出来了
如果我们想成功，还必须安装express-generator
> 清除缓存 npm cache clean --force（删除express目录）

- 创建工程
```
express -e MyDapp
cd MyDapp
npm install
npm start
```
- 和区块链交互（本地安装）
`npm install web3 -save`

修改routes/index.js代码：
```
var express = require('express');
var router = express.Router();

var Web3 = require('web3');
var web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));

web3.eth.getAccounts().then(console.log);

/* GET home page. */
router.get('/', function(req, res, next) {
  web3.eth.getAccounts().then(function(accounts) {
    var account = accounts[0]; // 获取第一个账户余额
    web3.eth.getBalance(account).then(function(balance) {
      var amount = web3.utils.fromWei(balance, 'ether');
      res.render('index', {account: account, balance: amount});
    });
  });
});

module.exports = router;

```
修改app.js代码
```
var app = express();

var ejs = require('ejs');

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.engine('.html', ejs.__express);
app.set('view engine', 'html');
```
在view下创建index.html -> 显示页面
```js
<!DOCTYPE html>
<html>
  <head>
    <title>账户余额</title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1>账户是：<%= account %></h1>
    <h1>余额是：<%= balance %></h1>
  </body>
</html>
```
两种访问方式：
- geth(运行geth后再运行dapp)
- ganache(运行ganache-cli后再运行dapp)

在页面上显示第一个钱包的余额：
![dapp1.png](./image/Dapp%20begin/gapp1.png)

## 6. 部署智能合约
![智能合约](image/Dapp%20begin/1662472410416.png)

Solidity - 智能合约的开发语言  
Truffle - DApp开发框架

[在线编写Solidity](http://remix.ethereum.org/)

[智能合约入门文档](https://learnblockchain.cn/docs/solidity/introduction-to-smart-contracts.html)

（1）编写简单demo - Counter.sol
```
pragma solidity >=0.4.22 <0.8.0;
contract MyCounter {
    uint256 counter;

    constructor() public {
        counter = 1;
    }

    function kipCounter(uint step) public {
        counter += step;
    }

    function getCounter() public view returns (uint256) {
        return counter;
    }
}
```
在线编译：
![1662473711423](image/Dapp%20begin/1662473711423.png)
新建dapp文件夹deploy:  
`vim deploy.js`
```
var Web3 = require('web3');
var web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));

var mycounterContract = new web3.eth.Contract([{"inputs":[],"stateMutability":"nonpayable","type":"constructor"},{"inputs":[],"name":"getCounter","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[{"internalType":"uint256","name":"step","type":"uint256"}],"name":"kipCounter","outputs":[],"stateMutability":"nonpayable","type":"function"}]);

mycounterContract.options.data = "608060405234801561001057600080fd5b50600160008190555060cf806100276000396000f3fe6080604052348015600f57600080fd5b506004361060325760003560e01c80638ada066e146037578063e24801d8146053575b600080fd5b603d607e565b6040518082815260200191505060405180910390f35b607c60048036036020811015606757600080fd5b81019080803590602001909291905050506087565b005b60008054905090565b8060008082825401925050819055505056fea26469706673582212204513bc21deb55ffc124acaf2a544f296b161e1bd42ecb653765f0736dafff0b264736f6c63430006060033";

var mycounter = mycounterContract.deploy({
}).send({
     from: '0x7bf926845F9Ee3Ce3F366b0Bba4Adc664cA74299', 
     gas: '4700000'
   }, function (e, contract){
    console.log(e, contract);
    if (typeof contract.address !== 'undefined') {
         console.log('Contract mined! address: ' + contract.address + ' transactionHash: ' + contract.transactionHash);
    }
 }).then(function(address) {
    console.log("Contract address: ", address);
 })

```
在MyDapp2下创建`express -e dapp`  
修改app.js代码
```
var app = express();

var ejs = require('ejs');

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.engine('.html', ejs.__express);
app.set('view engine', 'html');
```

修改routes/index.js代码：
```js
var express = require('express');
var router = express.Router();

var Web3 = require('web3');
var web3 = new Web3(new Web3.providers.HttpProvider('http://localhost:8545'));

//web3.eth.getAccounts().then(console.log);

// 调用合约
var myContract = new web3.eth.Contract([
	{
		"inputs": [
			{
				"internalType": "uint256",
				"name": "step",
				"type": "uint256"
			}
		],
		"name": "kipCounter",
		"outputs": [],
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"inputs": [],
		"stateMutability": "nonpayable",
		"type": "constructor"
	},
	{
		"inputs": [],
		"name": "getCounter",
		"outputs": [
			{
				"internalType": "uint256",
				"name": "",
				"type": "uint256"
			}
		],
		"stateMutability": "view",
		"type": "function"
	}
], '0xaB5Fe699CCE14CE53e1acf7c76830A88AC2Ff167')

// 调用合约
// myContract.methods.getCounter().call({from: '0x8959177f36A7fFd4a121231428C0e0f65aC36C78'}, function(error, result){
//   console.log("counter: ", result);
// })

var init_counter = 0;
var current_counter = 0;

myContract.methods.getCounter().call().then(function(counter) {
  console.log('init counter: ', counter);
  init_counter = counter;
});

/* GET home page. */
router.get('/', function(req, res, next) {
  // 修改值
  myContract.methods.kipCounter(5).send({from: '0x8959177f36A7fFd4a121231428C0e0f65aC36C78'}).then(function(counter){
    myContract.methods.getCounter().call().then(function(counter) {
      console.log('current counter: ', counter);
      current_counter = counter;
      res.render('index', { init: init_counter, current: current_counter});
    });
  });
  
});

module.exports = router;
```

在view下创建index.html -> 显示页面
```js
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    # <title>调用智能合约</title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>

    <p>Welcome to <%= title %></p>
  </body>
</html>
```