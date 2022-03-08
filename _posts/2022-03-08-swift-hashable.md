---
layout: post
title: "Hashable"
date: 2022-03-08 14:33:11 +0800
categories: [article]
tags: [Swift]
published: true
---

在 Swift 中可以使用自定义的类型作为字典的 key , 单需要遵循 Hashable 协议，Hashable

这是一个例子：

```swift
struct Person: Hashable {
    let name: String
    let id: String

    func hash(into hasher: inout Hasher) {
        hasher.combine(id)
    }
}

struct Car {
    let brand: String
    let model: String
}

struct House {
    let location: String
    let price: Float
}

let cars: [Person: Car] = [
    Person(name: "Tom", id: "123"): Car(brand: "BMW", model: "3")
]

print(cars[Person(name: "Tom", id: "123")])

print(cars[Person(name: "Tom", id: "123x")])

```

**hash(into:)** 方法可以根据 **id** 生成一个唯一的 hash

```swift
func hash(into hasher: inout Hasher) {
    hasher.combine(id)
}
```
