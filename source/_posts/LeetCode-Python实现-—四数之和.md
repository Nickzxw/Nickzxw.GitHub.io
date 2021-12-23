---
title: LeetCode(Python实现)—四数之和
date: 2021-12-23 20:56:59
categories:
- Python
- LeetCode
tags:
- Python
- LeetCode
---

给你一个由 `n` 个整数组成的数组 `nums` ，和一个目标值 `target` 。请你找出并返回满足下述全部条件且不重复的四元组 `[nums[a], nums[b], nums[c], nums[d]] `（若两个四元组元素一一对应，则认为两个四元组重复）

<!--more-->

### 题目描述

给你一个由 `n` 个整数组成的数组 `nums` ，和一个目标值 `target` 。请你找出并返回满足下述全部条件且不重复的四元组 `[nums[a], nums[b], nums[c], nums[d]] `（若两个四元组元素一一对应，则认为两个四元组重复）:

 - 0 <= a, b, c, d < n
 - a、b、c 和 d 互不相同
 -  nums[a] + nums[b] + nums[c] + nums[d] == target

你可以按 **任意顺序** 返回答案 。

**示例 1：**
```
输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```
**示例 2：**
```
输入：nums = [2,2,2,2,2], target = 8
输出：[[2,2,2,2]]
```
### 题目大意
给定一个数组，要求在这个数组中找出 4 个数之和为 0 的所有组合。
### 解题思路
解题方法主要是：排序 + 双指针，先对给定列表进行升序排序，然后对于前两个数字采取枚举的方式计算，后面两个数字使用双指针进行运算，然后对运算过程中一些无效计算进行剪枝，减少算法使用时间。
#### 代码
```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        if not nums or len(nums) < 4:
            return list()
        nums.sort()
        n = len(nums)
        res = list()
        # 第一个数字
        for i in range(n - 3):
            # 若当前数字等于上一个数字则 continue
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            # 若此时四个数加起来大于目标值，则后面的结果全部大于目标值
            if nums[i] + nums[i + 1] + nums[i + 2] + nums[i + 3] > target:
                break
            # 第二个数字
            for j in range(i + 1, n - 2):
                # 若当前数字等于上一个数字则 continue
                if j > i + 1 and nums[j] == nums[j - 1]:
                    continue
                # 若此时四个数加起来大于目标值，则后面的结果全部大于目标值
                if nums[i] + nums[j] + nums[j + 1] + nums[j + 2] > target:
                    break
                # 左指针，右指针
                left, right = j + 1, n - 1
                while left < right:
                    summary = nums[i] + nums[j] + nums[left] + nums[right] 
                    if summary == target:
                        res.append([nums[i], nums[j], nums[left], nums[right]])
                        # 检查后一个数是否和当前数重复
                        while left < right and nums[left] == nums[left + 1]:
                            # while 结束后 left 指向重复值
                            left += 1
                        # 此时 left 指向不重复值
                        left += 1
                        while left < right and nums[right] == nums[right - 1]:
                            # while 结束后 right 指向重复值
                            right -= 1
                        # 此时 right 指向不重复值
                        right -= 1
                    elif summary < target:
                        left += 1
                    else:
                        right -= 1
        return res
```
