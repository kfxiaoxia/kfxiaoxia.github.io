---
layout: post
title: "Smart Contract Introduction"
date: 2022-03-11 21:44:00 +0800
categories: [article]
tags: [Smart Contract]
published: true
---

#### 变量
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;
contract Demo {
    uint number = 12;
    uint256 public count;
    string name = "";
}
```

#### 结构体
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;
contract Demo {
    // 定义结构体
    struct Person {
        uint age;
        string name;
    }
    Person public p;
    constructor() {
        // 使用结构体
         p = Person(18,"Jobs");
    }
}
```

#### 枚举
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;
contract Demo {
    // 定义枚举
    enum Gender{
        Male, Female
    }
    struct Person {
        uint age;
        string name;
        // 性别，枚举
        Gender gender;
    }
    Person public p;
    constructor() {
        // 使用枚举
         p = Person(18,"Jobs", Gender.Female);
    }

}
```

#### 函数
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;
contract Demo {
    enum Gender{
        Male, Female
    }
    struct Person {
        uint age;
        string name;
        Gender gender;
    }
    Person public p;
    constructor() {
         p = Person(18,"Jobs", Gender.Female);
    }

    // memory 表示是临时 对应的还有 storage 标识存储  public 可以在外部调用 returns 返回值
    function update(string memory _name, uint  _age) public returns(bool) {
        p.name = _name;
        p.age = _age;
    }
}
// 外部函数  pure 表示不得修改变量（以后的文章会细讲）
function isOlder(uint age) pure returns (bool) {
    return age >= 60;
}
```

#### 函数修饰符
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract Demo {
    address public owner;
    // 只有部署的人员有权限修改
    modifier onlyOwner() {
        require(msg.sender == owner, "Only seller can call this.");
        // _; 这个必须要，标识是修饰符
        _;
    }
    constructor() {
        owner = msg.sender;
    }
    function blockList() public view onlyOwner {
        //....
    }
}
```