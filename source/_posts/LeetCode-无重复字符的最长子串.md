---
title: LeetCode-无重复字符的最长子串
date: 2021-12-23 20:55:17
categories:
- Python
- LeetCode
tags:
- Python
- LeetCode
---

### 3.无重复字符的最长字串
#### 题目大意
在一个字符串重寻找没有重复字母的最长子串。

示例 1:
```bash
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```
示例 2:
```bash
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```
示例 3:
```bash
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```
 示例 4:
 ```bash
输入: s = ""
输出: 0
 ```
#### 解题思路
官方使用的方法为滑动窗口，但是其解法有比较多的无效循环，通过Python的字典实现哈希map可以避免这些无效循环。
```python
def lengthOfLongestSubstring(self, s: str) -> int:
        z, res = -1, 0
        dictStr = {}
        for i, x in enumerate(s):
            # 字符已在字典中且下标比字典中的字符下标大
            # 修改字典中的字符下标
            if x in dictStr and z < dictStr[x]:
                z = dictStr[x]
                dictStr[x] = i
            # 字符不在字典中
            # 加入字典，结果为当前枚举下标减去字典中出现重复的字符下标
            else:
                dictStr[x] = i
                res = max(res, i-z)
        return res
```
