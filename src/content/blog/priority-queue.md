---
title: "⚡ 优先级队列（堆！堆！堆！）"
description: "本文将系统性地介绍优先级队列（Priority Queue）就是这样一个不讲武德的数据结构，它不遵循FIFO（先进先出），而是谁优先级高谁先出。"
pubDate: "2025-08-12"
tags: ["数据结构", "算法", "C++"]
---

# 优先级队列

优先级队列（Priority Queue）是一个非常重要的数据结构。

## 什么是优先级队列？

不同于普通的队列（Queue）遵循先进先出（FIFO）的原则，优先级队列中的每个元素都有一个“优先级”。在出队的时候，总是优先级最高的元素先出队。

## 底层实现：堆（Heap）

优先级队列通常使用**堆**来实现，特别是二叉堆。

```cpp
#include <iostream>
#include <queue>

int main() {
    // 默认是大顶堆
    std::priority_queue<int> pq;
    
    pq.push(10);
    pq.push(30);
    pq.push(20);
    
    while(!pq.empty()) {
        std::cout << pq.top() << " "; // 输出: 30 20 10
        pq.pop();
    }
    
    return 0;
}
```

## 为什么使用堆？

因为堆在插入和删除最大（或最小）元素时的时间复杂度都是 O(log N)，非常高效。
