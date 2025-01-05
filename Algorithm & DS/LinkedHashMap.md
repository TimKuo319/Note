---
tags:
  - DataStructrue
---

## What is `LinkedHashMap`, why we need it

LinkedHashMap 是一種確保 map 的 entry 有順序的資料結構，常用在 LRU Cache 上，主要透過 doubly-ended linked list 來確保 entry 的順序。再搭配 map 來讓 put 和 get 的時間複雜度維持在 O(1)

## Reference

- [LRU Cache - LeetCode](https://leetcode.com/problems/lru-cache/)
- [【深入Java基础】LinkedHashMap的特点与原理-CSDN博客](https://blog.csdn.net/wxgxgp/article/details/79248921)