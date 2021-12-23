---
title: LeetCode(Python实现)—字符串转换整数 (atoi)
date: 2021-12-23 20:55:58
categories:
- Python
- LeetCode
tags:
- Python
- LeetCode
---

## 题目描述
请你来实现一个 `myAtoi(string s)` 函数，使其能将字符串转换成一个 32 位有符号整数（类似 C/C++ 中的 `atoi` 函数）。

函数 `myAtoi(string s)` 的算法如下：

 - 读入字符串并丢弃无用的前导空格
 - 检查下一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。 如果两者都不存在，则假定结果为正。
 - 读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
 - 将前面步骤读入的这些数字转换为整数（即，`"123"` -> 123， `"0032" `-> 32）。如果没有读入数字，则整数为 0 。必要时更改符号（从步骤 2 开始）。
 - 如果整数数超过 32 位有符号整数范围 [$−2^{31}$,  $2^{31} − 1$] ，需要截断这个整数，使其保持在这个范围内。具体来说，小于 $−2^{31}$ 的整数应该被固定为 $−2^{31}$ ，大于 $2^{31} − 1$ 的整数应该被固定为 $2^{31} − 1$ 。
 - 返回整数作为最终结果。

**注意：**

 - 本题中的空白字符只包括空格字符 ' ' 。
 - 除前导空格或数字后的其余字符串外，请勿忽略 任何其他字符。

**示例 1：**
```
输入：s = "42"
输出：42
解释：加粗的字符串为已经读入的字符，插入符号是当前读取的字符。
第 1 步："42"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："42"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："42"（读入 "42"）
           ^
解析得到整数 42 。
由于 "42" 在范围 [-2**31, 2**31 - 1] 内，最终结果为 42 。
```
**示例 2：**
```
输入：s = "   -42"
输出：-42
解释：
第 1 步："   -42"（读入前导空格，但忽视掉）
            ^
第 2 步："   -42"（读入 '-' 字符，所以结果应该是负数）
             ^
第 3 步："   -42"（读入 "42"）
               ^
解析得到整数 -42 。
由于 "-42" 在范围 [-2**31, 2**31 - 1] 内，最终结果为 -42 。
```
**示例 3：**
```
输入：s = "4193 with words"
输出：4193
解释：
第 1 步："4193 with words"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："4193 with words"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："4193 with words"（读入 "4193"；由于下一个字符不是一个数字，所以读入停止）
             ^
解析得到整数 4193 。
由于 "4193" 在范围 [-2**31, 2**31 - 1] 内，最终结果为 4193 。
```
**示例 4：**
```
输入：s = "words and 987"
输出：0
解释：
第 1 步："words and 987"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："words and 987"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："words and 987"（由于当前字符 'w' 不是一个数字，所以读入停止）
         ^
解析得到整数 0 ，因为没有读入任何数字。
由于 0 在范围 [-2**31, 2**31 - 1] 内，最终结果为 0 。
```
**示例 5：**
```
输入：s = "-91283472332"
输出：-2147483648
解释：
第 1 步："-91283472332"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："-91283472332"（读入 '-' 字符，所以结果应该是负数）
          ^
第 3 步："-91283472332"（读入 "91283472332"）
                     ^
解析得到整数 -91283472332 。
由于 -91283472332 小于范围 [-2**31, 2**31 - 1] 的下界，最终结果被截断为 -2**31 = -2147483648 。
```

**提示：**
```
0 <= s.length <= 200
s 由英文字母（大写和小写）、数字（0-9）、' '、'+'、'-' 和 '.' 组成
```
## 解题思路
### 自动机
根据题意构建如下的自动机，在每个时刻有一个状态 `status`，每次从序列中输入一个字符` s`，并根据字符 `s `转移到下一个状态 `status'`。这样，我们只需要建立一个覆盖所有情况的从 `status` 与 `s` 映射到 `status'` 的表格即可解决题目中的问题。

|                | ""    | +/-    | number     | other |
| -------------- | ----- | ------ | ---------- | ----- |
| **start**      | start | signed | int_number | end   |
| **signed**     | end   | end    | int_number | end   |
| **int_number** | end   | end    | int_number | end   |
| **end**        | end   | end    | end        | end   |

### 代码

```python
INT_MAX = 2 ** 31 - 1
INT_MIN = -2 ** 31


class Automation:
    def __init__(self):
        # 初始状态
        self.state = 'start'
        # 初始符号
        self.sign = 1
        # 初始答案
        self.ans = 0
        # 状态表
        self.table = {
            'start': ['start', 'signed', 'int_number', 'end'],
            'signed': ['end', 'end', 'int_number', 'end'],
            'int_number': ['end', 'end', 'int_number', 'end'],
            'end': ['end', 'end', 'end', 'end']
        }

    def get_col(self, c: str) -> int:
        # 通过字符的类型返回相应的状态
        if c.isspace():
            return 0
        if c == '+' or c == '-':
            return 1
        if c.isdigit():
            return 2
        return 3

    def get(self, c: str):
        # 初始状态为start
        self.state = self.table[self.state][self.get_col(c)]
        # 如果状态为 int_number，诸位计算 ans，然后与上下界进行比较
        if self.state == 'int_number':
            self.ans = self.ans * 10 + int(c)
            self.ans = min(self.ans, INT_MAX) if self.sign == 1 else min(self.ans, -INT_MIN)
        # 如果状态为signed，修正符号
        elif self.state == 'signed':
            self.sign = 1 if c == '+' else -1
            
            
class Solution:
    def myAtoi(self, strs: str) -> int:
        automaton = Automation()
        for s in strs:
            automaton.get(s)
        # 所有的 end 都不会对 ans 进行操作，所以如果状态为 end，结果会为0
        return automaton.sign * automaton.ans
```
### 更简单易懂的方法
虽然自动机也挺好理解的，但是在代码上还是不简单的，第二种方法可以直接先去掉字符串左边的空格，然后使用多个 if...elif...else 进行判断，在代码上更好理解但是会略显臃肿。
#### 代码

```python
INT_MAX = 2 ** 31 - 1
INT_MIN = -2 ** 31


class Solution:
    def myAtoi(self, s: str) -> int:
        # 去除字符串左边的空格
        s = s.lstrip()
        res = 0
        # 标记是否有数字被选取
        symNum = False
        # 标记是否有符号被选取
        flagSign = False
        # 答案符号
        sign = 1
        for i in s:
            if i.isdigit():
                res = res * 10 + int(i)
                symNum = True
            elif i == '+' or i == '-':
                # 如果已经选取过数字，则直接跳出循环
                if symNum:
                    break
                # 如果已经选取过符号则直接输出0
                if flagSign:
                    return 0
                # 符号判断
                if i == '+':
                    sign = 1
                if i == '-':
                    sign = -1
                flagSign = True
            else:
                break
        if res == 0:
            return 0
        else:
            if sign * res > INT_MAX:
                return INT_MAX
            elif sign * res < INT_MIN:
                return INT_MIN
            else:
                return sign * res

```
