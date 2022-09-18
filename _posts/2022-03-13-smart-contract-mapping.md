---
layout: post
title: "Smart Contract Mapping"
date: 2022-03-12 11:09:00 +0800
categories: [web3]
tags: [Smart Contract]
published: true
---

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract Demo {
    // 余额
    mapping(address => uint)  balances;
    // 更新
    function setBalance(uint _balance) public {
        balances[msg.sender] =  _balance;
    }
    // 读取
    function getBalance() public view  returns(uint){
        return balances[msg.sender];
    }
}
```