---
layout: post
title: "Swift 多线程 "
date: 2021-06-14 15:24:11 +0800
categories: [article]
tags: [Swift]
published: true
---

<link rel="stylesheet" href="/assets/css/style.css">

#### 术语

同步：`【相对于任务而言】`按顺序执行多个任务，在一个线程里按顺序执行，结束顺序是固定的。

异步：`【相对于任务而言】`按顺序执行多个任务，在多个线程里执行，结束的顺序是随机的。

串行：`【相对于队列而言】`多个任务在串行队列里执行，按顺序执行。

并发：`【相对于队列而言】`多个任务在并行队列里执行，同时执行。

#### GCD

1、串行队列

```swift
let q = DispatchQueue(label: "com.example.1")
```

2、并发队列

```swift
let q = DispatchQueue(label: "com.example.2", attributes: .concurrent)
```

3、主队列

```swift
let main = DispatchQueue.main
```

4、全局队列

```swift
let global = DispatchQueue.global()
```

**队列优先级**

| 名称             | 注释                                                         |
| ---------------- | ------------------------------------------------------------ |
| .userInteractive | 和用户交互，优先级最高。场景：界面绘图                       |
| .userInitiated   | 立即发生。场景：用户点击按钮                                 |
| .default         | 默认                                                         |
| .utility         | 一些计算任务，场景：连续数据处理                             |
| .background      | 后台任务，场景：一些日志，同步服务等                         |
| .unspecified     | 用于兼容旧版 API，因为有些旧版 API 可能会使线程超出 QoS 范围 |

5、添加任务

```swift
q.async {
    /// 异步执行
}
q.sync {
    /// 同步执行
}
```

6、线程死锁

```swift
let q = DispatchQueue(label: "com.example.3", attributes: .concurrent)
q.sync {
    DispatchQueue.main.sync {
    }
}
```

#### DispatchGroup

Group 管理多个队列

执行顺序不确定

```swift
let group = DispatchGroup()
let requestQueue = DispatchQueue(label: "com.example.request", qos: .background)
let calculateQueue = DispatchQueue(label: "com.example.calculate",qos: .utility)
let otherQueue = DispatchQueue(label: "com.example.other", qos: .default)
requestQueue.async(group: group) {
    /// 任务
}
calculateQueue.async(group: group) {
    /// 任务
}
otherQueue.async(group: group) {
    ///
}
// 任务全部结束,在指定队列通知
group.notify(queue: .main) {
    print("all done")
}
```

await() 线程等待，超时时间

```swift
let g = DispatchGroup()
let q = DispatchQueue(label: "com.example.await", attributes: .concurrent)
q.async(group: g) {
    Thread.sleep(forTimeInterval: 5)
}
q.async(group: g) {
    Thread.sleep(forTimeInterval: 2)
}
if g.wait(timeout: .now() + 6) == .success {
    print("sucess")
} else {
    print("timeout")
}
```

enter() 和 leave()
队列里面的任务执行完成之后默认会通知 Group, 但是如果任务中还有异步任务需要手动通知 Group

```swift
let g = DispatchGroup()
let q = DispatchQueue.init(label: "com.ecample.enter_leave")
q.async(group: g) {
    print("1")
    g.enter()
    DispatchQueue.global().asyncAfter(deadline: .now() + 3) {
        print("2")
        g.leave()
    }
    print("3")
}
g.notify(queue: .main) {
    print("done")
}
```

DispatchSemaphore

```swift
let d = DispatchSemaphore(value: 1)
let q = DispatchQueue.global(qos: .background)

q.async {
    d.wait() // 信号量 -1
    DispatchQueue.global().asyncAfter(deadline: .now() + 1) {
        print("1")
        d.signal() // 信号量 +1
    }
}

q.async {
    d.wait()
    DispatchQueue.global().asyncAfter(deadline: .now() + 5) {
        print("2")
        d.signal()
    }
}

q.async {
    d.wait()
    DispatchQueue.global().asyncAfter(deadline: .now() + 1) {
        print("3")
        d.signal()
    }
}
```
