## AETH FullNode 钱包接口文档

---

### 启动节点

启动节点方法如下

`./aeth-wallet`

启动节点并设置P2P端口和指定data目录

`./aleth --listen 33331 --db-path data`

---

### 默认目录

---


windows默认数据目录为：

`%APPDATA%\Ethereum`

Linux默认数据目录为：

`~/.ethereum`

windows默认Keystore目录为：

`%APPDATA%\Roaming\Web3\Keys`

Linux默认Keystore目录为：

`~/.web3/Keys`

---

### 节点RPC接口

---

#### 交易相关

---

## eth_gasPrice

返回当前的gas价格，单位：wei。

## 参数

    无

## 返回值

    * QUANTITY - 整数，以wei为单位的当前gas价格

## 示例代码

请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":73}'`

响应：
```
{
  "id":73,
  "jsonrpc": "2.0",
  "result": "0x09184e72a000" // 10000000000000
}
```
---

## eth_getTransactionCount

返回指定地址发生的交易数量。

## 参数
    DATA - 20字节，地址
    QUANTITY|TAG - 整数块编号，或字符串"latest"、"earliest"或"pending"
    params: [
       '0x407d73d8a49eeb85d32cf465507dd71d507100c1',
       'latest' // state at the latest block
    ]
## 返回值
    * QUANTITY - 从指定地址发出的交易数量，整数

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionCount","params":["0x407d73d8a49eeb85d32cf465507dd71d507100c1","latest"],"id":1}'`

响应：
```
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```
---

## eth_getBlockTransactionCountByHash

返回指定块内的交易数量，使用哈希来指定块。

## 参数

    DATA - 32字节，块哈希

    params: [
      '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
    ]

## 返回值

    * QUANTITY - 指定块内的交易数量，整数

## 示例代码

请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'`

响应：

```
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xb" // 11
}
```

---

## eth_getBlockTransactionCountByNumber

返回指定块内的交易数量，使用块编号指定块。

## 参数
    QUANTITY|TAG - 整数块编号，或字符串"earliest"、"latest"或"pending"

    params: [
       '0xe8', // 232
    ]

## 返回值

    * QUANTITY - 指定块内的交易数量

## 示例代码

请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByNumber","params":["0xe8"],"id":1}'`

响应：
```

{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xa" // 10
}
```
---

## eth_sendTransaction
创建一个新的消息调用交易，如果数据字段中包含代码，则创建一个合约。

## 参数
Object - 交易对象，结果如下：

* from: DATA, 20字节 - 发送交易的源地址
* to: DATA, 20字节 - 交易的目标地址，当创建新合约时可选
* gas: QUANTITY - 交易执行可用gas量，可选整数，默认值90000，未用gas将返还。
* gasPrice: QUANTITY - gas价格，可选，默认值：待定(To-Be-Determined)
* value: QUANTITY - 交易发送的金额，可选整数
* data: DATA - 合约的编译带啊或被调用方法的签名及编码参数
* nonce: QUANTITY - nonce，可选。可以使用同一个nonce来实现挂起的交易的重写

## 
    params: [{
      "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
      "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
      "gas": "0x76c0", // 30400
      "gasPrice": "0x9184e72a000", // 10000000000000
      "value": "0x9184e72a", // 2441406250
      "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
    }]

## 返回值
    * DATA, 32字节 - 交易哈希，如果交易还未生效则返回0值哈希

当创建合约时，在交易生效后，使用eth_getTransactionReceipt调用获取合约地址。

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendTransaction","params":[{see above}],"id":1}'`

响应：
```
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```
---

## eth_getBlockByHash

返回具有指定哈希的块。

## 参数
* DATA, 32字节 - 块哈希
* Boolean - 为true时返回完整的交易对象，否则仅返回交易哈希

##
    params: [
       '0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331',
       true
    ]

## 返回值
Object - 匹配的块对象，如果未找到块则返回null，结构如下：

