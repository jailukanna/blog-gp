---
title: mysql example word%2520counter (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'word%2520counter'

Functions in program: 
* `def clean_up_list(word_list):`
* `def start(url):`

Modules used in program: 
* `import operator`
* `import requests`

## python word%2520counter

Python mysql example: word%2520counter

```python
import requests

from BeautifulSoup4 import beautifulsoup as soup
import operator
 
def start(url):
    word_list = []
    source_code = requests.get(url).text
    soups = soup(source_code)
    for post_text in soups.findAll('a',{'class': "mar_b" }):
        content = post_text.string
        words = content.lower().split()
        for each_word in words:
            print(each_word)
            word_list.append(each_word)
        clean_up_list(word_list)

def clean_up_list(word_list):
    clean_word_list = []
    for word in word_list:
        symbols = r"!@#$%^&*()_+<>{}:\"[];'.,/\|"
        for i in range(0, len(symbols)):
            word = word.replace(symbols[i],"")
            
        


start('http://www.cercind.gov.in/')
        


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
