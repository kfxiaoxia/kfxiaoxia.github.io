---
layout: post
title: "Smart Contract Array"
date: 2022-03-12 11:09:00 +0800
categories: [web3]
tags: [Smart Contract]
published: true
---

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;
contract Demo {
    // 声明数组 类型[] 权限 名字
    uint[] public  nums;
    constructor() {
        // 初始化
        nums = [1, 2, 3];
    }
    // 添加
    function push( uint el) public {
        nums.push(el);
    }
    // 读取
    function get(uint256 index) public view returns(uint) {
       return nums[index];
    }
    // 修改
    function update(uint256 index, uint256 el) public {
        nums[index] = el;
    }
    // 删除
    function deleteEl(uint256 index) public {
        delete nums[index];
    }
    // 遍历
    function forEach() public view{
        for(uint i = 0; i < nums.length; i++) {
            nums[i];
        }
    }
    // 获取所有
    function all()  public view returns(uint[] memory) {
        return nums;
    }
}
```