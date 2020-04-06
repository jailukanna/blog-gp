---
title: mysql example download%2520pdf%2520base1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'download%2520pdf%2520base1'

Functions in program: 
* `def Download_pdf():`

Modules used in program: 
* `import datetime`
* `import db_conn_param`
* `import mysql.connector`
* `import os`
* `import mysql.connector as mariadb`
* `import xlrd`
* `import time`

## python download%2520pdf%2520base1

Python mysql example: download%2520pdf%2520base1

```python
#downloading pdf files from web
from urllib import request
import time
import xlrd
import mysql.connector as mariadb
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import Select, WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from time import sleep, strftime
import os
from datetime import date, timedelta
import mysql.connector
from pyvirtualdisplay import Display
import db_conn_param
import datetime
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.action_chains import ActionChains

def Download_pdf():
    
    Date=datetime.datetime.now().strftime("%d-%m-%Y")
    Date=str(Date)
    try:
        driver.get('https://posoco.in/download/'+Date+'_nldc_psp/')
    except:
        print(('Timeout...Retrying.....'))
        time.sleep(900)
        Download_pdf()    
    driver.maximize_window()
    WebDriverWait(driver, 20)
    time.sleep(10)
    try:
        driver.find_element_by_xpath('/html/body/div[1]/div[1]/div/main/article/div/div/div/div[2]/table/tbody/tr[5]/td/a').click()
        time.sleep(10)
        shutil.move("Users/abhishek chandel/Desktop/nldc files/"+str(datetime.datetime.now().strftime("%d.%m.%y"))+"_NLDC_PSP.pdf", "Users/abhishek chandel/Desktop/nldc files/code_11/nldc_daily_reports")
    except:
        print(('File Not found...Retrying.....'))
        time.sleep(1800)
        Download_pdf()    
    #actions = ActionChains(driver)
    #actions.key_down(Keys.CONTROL).send_keys("s").key_up(Keys.CONTROL).perform()
    #elem = driver.switch_to_active_element()
    #elem.send_keys(Keys.RETURN)
    
    #actions.perform()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
