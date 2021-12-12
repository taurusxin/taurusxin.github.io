---
title: "区块链学习笔记——概念与简单JS实现"
categories: [ "程序人生","开源项目","算法解析" ]
tags: [  ]
draft: false
slug: "blockchain-01"
date: "2020-07-12 16:28:00"
---

在哔哩哔哩上看 [是落拓呀][1] 的区块链简明教程，趁热打铁，把自己学习过程记录下来
本项目的 Github 地址 ↓
[github repo="taurusxin/taurusxincoin" /]

## 区块
区块即是每一次的交易，比如A转账给B十元，那么这条交易就写在这个区块内
区块里面实际包含的是

 - 交易数据
 - 本次交易的 hash
 - 前一次交易的 hash

其中本一次交易的 hash 为 "本次交易的数据 + 上一次交易的hash" 的 hash 值
用简明的伪代码表示就是

```javascript
Block = {
  data = '',
  hash = sha256(data + previousHash),
  previousHash = ''
}
```

## 链
链就是一系列区块的总和，与之相联通的就是 hash 值，每一次的交易都有它的 hash，与链表的作用机制相似
可以理解为一个链表，简明代码表示就是一个数组

```javascript
Chain = {
  blocks[Block]
}
```

## JS实现
这里用 node.js 来实现一个简单的区块链

初始化项目，安装 crypto-js 依赖
```cmd
npm init
npm install crypto-js -S
```

构建 Block 和 Chain 对象
```javascript
class Block {
  constructor(data, previousHash) {
    this.data = data
    this.previousHash = previousHash
    this.hash = this.computeHash()
  }

  computeHash() {
    return sha256(this.data + this.previousHash).toString()
  }
}
```
构造函数传入了这个对象的 `data` `previousHash` 数据，然后定义了对象成员 `hash` ，用于计算当前区块的 hash
```javascript
class Chain {
  constructor() {
    this.chain = [this.bigBang()]
  }

  bigBang() {
    const genisisBlcok = new Block('我是祖先', '')
    return genisisBlcok
  }

  getLatestBlock() {
    return this.chain[this.chain.length - 1]
  }

  addBlockToChain(newBlock) {
    // find the nearest block's hash
    // which is the new block's previous hash
    newBlock.previousHash = this.getLatestBlock().hash
    newBlock.hash = newBlock.computeHash()
    this.chain.push(newBlock)
  }

  // check the previous hash is equal to previous block's hash
  validateChain() {
    if (this.chain.length === 1) {
      if (this.chain[0].hash !== this.chain[0].computeHash()) {
        return false
      }
      return true
    }
    // check if current data has been change
    for (let i = 1; i < this.chain.length; i++) {
      const blockToValidate = this.chain[i]
      if (blockToValidate.hash !== blockToValidate.computeHash()) {
        console.log('data has been change')
        return false
      }

      const previousBlcok = this.chain[i - 1]
      if (blockToValidate.previousHash !== previousBlcok.hash) {
        console.log('previous and current block lost connection')
        return false
      }
    }
    return true
  }
}
```
最初始的 `chain` 只包含一个祖先区块，它没有 `previousHash`，并用 `bigBang` 方法进行初始化，`addBlockToChain` 方法实现了添加一个区块，并自动计算其 hash
`validateChain()` 方法，从三种情况去校验这个链是否合法。
 1. 只有一个祖先区块时，校验祖先区块数据是否被篡改
 2. 从第二个区块开始遍历是否有数据被篡改
 3. 从第二个区块开始遍历是否有hash值被修改

检验代码是否有效
```js
const taurusxinChain = new Chain()

const block1 = new Block('转账十元', '')
taurusxinChain.addBlockToChain(block1)
const block2 = new Block('转账十个十元', '')
taurusxinChain.addBlockToChain(block2)

console.log(taurusxinChain)
console.log(taurusxinChain.validateChain())

// try to change the block data
taurusxinChain.chain[1].data = '转账一百个十元'
console.log(taurusxinChain.validateChain())

// try to change the block hash
taurusxinChain.chain[1].hash = taurusxinChain.chain[1].computeHash()
console.log(taurusxinChain.validateChain())
```

输出结果
```
Chain {
  chain: [
    Block {
      data: '我是祖先',
      previousHash: '',
      hash: 'dbf4cebe91cf0212be8ee04289855fc17e0085d45a7b4c69eeb79e7c636b48fc'
    },
    Block {
      data: '转账十元',
      previousHash: 'dbf4cebe91cf0212be8ee04289855fc17e0085d45a7b4c69eeb79e7c636b48fc',
      hash: 'f135810d2aa85703b2f7356877b95de3dad00955830d570568dd42dd52b93500'
    },
    Block {
      data: '转账十个十元',
      previousHash: 'f135810d2aa85703b2f7356877b95de3dad00955830d570568dd42dd52b93500',
      hash: 'ec3caa66201967755657f30156f38207de23c897caa9aa3ca972f413b9a6a749'
    }
  ]
}
true
data has been change
false
previous and current block lost connection
false
```
第一次生成区块链，校验成功，输出true
尝试更改第二个区块的数据，输出 `data has been change`
尝试更改第二个区块的hash值，输出 `previous and current block lost connection`

## 下一阶段

 - 工作量证明
 - 生成钱包地址
 - 加密

  [1]: https://space.bilibili.com/43276908/