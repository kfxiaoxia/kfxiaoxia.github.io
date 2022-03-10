---
layout: post
title: "Swift Subscripts"
date: 2022-03-10 21:48:00 +0800
categories: [article]
tags: [Swift]
published: true
---

在 **Swift** 中， **Dictionary** 可以通过下标的方式取值  **(dic["key"])** , 这种方式是通过 **Subscripts** 实现的
```swift
// https://github.com/apple/swift/blob/0d87a1084d5a864d02310cd3d683b1ba02b4fc17/stdlib/public/core/Dictionary.swift#L783

@inlinable
public subscript(key: Key) -> Value? {
  get {
    return _variant.lookup(key)
  }
  set(newValue) {
    if let x = newValue {
      _variant.setValue(x, forKey: key)
    } else {
      removeValue(forKey: key)
    }
  }
  _modify {
    defer { _fixLifetime(self) }
    yield &_variant[key]
  }
}
```

使用 **Subscripts** 定义一个乘法表：

```swift
struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index
    }
}

let threeTimesTable = TimesTable(multiplier: 3)

print("six times three is \(threeTimesTable[6])")
//six times three is 18
```



使用 **subscript** 实现的 Matrix 结构

```swift
struct Matrix {

    let rows: Int, columns: Int

    var grid: [Double]

    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(repeating: 0.0, count: rows * columns)
    }

    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }

    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}

var matrix = Matrix(rows: 2, columns: 2)
print("\(matrix)")
//Matrix(rows: 2, columns: 2, grid: [0.0, 0.0, 0.0, 0.0])
matrix[0, 1] = 1.5
matrix[1, 0] = 3.2
print(matrix[0 , 0])
//0.0
print(matrix[0 , 1])
//1.5
```

枚举中的使用:

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
    static subscript(n: Int) -> Planet {
        return Planet(rawValue: n)!
    }
}
let mars = Planet[4]
//mars
print(mars)
```
