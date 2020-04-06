---
title: mysql example automatically submit issue (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'automatically submit issue'

Functions in program: 
* `def release_issues():`
* `def get_title_and_content_by_date(mydb):`
* `def timer(callback, target_time):`
* `def handle_browser(title, content):`
* `def createMysql(): `

Modules used in program: 
* `import mysql.connector`
* `import os`
* `import threading`
* `import re`
* `import time`
* `import datetime`

## python automatically submit issue

Python mysql example: automatically submit issue

```python
# encoding: utf-8
from selenium import webdriver
import datetime
import time
import re
import threading
import os
import mysql.connector

URL_LOGIN = "https://github.com/login"
target_url = "https://github.com/xxxxxx/xxxxxx/issues"
login_field = "xxxxxx"
password = "xxxxxx"

def createMysql(): 
  mydb = mysql.connector.connect(
    host="xxxxxx",
    user="xxxxxx",
    passwd="xxxxxx",
    database="xxxxxx",
  )
  return mydb



def handle_browser(title, content):
  chrome_options = webdriver.ChromeOptions()
  # chrome_options.binary_location  = "/usr/local/lib/python3.5/dist-packages/selenium/webdriver/chrome"
  chrome_options.add_argument('--no-sandbox')
  chrome_options.add_argument('--disable-dev-shm-usage')
  chrome_options.add_argument('--headless')
  browser = webdriver.Chrome('/usr/local/bin/chromedriver', chrome_options=chrome_options)
  browser.get(URL_LOGIN)

  browser.find_element_by_id('login_field').send_keys(login_field)
  browser.find_element_by_id('password').send_keys(password)
  browser.find_element_by_xpath("/html/body/div[3]/main/div/form/div[3]/input[4]").click()

  browser.get(target_url)

  browser.find_element_by_xpath("/html/body/div[4]/div/main/div[2]/div[1]/div/div[1]/a").click()

  browser.find_element_by_xpath('//*[@id="issue_title"]').send_keys(title)
  browser.find_element_by_xpath('//*[@id="issue_body"]').send_keys(content)
  browser.find_element_by_xpath("/html/body/div[4]/div/main/div[2]/div[1]/div/form/div/div[1]/div/div/div[3]/button").click()

def timer(callback, target_time):
  print('start')
  while True:
    time.sleep(1)
    curTime = time.ctime()
    print(curTime)
    # 5 am
    if re.search(target_time,curTime) != None: 
      callback()
      time.sleep(60)

def get_title_and_content_by_date(mydb):
  mycursor = mydb.cursor()
  today = datetime.date.today()
  now = str(today.year)+'-'+str(today.month)+'-'+str(today.day)
  sql = "SELECT title, content, release_time FROM issue where release_time = "+"'"+now+"'"
  print('sql:' + sql)
  mycursor.execute(sql)

  myresult = mycursor.fetchall()
  print(len(myresult))
  print(myresult)
  if len(myresult) > 0:
    return {'title': myresult[0][0], 'content': myresult[0][1]}
  return None  

def release_issues():
  mydb = createMysql()
  info = get_title_and_content_by_date(mydb)
  if info == None:
    print('title and content have no value')
  else:
    title = info['title']
    content = info['content']
    print("title:" + title)
    print("content:" + content)
    handle_browser(title, content)
    print('release issue success')
  mydb.close()

if __name__ == '__main__':
  timer(release_issues, "21:00:[0-6][0-9]")

  # sudo nohup python3 -u release_issue.py > output.log &
  

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
