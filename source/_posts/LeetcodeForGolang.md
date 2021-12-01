---
title: Leetcode For Golang
date: 2021-12-01 20:42:43
categories:
- Go
tags:
- Go
---

 在此记录Leetcode刷题之路！

<!--more-->

### 两数之和

#### 题目描述

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

示例 1：

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

示例 2：

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

示例 3：

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

#### 思路

题目关键：

1. 每种输入对应一个答案（答案唯一）。
2. 数组中同一个数字在答案中不能重复出现（不能出现3+3=6这种情况）。

#### 解题

**方法一：暴力枚举**

枚举数组中的每一个数`x`，寻找数组中是否存在`target-x`。

但是时间复杂度为$$ O(N^2) $$



```go
func twoSum(nums []int, target int) []int {
    for i, x := range nums {
        for j := i + 1; j < len(nums); j++ {
            if x+nums[j] == target {
                return []int{i, j}
            }
        }
    }
    return nil
}
```

**方法二：利用map**

时间复杂度为*O*(*N*)。

扫描数组，对于每一个元素，在`map`中寻找能够相加得到给定值的另一半数字，找到了则返回两个数字的下标，没找到就把这个数字存入`map`，等扫描到“另一半”数字时再取出下标。

```go
// TwoSum函数返回数组中相加等于目标值的值的下标——map方法
func twoSum(nums []int, target int) []int {
	// map[键类型]int{}-值类型
	hashTable := make(map[int]int)
	for i := 0; i < len(nums); i++{
		anotherSum := target - nums[i]
		if _, ok := hashTable[anotherSum]; ok{
			return []int{hashTable[anotherSum], i}
		}
		hashTable[nums[i]] = i
	}
	return nil
}
```

