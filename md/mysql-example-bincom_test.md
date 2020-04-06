---
title: mysql example bincom test (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'bincom test'

Functions in program: 
* `def fibonacci(n):`

Modules used in program: 
* `import mysql.connector`
* `import random`
* `import statistics`
* `import re`

## python bincom test

Python mysql example: bincom test

```python
### Submitted By Bello Osagie Noah

print('\nQuestion 1 - 5, 8, 9, 6')
import re
file = open('C:/Users/user/Desktop/python_class_test.html')
f = file.read()
'''
pattern = re.compile(r'GREEN|PINK|YELLOW|BLUE|BROWN|ORANGE|CREAM|RED|WHITE|ARSH')
pattern = re.compile(r'GREEN')
matches = pattern.findall(f)
for match in matches:
    print(match, end='')
'''

pattern_arsh = re.compile(r'ARSH')
match_arsh = pattern_arsh.findall(f)
arsh_len = len(match_arsh)

pattern_green = re.compile(r'GREEN')
match_green = pattern_green.findall(f)
green_len = len(match_green)

pattern_pink = re.compile(r'PINK')
match_pink = pattern_pink.findall(f)
pink_len = len(match_pink)

pattern_yellow = re.compile(r'YELLOW')
match_yellow = pattern_yellow.findall(f)
yellow_len = len(match_yellow)

pattern_blue = re.compile(r'BLUE')
match_blue = pattern_blue.findall(f)
blue_len = len(match_blue)

pattern_brown = re.compile(r'BROWN')
match_brown = pattern_brown.findall(f)
brown_len = len(match_brown)

pattern_orange = re.compile(r'ORANGE')
match_orange = pattern_orange.findall(f)
orange_len = len(match_orange)

pattern_cream = re.compile(r'CREAM')
match_cream = pattern_cream.findall(f)
cream_len = len(match_cream)

pattern_red = re.compile(r'RED')
match_red = pattern_red.findall(f)
red_len = len(match_red)

pattern_white = re.compile(r'WHITE')
match_white = pattern_white.findall(f)
white_len = len(match_white)

pattern_black = re.compile(r'BLACK')
match_black = pattern_black.findall(f)
black_len = len(match_black)

pattern_blew = re.compile(r'BLEW')
match_blew = pattern_blew.findall(f)
blew_len = len(match_blew)

dict_ = {
    'GREEN': green_len,
    'PINK': pink_len,
    'YELLOW': yellow_len,
    'BLUE': blue_len,
    'BROWN': brown_len,
    'ORANGE': orange_len,
    'CREAM': cream_len,
    'RED': red_len,
    'WHITE': white_len,
    'ARSH': arsh_len,
    'BLACK': black_len,
    'BLEW': blew_len
    }

n = len(dict_.values())
print('\nQuestion 1')
print(f'The mean color is "{sum(dict_.values()) / n}"')

print('\nQuestion 2')
print(f'The most frequent color is "{max(dict_.keys())}" - worn {max(dict_.values())} time')
list_color = list(dict_.values()).copy()
list_color_keys = list(zip(dict_.values(), dict_.keys())).copy()
print(list_color_keys)
list_color_keys.sort()
print(list_color_keys)

print('\nQuestion 3')
import statistics
median_color = f'That is value {statistics.median(list_color)}'
print(f'Median is {statistics.median(dict_.values())} -> {list_color_keys[5]} and {list_color_keys[6]}')

print('\nQuestion 4')
variance_color = statistics.pvariance(list_color)	
print(f'The variant number of all colors is {variance_color}')

print('\nQuestion 5')
prob_red = n / sum(dict_.values()) 
print(f'Probability that it is red or any other color is {round(prob_red, 2) * 100}%')

#for item in dict_.values():
    #print(item)
'''
array = [green_len, pink_len, yellow_len, blue_len, brown_len, orange_len, cream_len, red_len, white_len]
x = green_len + pink_len + yellow_len + blue_len + brown_len + orange_len + cream_len + red_len + white_len 
mean = x / n
print(mean)

print(max(array))
'''

print('\nQuestion 8')
import random
random_numbers = random.randint(8, 15)
hex_ = f'The binary number of {random_numbers} is {random_numbers:b}'
print(hex_)

print('\nQuestion 9')
# Fibonacci series 
def fibonacci(n):
    """Print a Fibonacci series up to n."""
    a, b = 0, 1
    while a < n:
        print(a, end=' ')
        a, b = b, a+b
    print()

fibonacci(50)

print('\nQuestion 6')
import mysql.connector

mydb = mysql.connector.connect(
  host="localhost",
  user="belloosagienoah",
  passwd="yourpassword1",
  database="mydatabase"
)

mycursor = mydb.cursor()

sql = "INSERT INTO customers (colors, frequencies) VALUES (%s, %s)"
val = [
  ('GREEN', green_len),
  ('PINK', pink_len),
  ('ORANGE', orange_len),
  ('ARSH', arsh_len),
  ('RED', red_len),
  ('WHITE', white_len),
  ('BROWN', brown_len),
  ('BLUE', blue_len),
  ('YELLOW', yellow_len),
  ('BLACK', yellow_len),
  ('BLEW', yellow_len),
]

mycursor.executemany(sql, val)

mydb.commit()

### Submitted By Bello Osagie Noah

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
