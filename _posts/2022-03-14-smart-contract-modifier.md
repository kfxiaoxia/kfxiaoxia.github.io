---
layout: post
title: "Smart Contract Modifier"
date: 2022-03-14 21:43:00 +0800
categories: [web3]
tags: [Smart Contract]
published: true
---

modifier可以改变函数的行为。通常用来做权限检查，条件显示等等

定义
```solidity
modifier isOwner {
    require(msg.sender == owner, "No permission");
    _;
}
```

使用

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract Demo {
    address owner;
    string public name;
    // 定义
    modifier isOwner {
        require(msg.sender == owner, "No permission");
        // 表示结尾
        _;
    }

    constructor() {
        name = "unknow";
        owner = msg.sender;
    }

    // 只有 owner 才能改变
    function updateUserName(string memory _name) public isOwner {
        name = _name;
    }
}
```