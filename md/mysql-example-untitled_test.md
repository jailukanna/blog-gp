---
title: mysql example untitled test (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'untitled test'

Functions in program: 
* `def init():`
* `def animate(i):`
* `def GetresultByMouth(drive,element,num):`

Modules used in program: 
* `import time`
* `import datetime`
* `import matplotlib.pyplot as plt`
* `import pandas as pd`
* `import numpy as np`

## python untitled test

Python mysql example: untitled test

```python
# -*- coding: utf-8 -*-
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib import animation
import datetime
import time
from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from Mysql import Mysql

def GetresultByMouth(drive,element,num):
    T = {}
    for i in range(600):
         ActionChains(drive).move_to_element_with_offset(element, 2*i+20, 100).perform()
         try:
             result_date=drive.find_element_by_xpath('/html/body/div[1]/div[2]/div[2]/div/div['+num+']/div[1]/div[3]/div[1]/div[1]/div/div[2]/div[1]').text
             times=result_date.split('~')
             if len(times)>1:
                 starttime=times[0][:-1]
                 if not T.__contains__(starttime):
                    results = drive.find_elements_by_xpath('/html/body/div[1]/div[2]/div[2]/div/div['+num+']/div[1]/div[3]/div[1]/div[1]/div/div[2]/div')
                    temr=[];
                    for i in range(4):
                        result=results[i+1].text.split('\n')
                        temr.append(int(result[1].replace(',','').replace(' ','')))
                    T[starttime]=temr
         except Exception as e:
             print("11111111111111111")
    return  T


# my_sql = Mysql()
drive = webdriver.Chrome('C:/Users/Administrator/AppData/Local/Google/Chrome/Application/chromedriver.exe')
webdriver.Chrome
drive.get('http://index.baidu.com/#/')

# element = drive.find_element_by_css_selector('#home > div.header > div.header-right > div.links-horizontal-container '+
#                                            '> div.links-item.link-cursor.links-item-unlogin > span > span').click()
# time.sleep(2)
# elementname=drive.find_element_by_id('TANGRAM__PSP_4__userName').send_keys('13261718093')
# time.sleep(2)
# elementpassword=drive.find_element_by_id('TANGRAM__PSP_4__password').send_keys('wangrui11254')
#
# time.sleep(5)
# element=drive.find_element_by_id('TANGRAM__PSP_4__submit').click()
# coo=drive.get_cookies();
# print(coo)

mycookis=[{'domain': '.index.baidu.com', 'expiry': 1574855883, 'httpOnly': False, 'name': 'Hm_lvt_d101ea4d2a5c67dab98251f0b5de24dc', 'path': '/', 'secure': False, 'value': '1543319871'},
         {'domain': '.index.baidu.com', 'httpOnly': False, 'name': 'Hm_lpvt_d101ea4d2a5c67dab98251f0b5de24dc', 'path': '/', 'secure': False, 'value': '1543319884'},
         {'domain': '.baidu.com', 'expiry': 1574855871.534802, 'httpOnly': False, 'name': 'BAIDUID', 'path': '/', 'secure': False, 'value': 'C6EC006D30A1ED8F68579FDE7EBE46F4:FG=1'},
         {'domain': '.baidu.com', 'expiry': 1802519882.21532, 'httpOnly': True, 'name': 'BDUSS', 'path': '/', 'secure': False, 'value': '2lxOGFHS2VRbnBCOUp5WnhORDF1LXN4dERiRDN-aklqemhuWk9tczhQUWV2aVJjQVFBQUFBJCQAAAAAAAAAAAEAAAB3WjsIsK7U2sbatP3Q0rijAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAB4x~VseMf1bQ'},
         {'domain': 'index.baidu.com', 'expiry': 1543406283.441749, 'httpOnly': False, 'name': 'CHKFORREG', 'path': '/', 'secure': False, 'value': '82df9e797812ac909bece66efecc8534'}]
for cooki in mycookis:
    drive.add_cookie(cooki)

drive.refresh()


# 输入检索词
time.sleep(2)
element=drive.find_element_by_xpath('//*[@id="search-input-form"]/input[3]').send_keys('山东鲁能')
element=drive.find_element_by_xpath('//*[@id="home"]/div[2]/div[2]/div/div[1]/div/div[2]/div/span/span').click()

# 最大化窗口
drive.maximize_window()
# 更改时间
element=drive.find_element_by_xpath('/html/body/div[1]/div[2]/div[2]/div/div[2]/div[1]/div[1]/div[1]/div/div[2]/button').click()
element=drive.find_element_by_xpath('/html/body/div[3]/div/div/div[6]').click()
element=drive.find_element_by_xpath('/html/body/div[1]/div[2]/div[2]/div/div[3]/div[1]/div[1]/div[1]/div/div[2]/button ').click()
element=drive.find_element_by_xpath('/html/body/div[8]/div/div/div[5]').click()
element=drive.find_element_by_xpath('/html/body/div[1]/div[2]/div[2]/div/div[3]/div[1]/div[1]/div[1]/ul/li[2]').click()



# 添加关键词
keywords=['山东鲁能','北京国安','广州恒大','上海上港']
for i in range(3):
    element=drive.find_element_by_xpath('//html/body/div[1]/div[2]/div[2]/div/div[1]/div/div/div['+str(i+2)+']').click()
    element = drive.find_element_by_xpath(
        '//html/body/div[1]/div[2]/div[2]/div/div[1]/div/div/div[' + str(i + 2) + ']/div[2]/input').send_keys(keywords[i+1])
element=drive.find_element_by_xpath('//html/body/div[1]/div[2]/div[2]/div/div[1]/div/div/span').click()
time.sleep(2)





# 移动鼠标 搜索量
element=drive.find_element_by_xpath('/html/body/div[1]/div[2]/div[2]/div/div[2]/div[1]/div[3]/div[1]/div[1]/div/div[1]/canvas')
T1=GetresultByMouth(drive,element,'2')

# 新闻资
element=drive.find_element_by_xpath('/html/body/div[1]/div[2]/div[2]/div/div[3]/div[1]/div[3]/div[1]/div[1]/div/div[1]/canvas')
T2=GetresultByMouth(drive,element,'3')

myT=[]
Num=[]
for dic in T1:
    myT.append(dic.replace(' ',''))
    Num.append(T1[dic])


ts = pd.DataFrame(np.array(Num),columns=keywords,index=pd.to_datetime(np.array(myT)))
ts1=pd.DataFrame(np.zeros([len(pd.date_range(myT[0],myT[-1])),4],int),columns=keywords,index=pd.date_range(myT[0],myT[-1]))
ts2=ts+ts1
ts2=ts2.fillna(method='pad')

years=np.arange(2011,2019)
moths=np.arange(1,13)
key=lambda x:x.year
groped=ts2.groupby(key)
resultyear=groped.sum()

fig, ax = plt.subplots()
x = range(4)
mybar = plt.bar(x, 0*np.array(range(4)))
def animate(i):
    mybar = plt.bar(x, Num[i])
    time.sleep(.5)
    return mybar
def init():
    mybar = plt.bar(x, Num[0])
    return mybar
ani = animation.FuncAnimation(fig=fig, func=animate, frames=10,
                              init_func=init, interval=1, blit=False)

plt.show()
#
# fig=plt.figure()
# mybar=plt.bar([],[])
#

# print(len(myT))
# print(array(myT[1:])-array(myT[:-1]))
#
# plt.plot(myT,Num)
# plt.show()










#
# b=[];
# for i in range(10):
#     print(i*4)
#     b.append(i*4)
#
# plt.plot(range(10),b)
# plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
