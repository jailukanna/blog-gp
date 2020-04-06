---
title: mysql example function%2520test1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'function%2520test1'

Functions in program: 
* `def recur_factorial(n):`

## python function%2520test1

Python mysql example: function%2520test1

```python
def recur_factorial(n):
   """Function to return the factorial
   of a number using recursion"""
   """if n == 1:
       return n
   else:
       return n*recur_factorial(n-1)

# Change this value for a different result
num = input()

# uncomment to take input from the user
#num = int(input("Enter a number: "))

# check is the number is negative
if num < 0:
   print("Sorry, factorial does not exist for negative numbers")
elif num == 0:
   print("The factorial of 0 is 1")
else:
   print("The factorial of",num,"is",recur_factorial(num))"""
   
# Python program to display all the prime numbers within an interval

# change the values of lower and upper for a different result
lower = int(input("lower value:"))
upper = int(input("upper value :"))

# uncomment the following lines to take input from the user
#lower = int(input("Enter lower range: "))
#upper = int(input("Enter upper range: "))

print("Prime numbers between",lower,"and",upper,"are:")

for num in range(lower,upper + 1):
   # prime numbers are greater than 1
   if num > 1:
       for i in range(2,num):
           if (num % i) == 0:
               break
       else:
           print(num)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