* number: QUANTITY - 块编号，挂起块为null
* hash: DATA, 32 Bytes - 块哈希，挂起块为null
* parentHash: DATA, 32 Bytes - 父块的哈希
* nonce: DATA, 8 Bytes - 生成的pow哈希，挂起块为null
* sha3Uncles: DATA, 32 Bytes - 块中叔伯数据的SHA3哈希
* logsBloom: DATA, 256 Bytes - 快日志的bloom过滤器，挂起块为null
* transactionsRoot: DATA, 32 Bytes - 块中的交易树根节点
* stateRoot: DATA, 32 Bytes - 块最终状态树的根节点
* receiptsRoot: DATA, 32 Bytes - 块交易收据树的根节点
* miner: DATA, 20 Bytes - 挖矿奖励的接收账户
* difficulty: QUANTITY - 块难度，整数
* totalDifficulty: QUANTITY - 截止到本块的链上总难度
* extraData: DATA - 块额外数据
* size: QUANTITY - 本块字节数
* gasLimit: QUANTITY - 本块允许的最大gas用量
* gasUsed: QUANTITY - 本块中所有交易使用的总gas用量
* timestamp: QUANTITY - 块时间戳
* transactions: Array - 交易对象数组，或32字节长的交易哈希数组
* uncles: Array - 叔伯哈希数组

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByHash","params":["0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331", true],"id":1}'`

响应：
```
{
"id":1,
"jsonrpc":"2.0",
"result": {
    "number": "0x1b4", // 436
    "hash": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
    "parentHash": "0x9646252be9520f6e71339a8df9c55e4d7619deeb018d2a3f2d21fc165dde5eb5",
    "nonce": "0xe04d296d2460cfb8472af2c5fd05b5a214109c25688d3704aed5484f9a7792f2",
    "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    "logsBloom": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
    "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    "stateRoot": "0xd5855eb08b3387c0af375e9cdb6acfc05eb8f519e419b874b6ff2ffda7ed1dff",
    "miner": "0x4e65fda2159562a496f9f3522f89122a3088497a",
    "difficulty": "0x027f07", // 163591
    "totalDifficulty":  "0x027f07", // 163591
    "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "size":  "0x027f07", // 163591
    "gasLimit": "0x9f759", // 653145
    "gasUsed": "0x9f759", // 653145
    "timestamp": "0x54e34e8e" // 1424182926
    "transactions": [{...},{ ... }] 
    "uncles": ["0x1606e5...", "0xd5145a9..."]
  }
}
```
---

## eth_getBlockByNumber
返回指定编号的块。

## 参数
* QUANTITY|TAG - 整数块编号，或字符串"earliest"、"latest" 或"pending"
* Boolean - 为true时返回完整的交易对象，否则仅返回交易哈希
##
    params: [
       '0x1b4', // 436
       true
    ]

## 返回值

    * 参考eth_getBlockByHash的返回值。

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["0x1b4", true],"id":1}'`

响应：

```
参考eth_getBlockByHash
```

---

## eth_getTransactionByHash
返回指定哈希对应的交易。

## 参数
DATA, 32 字节 - 交易哈希

    params: [
       "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"
    ]

## 返回值
Object - 交易对象，如果没有找到匹配的交易则返回null。结构如下：

* hash: DATA, 32字节 - 交易哈希
* nonce: QUANTITY - 本次交易之前发送方已经生成的交易数量
* blockHash: DATA, 32字节 - 交易所在块的哈希，对于挂起块，该值为null
* blockNumber: QUANTITY - 交易所在块的编号，对于挂起块，该值为null
* transactionIndex: QUANTITY - 交易在块中的索引位置，挂起块该值为null
* from: DATA, 20字节 - 交易发送方地址
* to: DATA, 20字节 - 交易接收方地址，对于合约创建交易，该值为null
* value: QUANTITY - 发送的以太数量，单位：wei
* gasPrice: QUANTITY - 发送方提供的gas价格，单位：wei
* gas: QUANTITY - 发送方提供的gas可用量
* input: DATA - 随交易发送的数据

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'`

