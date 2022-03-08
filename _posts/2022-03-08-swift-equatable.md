---
layout: post
title: "Swift Equatable"
date: 2022-03-08 21:21:00 +0800
categories: [article]
tags: [Swift]
published: true
---

在 Swift 中，自定义类型统一通过  **Equatable** 协议方便的实现 **==** 、**contains** 操作。

这是一个例子，我们有一个 **Mobile** 并且实现了 **Equatable** 协议
```swift
struct Mobile: Equatable {
    let id: String
    let model: String

    static func == (lhs: Self, rhs: Self) -> Bool {
        return lhs.model == rhs.model
    }
}
```

比较是否是同一个型号
```swift
let a = Mobile(id: "123134321432", model: "iPhone 13")
let b = Mobile(id: "464645364325", model: "iPhone 13")
let c = Mobile(id: "432532543543", model: "iPhone 13 Pro Max")

if a == b {
    print("same")
} else {
    print("different")
}
//same

if a == c {
    print("same")
} else {
    print("different")
}
//different
```

是否包含
```swift
let arr = [a, b, c];

if arr.contains(Mobile(id: "3243242342432", model: "iPhone 13 Pro Max")) {
    print("yes")
} else {
    print("no")
}
// yes

if arr.contains(Mobile(id: "3243242342432", model: "iPhone 13 Pro")) {
    print("yes")
} else {
    print("no")
}
// no
```
