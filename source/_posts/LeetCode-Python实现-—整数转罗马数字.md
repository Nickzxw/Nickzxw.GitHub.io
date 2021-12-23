---
title: LeetCode(Python实现)—整数转罗马数字
date: 2021-12-23 20:56:26
categories:
- Python
- LeetCode
tags:
- Python
- LeetCode
---

## 题目大意
罗马数字包含以下七种字符： I， V， X， L，C，D 和 M。
```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做  `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

 - I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
 - X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。
 - C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。

**条件**：
```
给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。
```

## 解题思路
因为给出的数字范围并不算很大，所以最直观的方法就是将每一位数字可能出现的情况都罗列出来，然后通过列表输出个位、十位、百位、千位数字对应的罗马数字。
### 代码

```python
class Solution:
    # 硬解码，将所有可能罗列
    VALUE_ONES = ["", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"]
    VALUE_TENS = ["", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"]
    VALUE_HUNDREDS = ["", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"]
    VALUE_THOUSANDS = ["", "M", "MM", "MMM"]

    def intToRoman(self, num: int) -> str:
        # 逐位取整，返回罗马数字结果
        return Solution.VALUE_THOUSANDS[num // 1000] + \
            Solution.VALUE_HUNDREDS[num % 1000 // 100] + \
            Solution.VALUE_TENS[num % 100 // 10] + \
            Solution.VALUE_ONES[num % 10]
```