响应：
```
{
"id":1,
"jsonrpc":"2.0",
"result": {
    "hash":"0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b",
    "nonce":"0x",
    "blockHash": "0xbeab0aa2411b7ab17f30a99d3cb9c6ef2fc5426d6ad6fd9e2a26a6aed1d1055b",
    "blockNumber": "0x15df", // 5599
    "transactionIndex":  "0x1", // 1
    "from":"0x407d73d8a49eeb85d32cf465507dd71d507100c1",
    "to":"0x85h43d8a49eeb85d32cf465507dd71d507100c1",
    "value":"0x7f110", // 520464
    "gas": "0x7f110", // 520464
    "gasPrice":"0x09184e72a000",
    "input":"0x603880600c6000396000f300603880600c6000396000f3603880600c6000396000f360",
  }
}
```

---

## eth_getTransactionByBlockHashAndIndex
返回指定块内具有指定索引序号的交易。

## 参数
* DATA, 32字节 - 块哈希
* QUANTITY - 交易在块内的索引序号
## 
    params: [
       '0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331',
       '0x0' // 0
    ]

## 返回值
    * 查阅eth_getTransactionByHash的返回值

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockHashAndIndex","params":["0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b", "0x0"],"id":1}'`

返回值:
```
请参考eth_getTransactionByHash的返回值。
```
---

## eth_getTransactionByBlockNumberAndIndex
返回指定编号的块内具有指定索引序号的交易。

## 参数
* QUANTITY|TAG - 整数块编号，或字符串"earliest"、"latest" 或"pending"
* QUANTITY - 交易索引序号
##
    params: [
       '0x29c', // 668
       '0x0' // 0
    ]

## 返回值

    * 请参考eth_getTransactionByHash的返回值。

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockNumberAndIndex","params":["0x29c", "0x0"],"id":1}'`

响应:
```
结果请参考eth_getTransactionByHash调用。
```
---

## eth_getTransactionReceipt

返回指定交易的收据，使用哈希指定交易。

需要指出的是，挂起的交易其收据无效。

## 参数
DATA, 32字节 - 交易哈希

    params: [
       '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
    ]

## 返回值
Object - 交易收据对象，如果收据不存在则为null。交易对象的结构如下：

* transactionHash: DATA, 32字节 - 交易哈希
* transactionIndex: QUANTITY - 交易在块内的索引序号
* blockHash: DATA, 32字节 - 交易所在块的哈希
* blockNumber: QUANTITY - 交易所在块的编号
* from: DATA, 20字节 - 交易发送方地址
* to: DATA, 20字节 - 交易接收方地址，对于合约创建交易该值为null
* cumulativeGasUsed: QUANTITY - 交易所在块消耗的gas总量
* gasUsed: QUANTITY - 该次交易消耗的gas用量
* contractAddress: DATA, 20字节 - 对于合约创建交易，该值为新创建的合约地址，否则为null
* logs: Array - 本次交易生成的日志对象数组
* logsBloom: DATA, 256字节 - bloom过滤器，轻客户端用来快速提取相关日志

返回的结果对象中还包括下面二者之一 :

* root : DATA 32字节，后交易状态根(pre Byzantium)
* status: QUANTITY ，1 (成功) 或 0 (失败)

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionReceipt","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'`

响应：

```
{
"id":1,
"jsonrpc":"2.0",
"result": {
     transactionHash: '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238',
     transactionIndex:  '0x1', // 1
     blockNumber: '0xb', // 11
     blockHash: '0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b',
     cumulativeGasUsed: '0x33bc', // 13244
     gasUsed: '0x4dc', // 1244
     contractAddress: '0xb60e8dd61c5d32be8058bb8eb970870f07233155', // or null, if none was created
     logs: [{
         // logs as returned by getFilterLogs, etc.
     }, ...],
     logsBloom: "0x00...0", // 256 byte bloom filter
     status: '0x1'
  }
}
```
---
#### 账户相关

---
## eth_accounts

