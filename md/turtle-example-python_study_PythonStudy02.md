---
title: turtle example python study PythonStudy02 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'python study PythonStudy02'


## python python study PythonStudy02

Python turtle example: python study PythonStudy02

```python
#序列
'''
    1、元组
        a、不可修改
    2、列表
        a、可修改
        基础列表操作：
            1、根据索引修改列表
            2、删除列表元素 del
            3、切边赋值，可以同时给多个元素赋值，通过切片赋值，可以将切片替换为与其长度不同的序列
    序列索引：
        从左向右，0-n
        从右向左，-n-0
    截取：
        [start:end:step] 包前不包后[开始索引:结束索引:步长（默认为1）]
        截取始于开头可简写为 [:end]
        截取始于末尾可简写为 [start:]
        复制序列 [:]
        步长不可为0，可为负数，从右向左
    序列相加
        使用加法拼接序列
    序列相乘
        序列 * x 将重复x次的序列拼接为一个序列
    创建长度为10的空列表
        [None] * 10
    检查特定的值是否在序列中
        特定值 in 序列
    内置函数：
        len() 序列长度
        max() 序列最大值
        min() 序列最小值
        list(字符串) 将字符串转换为对应列表
'''
str = 'Hello'
str2 = ['H', 'e', 'l', 'l', 'o']
number = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
num = [1, 3, 5, 7, 9]
'''
列表切片
print(str[1])
print(str[-1])
print(str[1:4])
print(str[:3])
print(str[4:])
print(number[::2])
'''
'''
创建长度为10的空列表
sequence = [None] * 10
print(sequence)
'''
'''
序列相加
print(str2 + number)
'''
'''
序列相乘
num = num * 4
print(num)
'''
'''
判断元素是否在序列中
print(2 in num)
'''
'''
内置函数
print(min(str2))
print(max(str2))
print(max(str))
print(len(str))
'''
'''
将字符串转换为列表
将列表转换为字符串
print(list(str))
print(''.join(list(str)))
'''
'''删除列表中的某个元素
del str2[1]
print(str2)


'''





```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
