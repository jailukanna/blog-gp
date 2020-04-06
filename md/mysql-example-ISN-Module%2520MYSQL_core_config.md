---
title: mysql example ISN-Module%2520MYSQL core config (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ISN-Module%2520MYSQL core config'

Functions in program: 
* `def get_brebie_lvl_5():`
* `def get_brebie_lvl_4():`
* `def get_brebie_lvl_3():`
* `def get_brebie_lvl_2():`
* `def get_brebie_lvl_1():`
* `def get_lvl():`
* `def get_money():`
* `def get_name():`
* `def get_info_line(l):`
* `def update_config_file(id):`

## python ISN-Module%2520MYSQL core config

Python mysql example: ISN-Module%2520MYSQL core config

```python
from Sqllib.getinfo import *

def update_config_file(id):
    lvl = getlvl(id)
    money = getmoney(id)
    brebie1 = get_brebie_lvl(id,1)
    brebie2 = get_brebie_lvl(id, 2)
    brebie3 = get_brebie_lvl(id, 3)
    brebie4 = get_brebie_lvl(id, 4)
    brebie5 = get_brebie_lvl(id, 5)
    file = open("data.txt", "w")
    file.write("player_name = " + str(id))
    file.write("\nplayer_money = " + str(money))
    file.write("\nplayer_lvl = " + str(lvl))
    file.write("\nbrebie_lvl_1 = " + str(brebie1))
    file.write("\nbrebie_lvl_2 = " + str(brebie2))
    file.write("\nbrebie_lvl_3 = " + str(brebie3))
    file.write("\nbrebie_lvl_4 = " + str(brebie4))
    file.write("\nbrebie_lvl_5 = " + str(brebie5))
    file.close()
    print("Data-UPDATED")



def get_info_line(l):
    file = open("data.txt", "r")
    lines = file.readlines()
    line = lines[l].replace("player_", "").replace("name = ", "").replace("lvl = ", "").replace("money = ", "").replace("brebie_lvl_1 = ", "").replace("brebie_lvl_2 = ", "").replace("brebie_lvl_3 = ", "").replace("brebie_lvl_4 = ", "").replace("brebie_lvl_5 = ", "")

    return line;

def get_name():
    id = get_info_line(0)
    return id;

def get_money():
    mony = get_info_line(1)
    money = int(mony)
    return money;

def get_lvl():
    lwl = get_info_line(2)
    lvl = int(lwl)
    return lvl;

def get_brebie_lvl_1():
    lwl = get_info_line(3)
    lvl = int(lwl)
    return lvl;

def get_brebie_lvl_2():
    lwl = get_info_line(4)
    lvl = int(lwl)
    return lvl;

def get_brebie_lvl_3():
    lwl = get_info_line(5)
    lvl = int(lwl)
    return lvl;

def get_brebie_lvl_4():
    lwl = get_info_line(6)
    lvl = int(lwl)
    return lvl;

def get_brebie_lvl_5():
    lwl = get_info_line(7)
    lvl = int(lwl)
    return lvl;

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
