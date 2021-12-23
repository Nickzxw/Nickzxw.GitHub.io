---
title: LeetCode(Python实现)—最接近的三数之和
date: 2021-12-23 20:56:38
categories:
- Python
- LeetCode
tags:
- Python
- LeetCode
---

给你一个长度为 `n` 的整数数组 `nums` 和 一个目标值 `target`。请你从 `nums` 中选出三个整数，使它们的和与 `target` 最接近。

返回这三个数的和。

假定每组输入只存在恰好一个解。

<!--more-->

## 题目描述

给你一个长度为 `n` 的整数数组 `nums` 和 一个目标值 `target`。请你从 `nums` 中选出三个整数，使它们的和与 `target` 最接近。

返回这三个数的和。

假定每组输入只存在恰好一个解。


**示例 1：**
```
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
示例 2：

输入：nums = [0,0,0], target = 1
输出：0
```

**提示：**
```
3 <= nums.length <= 1000
-1000 <= nums[i] <= 1000
-104 <= target <= 104
```

### 题目大意
给定一个数组，要求在这个数组中找出 3 个数之和离 target 最近，返回这三个数的和。
### 解题思路
这一题和15题很像，都用到了双指针。这题的主要思路是通过对列表排序，然后通过指针夹逼得方法得到与目标差值最小的值，需要注意的是列表中可能会存在多个重复的值，所有要对重复值进行判断，否则会有很多冗余的循环操作。

```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        # 用一个很大的值减去target来比较
        best = 10 ** 5
        # 列表排序
        nums.sort()
        n = len(nums)

        # update
        def update(cur):
            nonlocal best
            if abs(cur - target) < abs(best - target):
                best = cur
                
        # 因为是三个数所有可以只循环到 n - 2
        for i in range(n - 2):
            # 保证与上一次取得的值不等
            if i > 0 and nums[i] ==[i - 1]:
                continue
            left, right = i + 1, n - 1
            while left < right:
                res = nums[i] + nums[left] + nums[right]
                if res == target:
                    return target
                # 更新离target最近的结果
                update(res)
                if res < target:
                    leftSym = left + 1
                    # 保证与上一次取得的值不等
                    while leftSym < right and nums[left] == nums[leftSym]:
                        leftSym += 1
                    left = leftSym
                else:
                    rightSym = right - 1
                    # 保证与上一次取得的值不等
                    while rightSym > left and nums[right] == nums[rightSym]:
                        rightSym -= 1
                    right = rightSym
        return best
```
