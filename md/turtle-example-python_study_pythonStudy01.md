---
title: turtle example python study pythonStudy01 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'python study pythonStudy01'


## python python study pythonStudy01

Python turtle example: python study pythonStudy01

```python
# -*- coding: utf-8 -*-
#长字符串,跨多行字符串，也可使用三个双引号
print('''123
456
789''')
#原始字符串 字符串前用r修饰,使转义字符失效，用于正则表达式
a = r'hello \nworld'
print(a)
#字符的Unicode名称（\N{name}）
'''print("This is a cat: \N{Cat}")'''
#文件读写使用bytes(不可变)、bytearray(可变)
'''
bytes对象，使用前缀b
bytearray对象，对用户不友好，进行操作时要使用ord()获取序数值，0-255
'''
bytesObject = b'hello world'
print(bytesObject)
bytearrayObject = bytearray(bytesObject)
print(bytearrayObject)

'''
本章小结
    1、abs(numbet),返回绝对值
    2、bytes(string,encoding[, errors])对指定字符串进行编码，并以指定的方式处理错误
    3、cmath.sqrt()返回平方根，可用于复数
    4、float()将字符串或数字转换为浮点型
    5、input(prompt)以字符串的方式获取用户输入
    6、int(object) 将字符串或数转换为整数
    7、math.ceil(number)以浮点数方式返回向上圆整结果
    8、math.floor(number)以浮点数方式返回向下圆整结果
    9、math.sqrt(number)开平方
    10、pow(x,y[,z])返回x的y次方对z求模的结果
    11、repr(object) 返回指定值的字符串表示
    12、round(number[, ndigits]) 四舍五入为制定的精度，为5时舍入到偶数
    13、str(object) 将指定的值转换为字符串，用于转换bytes时可以指定编码方式和错误处理方式
'''
print(repr('A'))
A = input()
print(A + 'a')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