返回客户端持有的地址列表。

## 参数

    无

## 返回值

    * 客户端持有的地址字符串列表

## 示例代码

请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}'`

响应：

```
{
  "id":1,
  "jsonrpc": "2.0",
  "result": ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]
}
```
---

## eth_getBalance

返回指定地址账户的余额。

## 参数
DATA - 20字节，要检查余额的地址

QUANTITY|TAG - 整数块编号，或者字符串"latest", "earliest" 或 "pending"

    params: [
    '0x407d73d8a49eeb85d32cf465507dd71d507100c1',
    'latest'
    ]
## 返回值

    * QUANTITY - 当前余额，单位：wei

## 示例代码

请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0x407d73d8a49eeb85d32cf465507dd71d507100c1", "latest"],"id":1}'`

响应：

```
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x0234c8a3397aab58" // 158972490234375000
}
```
---

## eth_getStorageAt

返回指定地址存储位置的值。

## 参数

    DATA - 20字节，存储地址
    QUANTITY - 存储中的位置号
    QUANTITY|TAG - 整数块号，或字符串"latest"、"earliest" 或"pending"

## 返回值

    * DATA - 指定存储位置的值

## 示例代码
根据要提取的存储计算正确的位置。考虑下面的合约，由0x391694e7e0b0cce554cb130d723a9d27458f9298 部署在地址 0x295a70b2de5e3953354a6a8344e616ed314d7251：

```
contract Storage {
    uint pos0;
    mapping(address => uint) pos1;

    function Storage() {
        pos0 = 1234;
        pos1[msg.sender] = 5678;
    }
}
```

提取pos0的值很直接：

`curl -X POST --data '{"jsonrpc":"2.0", "method": "eth_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x0", "latest"], "id": 1}' localhost:8545`

响应结果：

```
{"jsonrpc":"2.0","id":1,"result":"0x00000000000000000000000000000000000000000000000000000000000004d2"}
```

要提取映射表中的成员就难一些了。映射表中成员位置的计算如下：

```
keccack(LeftPad32(key, 0), LeftPad32(map position, 0))
```

这意味着为了提取pos1["0x391694e7e0b0cce554cb130d723a9d27458f9298"]的值，我们需要如下计算：

```
keccak(decodeHex("000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"))
```

geth控制台自带的web3库可以用来进行这个计算：

```
> var key = "000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"
undefined
> web3.sha3(key, {"encoding": "hex"})
"0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9"
```

现在可以提取指定位置的值了：

`curl -X POST --data '{"jsonrpc":"2.0", "method": "eth_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9", "latest"], "id": 1}' localhost:8545`

相应结果如下：

```
{"jsonrpc":"2.0","id":1,"result":"0x000000000000000000000000000000000000000000000000000000000000162e"}
```

---

## eth_getCode
返回指定地址的代码。

## 参数

    DATA - 20字节，地址
    QUANTITY|TAG - 整数块编号，或字符串"latest"、"earliest" 或"pending"
    params: [
       '0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b',
       '0x2'  // 2
    ]

## 返回值

    * DATA - 指定地址处的代码

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getCode","params":["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x2"],"id":1}'`

响应：
```
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"
}
```
---

#### 挖矿相关

---
## eth_coinbase

返回客户端的coinbase地址,如果为0x0则是没设置

## 参数

    无

## 返回值

    * DATA, 20 bytes - 当前coinbase地址

## 示例代码

请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_coinbase","params":[],"id":0}'`

响应：

```
{
  "id":0,
  "jsonrpc": "2.0",
  "result": "0x407d73d8a49eeb85d32cf465507dd71d507100c1"
}
```
---

## eth_mining

如果客户端在积极挖矿则返回true。

## 参数
    无

## 返回值
    * Boolean - 当客户端在挖矿时返回true，否则返回false

## 示例代码

请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_mining","params":[],"id":0}'`

响应：
```
{
  "id":0,
  "jsonrpc": "2.0",
  "result": true
}
```
---
## miner_setEtherBase
miner的setEtherBase方法用来设置接收挖矿奖励的AETH账号。

