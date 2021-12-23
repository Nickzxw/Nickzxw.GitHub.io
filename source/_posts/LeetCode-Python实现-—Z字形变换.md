---
title: LeetCode(Python实现)—Z字形变换
date: 2021-12-23 20:55:44
categories:
- Python
- LeetCode
tags:
- Python
- LeetCode
---

## 题目描述
将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 `"PAYPALISHIRING"` 行数为 `3` 时，排列如下：
```
P   A   H   N
A P L S I I G
Y   I   R
```
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"PAHNAPLSIIGYIR"`。

**示例 1：**
```
输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
```
**示例 2：**
```
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
```
**示例 3：**
```
输入：s = "A", numRows = 1
输出："A"
```


提示：

    1 <= s.length <= 1000
    s 由英文字母（小写和大写）、',' 和 '.' 组成
    1 <= numRows <= 1000
## 题目大意
将一个给定字符串 `s` 根据给定的行数 `numRows` ，以从上往下、从左到右进行 Z 字形排列(实际上我觉得更像N字形排列)。

比如输入字符串为 `"PAYPALISHIRING"` 行数为 3 时，排列如下
```
P   A   H   N
A P L S I I G
Y   I   R
```
## 解题思路
这题有种找规律的意思，同时也看你对程序控制能力的掌握，遍历顺序实际上是从上往下，然后再从下忘上，以此类推。因此我们只需要设定一个标签`flag`在开始的时候能够使下标递增，在抵达`numRows-1`时使下标再递减，重复步骤即可。
## 代码

```python
def convert(self, s: str, numRows: int) -> str:
        if numRows <  2:
            return s
        res = ["" for _ in range(numRows)]
        i = 0
        flag = -1
        for character in s:
            res[i] += character
            if i == 0 or i == numRows - 1: flag = -flag
            i += flag 
        return "".join(res)
```
