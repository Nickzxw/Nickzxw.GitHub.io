---
title: LeetCode(Python实现)—最长回文子串
date: 2021-12-23 20:55:31
categories:
- Python
- LeetCode
tags:
- Python
- LeetCode
---

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

<!--more-->

### 题目大意
给你一个字符串 `s`，找到 `s` 中最长的回文子串。

**Example 1:**
```
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```
**Example 2:**
```
Input: s = "cbbd"
Output: "bb"
```
**Example 3:**
```
Input: s = "a"
Output: "a"
```
**Example 4:**
```
Input: s = "ac"
Output: "a"
```
**Constraints:**
```
1 <= s.length <= 1000
`s` consist of only digits and English letters (lower-case and/or upper-case)
```

### 解题思路
这道题有多种解法，其中前两种比较基础且常见，马拉车算法较为复杂，这篇文章不进行详细介绍，有兴趣的朋友可以点击链接深入了解

 1. 动态规划法：定义 `dp[i][j]` 表示从字符串第 `i` 个字符到第 `j` 个字符这一段子串是否是回文串。由回文串的性质可以得知，回文串去掉一头一尾相同的字符以后，剩下的还是回文串。所以状态转移方程是 `dp[i][j] = (s[i] == s[j]) && ((j-i < 3) || dp[i+1][j-1])`。特殊情况：`j - i == 1` 的时候，即只有 2 个字符的情况，只需要判断 2 个字符是否相同即可；此外`j - i == 2` 的时候，即只有 3 个字符的情况，只需要判断除去中心以外对称的 2 个字符是否相等。每次循环动态维护保存最长回文串即可。时间复杂度 $O(n^2)$，空间复杂度 $O(n^2)$。 
 2. 中心扩散法：判断回文有一个核心问题是找到“轴心”。如果长度是偶数，那么轴心是中心虚拟的，如果长度是奇数，那么轴心正好是正中心的那个字母。中心扩散法的核心思想是通过枚举每个轴心的位置之后做两次假设：1、假设最长回文串是偶数，那么以虚拟中心往 2 边扩散；2、假设最长回文串是奇数，那么以正中心的字符往 2 边扩散。扩散的过程就是判断对称中心两边字符是否相等的过程。这个方法时间复杂度和动态规划是一样的，但是空间复杂度降低了。时间复杂度 O(n^2)，空间复杂度 O(1)。
 3.  [马拉车算法](https://books.halfrost.com/leetcode/ChapterFour/0001~0099/0005.Longest-Palindromic-Substring/)
#### 代码

```python
# 中心扩展法
def expendAroundCenter(self, s, left, right):
    while left >= 0 and right < len(s) and s[left] == s[right]:
        # 由中心向外扩散
        left -= 1
        right += 1
    # 返回扩散的最远下标
    return left + 1, right - 1


def longestPalindrome(self, s: str) -> str:
    start, end = 0, 0
    for i in range(len(s)):
        # 奇数回文串
        left1, right1 = self.expendAroundCenter(s, i, i)
        # 偶数回文串
        left2, right2 = self.expendAroundCenter(s, i, i + 1)
        # 修改下标
        if right1 - left1 > end - start:
            start, end = left1, right1
        if right2 - left2 > end - start:
            start, end = left2, right2
    return s[start: end + 1]
```
