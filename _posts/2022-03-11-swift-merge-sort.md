---
layout: post
title: "Merge Sort"
date: 2022-03-11 21:17:00 +0800
categories: [article]
tags: [Swift]
published: true
---

<div align=center>
<img style="height:500px;"  src='../../../../assets/images/ms.jpg'>
</div>

<br>


```swift
func mergeSort(_ array: [Int]) -> [Int] {
    guard array.count > 1 else {
        return array
    }
    let middle = array.count / 2

    let left = mergeSort(Array(array[0..<middle]))
    let right = mergeSort(Array(array[middle..<array.count]))
    return merge(left, right)
}


func merge(_ left: [Int], _ right: [Int]) -> [Int] {
    var leftIndex = 0
    var rightIndex = 0
    var res: [Int] = []
    res.reserveCapacity(left.count + right.count)
    while leftIndex < left.count && rightIndex < right.count {
        if left[leftIndex] < right[rightIndex] {
            res.append(left[leftIndex])
            leftIndex += 1
        } else if left[leftIndex] > right[rightIndex] {
            res.append(right[rightIndex])
            rightIndex += 1
        } else {
            res.append(left[leftIndex])
            res.append(right[rightIndex])
            leftIndex += 1
            rightIndex += 1
        }
    }

    while leftIndex < left.count {
        res.append(left[leftIndex])
        leftIndex += 1
    }

    while rightIndex < right.count {
        res.append(right[rightIndex])
        rightIndex += 1
    }

    return res
}
```

使用
```swift
let arr = [12, 33, 11, 34, 5, 24, 34, 25, 45, 4, 66, 56, 55]
let sorted = mergeSort(arr)
print(sorted)
//[4, 5, 11, 12, 24, 25, 33, 34, 34, 45, 55, 56, 66]
```