## 调用方法

Geth控制台：

    miner.setEtherbase(address)
JSON RPC：

    {"method": "miner_setEtherbase", "params": [address]}
---

## miner_start

miner的start方法启动具有指定线程数量的CPU挖矿进程，并根据需要生成新的有向无环图。
## 调用方法
Geth控制台：

    miner.start(number)
JSON RPC：

    {"method": "miner_start", "params": [number]}
---

## miner_stop
miner的stop方法用来停止CPU挖矿操作的执行。

## 调用方法
Geth控制台：

    miner.stop()
JSON RPC：

    {"method": "miner_stop", "params": []}
---
## miner_getNewPlotter
生成PID和脑密码

## 参数
    无

## 返回值
    * passphrase - 脑密码
    * plotterId - PID

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","id":9,"method":"miner_getNewPlotter","params":[]}'`

响应：
```
{
    "id": 9,
    "jsonrpc": "2.0",
    "result": {
        "passphrase": "river least season leaf hospital age advice dude spring bottom define lovely",
        "plotterId": "1464682401897261229"
    }
}
```

---
## eth_bindPID
绑定PID

## 参数

    交易地址
    pid
    绑定地址
    脑密码

## 返回值

    * 交易ID

## 示例代码

请求：

`curl -X POST --data '{"jsonrpc":"2.0","id":9,"method":"eth_bindPID","params":["0x030d91538d3515501e1e45f311e57305786ac8d4","4291418842198847361","0x030d91538d3515501e1e45f311e57305786ac8d6","deadly fairy express twice weird brother underneath straight spell dude practice happen"]}'`

响应：
```
{
  "id":9,
  "jsonrpc": "2.0",
  "result": "0xcfee52326d289d70eeadca136466d37e2c9aa3e8e7f91b90fa8e3adc8b92e4af"
}
```

---
## eth_getRawBindPID
生成PID绑定数据
## 参数

    pid
    绑定地址
    脑密码

## 返回值

    * 绑定数据

## 示例代码
请求：
`curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"eth_getRawBindPID","params":["4291418842198847361","0x030d91538d3515501e1e45f311e57305786ac8d9","deadly fairy express twice weird brother underneath straight spell dude practice happen"]}'`

响应：
```
{
    "id": 0,
    "jsonrpc": "2.0",
    "result": "0xf88201a0931bde1dd80c3ccd2f65e6b686add96e466ebf3b7bc006b158025cd7e31fcf12883b8e2eb337ae2f8194030d91538d3515501e1e45f311e57305786ac8d9b8400200428cf51d5a599c82a03b44754e1b0ae8c90cdc46e0e4418224d4037cfa010aee96c24c3ec0bea82cf027c9c1f062880cd680664ead1c6c2a8fb2f935ed58"
}
```
---
## eth_sendRawBindPID
发送绑定数据绑定PID
## 参数

    绑定地址
    绑定数据

## 返回值

    * 交易ID

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","id":0,"method":"eth_sendRawBindPID","params":["0x030d91538d3515501e1e45f311e57305786ac8d4","0xf8a301a0931bde1dd80c3ccd2f65e6b686add96e466ebf3b7bc006b158025cd7e31fcf129334323931343138383432313938383437333631aa307830333064393135333864333531353530316531653435663331316535373330353738366163386435b84024220ae35df836576629efd39290c4afed65b14aff640f03ed16223dd275e708fd46ec918936acc23077782e1fea1330299824558bb128610008093c89bfacd9"]}'`
响应：
```
{
  "id":9,
  "jsonrpc": "2.0",
  "result": "0xcfee52326d289d70eeadca136466d37e2c9aa3e8e7f91b90fa8e3adc8b92e4af"
}
```
---

## eth_getPIDByAddress
返回是绑定PID的列表
## 参数

    0x030d91538d3515501e1e45f311e57305786ac8d0 - 地址


## 返回值

    * PID

