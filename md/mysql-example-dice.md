---
title: mysql example dice (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'dice'

Functions in program: 
* `def randNum2(it1,it2,it3):`
* `def randNum1():`

Modules used in program: 
* `import mysql.connector`
* `import re,random`
* `import asyncio`
* `import discord`

## python dice

Python mysql example: dice

```python
# -*- coding: utf-8 -*-
import discord
import asyncio
import re,random
import mysql.connector

#Mysql接続とディスコのAPI取得
conn = mysql.connector.connect(user='root', password='', host='localhost', database='cochar')
cur = conn.cursor()
client = discord.Client()

@client.event
@asyncio.coroutine
async def on_ready():
    pass
    print('Logged in as')
    print(client.user.name)
    print(client.user.id)
    print('------')
#メッセが入力されたら
@client.event
async def on_message(message):
    #メッセを格納
    strg = message.content
    #メッセを判定
    matchDice = re.search('1d100 <= (\d+)', message.content)
    Mchar = re.search("makeChar", strg)

    #以下各処理

    if Mchar:
        #名前の抽出
        slice = strg[Mchar.end():]
        #各ステータスの制作
        sql = 'insert into mchar values (0, %s, %s, %s, %s, %s, %s, %s, %s, %s)'
        str1 = randNum2(3,6,0)
        con1 = randNum2(3,6,0)
        pow1 = randNum2(3,6,0)
        dex1 = randNum2(3,6,0)
        app1 = randNum2(3,6,0)
        siz1 = randNum2(2,6,6)
        int1 = randNum2(2,6,6)
        edu1 = randNum2(3,6,3)
        #mysqlに挿入
        cur.execute(sql, (slice,str1,con1,pow1,dex1,app1,siz1,int1,edu1))

        #挿入したものの表示
        cur.execute("select * from mchar order by id desc limit 1;")
        for row in cur.fetchall():
            await client.send_message(message.channel,row[1:10])

        conn.commit()
        cur.close
        conn.close


    dice100 = re.search(r"([0-9]+)d([0-9]+)", strg)
    if dice100:
        await client.send_message(message.channel,'結果表示します！！')
        it1=int(dice100.group(1))
        it2=int(dice100.group(2))
        num2 = randNum2(it1,it2,0)
        await client.send_message(message.channel,num2)
#1d100のダイス作成
#1d100のダイス作成
def randNum1():
    randomNum = random.randint(1,100)
    return randomNum
#nDn+Dのダイス作成
def randNum2(it1,it2,it3):
    randomNum2=0
    for i in range(it1):
        diceNum = random.randint(1,it2)
        randomNum2 = randomNum2 + diceNum
    return randomNum2+it3

client.run("token")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
