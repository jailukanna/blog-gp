---
title: mysql example web%2520browser%2520opening%2520a%2520specific%2520page (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'web%2520browser%2520opening%2520a%2520specific%2520page'


Modules used in program: 
* `import datetime`
* `import webbrowser`
* `import selenium`
* `import urllib.request`

## python web%2520browser%2520opening%2520a%2520specific%2520page

Python mysql example: web%2520browser%2520opening%2520a%2520specific%2520page

```python

# program for downloading the pdfs
import urllib.request
import selenium
import webbrowser
import datetime
webbrowser.open('https://posoco.in/reports/daily-reports/', new=2)
Date=datetime.datetime.now().strftime("%d.%m.%y")
Date=str(Date)
print((Date))
#downloading the file

# moving the file into some other folder

"""def Download_pdf():
    
    Date=datetime.datetime.now().strftime("%d.%m.%y")
    Date=str(Date)
    print((Date))
    try:
        driver.get('https://posoco.in/download/'+Date+'_nldc_psp/')
    except:
        print('Timeout...Retrying.....')
        time.sleep(900)
        Download_pdf()    
    driver.maximize_window()
    WebDriverWait(driver, 20)
    time.sleep(10)
    try:
        driver.find_element_by_xpath('/html/body/div[1]/div[1]/div/main/article/div/div/div/div[2]/table/tbody/tr[5]/td/a').click()
        time.sleep(10)
        shutil.move("Users/abhishek chandelpython program/"+"_NLDC_PSP.pdf", "Users/abhishek chandel/Desktop/nldc files/code_11/nldc_daily_reports")
    except:
     print('File Not found...Retrying.....')
     time.sleep(1800)
     Download_pdf()   """ 


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