## 示例代码
请求：
`curl -X POST --data '{"jsonrpc":"2.0","id":9,"method":"eth_getPIDByAddress","params":["0x030d91538d3515501e1e45f311e57305786ac8d0"]}'`

响应：
```
{
    "id": 9,
    "jsonrpc": "2.0",
    "result": [
        "4291418842198847361"
    ]
}
```
---

## eth_getAddressByPID
返回是绑定PID的列表
## 参数

    4291418842198847361 - pid


## 返回值

    * 地址

## 示例代码
请求：
`curl -X POST --data '{"jsonrpc":"2.0","id":9,"method":"eth_getAddressByPID","params":["4291418842198847361"]}'`

响应：
```
{
    "id": 9,
    "jsonrpc": "2.0",
    "result": [
        "0x030d91538d3515501e1e45f311e57305786ac8d0"
    ]
}
```

---

#### POC相关

---
## eth_protocolVersion

返回当前AETH协议的版本。

## 参数
    无

## 返回值
    * String - 当前的AETH协议版本

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_protocolVersion","params":[],"id":0}'`

响应：

```
{
  "id":0,
  "jsonrpc": "2.0",
  "result": "54"
}
```
---

## eth_blockNumber
返回最新块的编号。

## 参数

    无

## 返回值

    * QUANTITY - 节点当前块编号

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":83}'`

响应：

```
{
  "id":83,
  "jsonrpc": "2.0",
  "result": "0x4b7" // 1207
}
```
---

## eth_call
立刻执行一个新的消息调用，无需在区块链上创建交易。

## 参数
Object - 交易调用对象

* from: DATA, 20 Bytes - 发送交易的原地址，可选
* to: DATA, 20 Bytes - 交易目标地址
* gas: QUANTITY - 交易可用gas量，可选。eth_call不消耗gas，但是某些执行环节需要这个参数
* gasPrice: QUANTITY - gas价格，可选
* value: QUANTITY - 交易发送的以太数量，可选
* data: DATA - 方法签名和编码参数的哈希，可选
* QUANTITY|TAG - 整数块编号，或字符串"latest"、"earliest"或"pending"

## 返回值

    * DATA - 所执行合约的返回值

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_call","params":[{see above}],"id":1}'`

响应：
```
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x"
}
```
---

## eth_newFilter
基于给定的选项创建一个过滤器对象，接收状态变化时的通知。要检查状态是否变化， 请调用eth_getFilterChanges。

关于特定主题过滤器的说明：主题是顺序相关的。如果一个交易的日志有主题[A, B]，那么将被 以下的主题过滤器匹配：

* [] 任何主题
* [A] 先匹配A主题
* [null, B] 先匹配其他主题，再匹配B主题
* [A, B] 先匹配A主题，再匹配B主题，最后匹配其他主题
* [[A, B], [A, B]] "先匹配A主题或B主题，再匹配A主题或B主题，最后匹配其他主题

## 参数
Object - 过滤器选项对象：

* fromBlock: QUANTITY|TAG - 可选，默认值："latest"。整数块编号，或字符串"latesr"表示最后挖出的块，"pending"或"earliest"用于未挖出的交易。
* toBlock: QUANTITY|TAG - 可选，默认值："latest"。整数块编号，或字符串"latesr"表示最后挖出的块，"pending"或"earliest"用于未挖出的交易。
* address: DATA|Array, 20字节 - 可选，合约地址或生成日志的一组地址
* topics: Array of DATA, - 可选，32字节主题数组，每个主题可以是数组或使用or选项连接
##
    params: [{
      "fromBlock": "0x1",
      "toBlock": "0x2",
      "address": "0x8888f1f195afa192cfee860698584c030f4c9db1",
      "topics": ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b", null, ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x0000000000000000000000000aff3454fce5edbc8cca8697c15331677e6ebccc"]]
    }]

## 返回值

    * QUANTITY - 过滤器编号

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_newFilter","params":[{"topics":["0x12341234"]}],"id":73}'`

响应：

```
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```

---

## eth_newBlockFilter
在节点中创建一个过滤器，以便当新块生成时进行通知。要检查状态是否变化， 请调用eth_getFilterChanges。

## 参数
    无

## 返回值
    * QUANTITY - 过滤器编号

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_newBlockFilter","params":[],"id":73}'`

响应：
```
{
  "id":1,
  "jsonrpc":  "2.0",
  "result": "0x1" // 1
}
```
---

## eth_newPendingTransactionFilter
在节点中创建一个过滤器，以便当产生挂起交易时进行通知。 要检查状态是否发生变化，请调用eth_getFilterChanges。

## 参数
    无

## 返回值

    * QUANTITY - 过滤器编号

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_newPendingTransactionFilter","params":[],"id":73}'`

响应：
```
{
  "id":1,
  "jsonrpc":  "2.0",
  "result": "0x1" // 1
}
```
---

## eth_uninstallFilter
写在具有指定编号的过滤器。当不在需要监听时，总是需要执行该调用。另外，过滤器 如果在一定时间内未接收到eth_getFilterChanges调用会自动超时。

## 参数
QUANTITY - 过滤器编号

    params: [
      "0xb" // 11
    ]

## 返回值
    * Boolean - 如果成功卸载则返回true，否则返回false

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_uninstallFilter","params":["0xb"],"id":73}'`


响应：
```
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```
---


## eth_getFilterChanges
轮询指定的过滤器，并返回自上次轮询之后新生成的日志数组。

## 参数
QUANTITY - 过滤器编号

    params: [
      "0x16" // 22
    ]

## 返回值
Array - 日志对象数组，如果没有新生成的日志，则返回空数组

使用eth_newBlockFilter创建的过滤器将返回块哈希（32字节），例如["0x3454645634534..."]。

使用eth_newPendingTransactionFilter创建的过滤器将返回交易哈希 (32字节)，例如["0x6345343454645..."]。

使用eth_newFilter创建的过滤器，日志对象具有如下参数：

* removed: TAG - 如果日志已被删除则返回true，如果是有效日志则返回false
* logIndex: QUANTITY - 日志在块内的索引序号。对于挂起日志，该值为null
* transactionIndex: QUANTITY - 创建日志的交易索引序号，对于挂起日志，该值为null
* transactionHash: DATA, 32字节 - 创建该日志的交易的哈希。对于挂起日志，该值为null
* blockHash: DATA, 32字节 - 该日志所在块的哈希。对于挂起日志，该值为null
* blockNumber: QUANTITY - 该日志所在块的编号。对于挂起日志，该值为null
* address: DATA, 20字节 - 该日志的源地址
* data: DATA - 包含该日志的一个或多个32字节无索引参数
* topics: Array of DATA -0~4个32字节索引日志参数的数据。在solidity中，第一个主题是事件签名，例如 Deposit(address,bytes32,uint256)，除非你声明的是匿名事件

## 示例代码
请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getFilterChanges","params":["0x16"],"id":73}'`


响应：
```
{
  "id":1,
  "jsonrpc":"2.0",
  "result": [{
    "logIndex": "0x1", // 1
    "blockNumber":"0x1b4", // 436
    "blockHash": "0x8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "transactionHash":  "0xdf829c5a142f1fccd7d8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcf",
    "transactionIndex": "0x0", // 0
    "address": "0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "data":"0x0000000000000000000000000000000000000000000000000000000000000000",
    "topics": ["0x59ebeb90bc63057b6515673c3ecf9438e5058bca0f92585014eced636878c9a5"]
    },{
      ...
    }]
}
```
---
## eth_getFilterLogs
返回指定编号过滤器中的全部日志。

## 参数
QUANTITY - 过滤器编号

    params: [
      "0x16" // 22
    ]

## 返回值
    * 请参阅eth_getFilterChanges。

## 示例代码

请求：

`curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getFilterLogs","params":["0x16"],"id":74}'`

响应:
```
请参阅eth_getFilterChanges。
```
---