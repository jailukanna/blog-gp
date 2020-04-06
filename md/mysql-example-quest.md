---
title: mysql example quest (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'quest'


Modules used in program: 
* `import datetime`
* `import mysql.connector as mariadb`
* `import configparser`
* `import locale`
* `import time`
* `import requests`
* `import json`

## python quest

Python mysql example: quest

```python
import json
import requests
import time
import locale
import configparser
import mysql.connector as mariadb
import datetime

config = configparser.ConfigParser()
config.read('config.ini')

machopURL = config.get('CONFIG', 'machopURL')
growlitheURL = config.get('CONFIG', 'growlitheURL')
aeroURL = config.get('CONFIG', 'aeroURL')
dratiniURL = config.get('CONFIG', 'dratiniURL')
laprasURL = config.get('CONFIG', 'laprasURL')
chanseyURL = config.get('CONFIG', 'chanseyURL')
spindaURL = config.get('CONFIG', 'spindaURL')
silverpinapURL = config.get('CONFIG', 'silverpinapURL')
rarecandyURL = config.get('CONFIG', 'rarecandyURL')
goldenURL = config.get('CONFIG', 'goldenURL')

bot_message=""
dateStr=datetime.datetime.now()
dateStr=f'{dateStr:%Y-%m-%d}'

query="select pokestop.latitude as lat, pokestop.longitude as lng, pokestop.name as stopname, pokestop.image as stopimg from pokestop LEFT JOIN trs_quest on pokestop.pokestop_id = trs_quest.GUID WHERE trs_quest.quest_pokemon_id=66 AND FROM_UNIXTIME(trs_quest.quest_timestamp) >='"+dateStr+"'"

mariadb_connection = mariadb.connect(host='192.168.1.128', user='userpass', password='userpass', database='DBNAME')
cursor = mariadb_connection.cursor()
cursor.execute(query)

for lat, lng, stopname, stopimg in cursor:
    bot_message="\n\nPokestop: {}\nReward: Machop".format(stopname)
    titleStr="{}".format(stopname)
    urlStr="{}".format(stopimg)
    imgStr="https://raw.githubusercontent.com/nhumber/icons/master/icons/66.png"

    data = {
        "username": "Machop",
        "avatar_url": imgStr,
        "title": titleStr,
        "icon_url": "https://raw.githubusercontent.com/nhumber/icons/master/icons/66.png",
        "content": bot_message+"\n"+urlStr,
        "thumbnail": "https://raw.githubusercontent.com/nhumber/icons/master/icons/66.png",
    }

    result = requests.post(machopURL, json=data)
    print(result)
    time.sleep(1)

query="select pokestop.latitude as lat, pokestop.longitude as lng, pokestop.name as stopname, pokestop.image as stopimg from pokestop LEFT JOIN trs_quest on pokestop.pokestop_id = trs_quest.GUID WHERE trs_quest.quest_pokemon_id=58 AND FROM_UNIXTIME(trs_quest.quest_timestamp) >='"+dateStr+"'"

mariadb_connection = mariadb.connect(host='192.168.1.128', user='userpass', password='userpass', database='DBNAME')
cursor = mariadb_connection.cursor()
cursor.execute(query)

for lat, lng, stopname, stopimg in cursor:
    bot_message="\n\nPokestop: {}\nReward: Growlithe".format(stopname)
    titleStr="{}".format(stopname)
    urlStr="{}".format(stopimg)
    imgStr="https://raw.githubusercontent.com/nhumber/icons/master/icons/58.png"

    data = {
        "username": "Growlithe",
        "avatar_url": imgStr,
        "title": titleStr,
        "icon_url": "https://raw.githubusercontent.com/nhumber/icons/master/icons/58.png",
        "content": bot_message+"\n"+urlStr,
        "thumbnail": "https://raw.githubusercontent.com/nhumber/icons/master/icons/58.png",
    }

    result = requests.post(growlitheURL, json=data)
    print(result)
    time.sleep(1)	

query="select pokestop.latitude as lat, pokestop.longitude as lng, pokestop.name as stopname, pokestop.image as stopimg from pokestop LEFT JOIN trs_quest on pokestop.pokestop_id = trs_quest.GUID WHERE trs_quest.quest_pokemon_id=142 AND FROM_UNIXTIME(trs_quest.quest_timestamp) >='"+dateStr+"'"

mariadb_connection = mariadb.connect(host='192.168.1.128', user='userpass', password='userpass', database='DBNAME')
cursor = mariadb_connection.cursor()
cursor.execute(query)

for lat, lng, stopname, stopimg in cursor:
    bot_message="\n\nPokestop: {}\nReward: Aerodactyl".format(stopname)
    titleStr="{}".format(stopname)
    urlStr="{}".format(stopimg)
    imgStr="https://raw.githubusercontent.com/nhumber/icons/master/icons/142.png"

    data = {
        "username": "Aerodactyl",
        "avatar_url": imgStr,
        "title": titleStr,
        "icon_url": "https://raw.githubusercontent.com/nhumber/icons/master/icons/142.png",
        "content": bot_message+"\n"+urlStr,
        "thumbnail": "https://raw.githubusercontent.com/nhumber/icons/master/icons/142.png",
    }

    result = requests.post(aeroURL, json=data)
    print(result)
    time.sleep(1)	

query="select pokestop.latitude as lat, pokestop.longitude as lng, pokestop.name as stopname, pokestop.image as stopimg from pokestop LEFT JOIN trs_quest on pokestop.pokestop_id = trs_quest.GUID WHERE trs_quest.quest_pokemon_id=147 AND FROM_UNIXTIME(trs_quest.quest_timestamp) >='"+dateStr+"'"

mariadb_connection = mariadb.connect(host='192.168.1.128', user='userpass', password='userpass', database='DBNAME')
cursor = mariadb_connection.cursor()
cursor.execute(query)

for lat, lng, stopname, stopimg in cursor:
    bot_message="\n\nPokestop: {}\nReward: Dratini".format(stopname)
    titleStr="{}".format(stopname)
    urlStr="{}".format(stopimg)
    imgStr="https://raw.githubusercontent.com/nhumber/icons/master/icons/147.png"

    data = {
        "username": "Dratini",
        "avatar_url": imgStr,
        "title": titleStr,
        "icon_url": "https://raw.githubusercontent.com/nhumber/icons/master/icons/147.png",
        "content": bot_message+"\n"+urlStr,
        "thumbnail": "https://raw.githubusercontent.com/nhumber/icons/master/icons/147.png",
    }

    result = requests.post(dratiniURL, json=data)
    print(result)
    time.sleep(1)	

query="select pokestop.latitude as lat, pokestop.longitude as lng, pokestop.name as stopname, pokestop.image as stopimg from pokestop LEFT JOIN trs_quest on pokestop.pokestop_id = trs_quest.GUID WHERE trs_quest.quest_pokemon_id=131 AND FROM_UNIXTIME(trs_quest.quest_timestamp) >='"+dateStr+"'"

mariadb_connection = mariadb.connect(host='192.168.1.128', user='userpass', password='userpass', database='DBNAME')
cursor = mariadb_connection.cursor()
cursor.execute(query)

for lat, lng, stopname, stopimg in cursor:
    bot_message="\n\nPokestop: {}\nReward: Lapras".format(stopname)
    titleStr="{}".format(stopname)
    urlStr="{}".format(stopimg)
    imgStr="https://raw.githubusercontent.com/nhumber/icons/master/icons/131.png"

    data = {
        "username": "Lapras",
        "avatar_url": imgStr,
        "title": titleStr,
        "icon_url": "https://raw.githubusercontent.com/nhumber/icons/master/icons/131.png",
        "content": bot_message+"\n"+urlStr,
        "thumbnail": "https://raw.githubusercontent.com/nhumber/icons/master/icons/131.png",
    }

    result = requests.post(laprasURL, json=data)
    print(result)
    time.sleep(1)	

query="select pokestop.latitude as lat, pokestop.longitude as lng, pokestop.name as stopname, pokestop.image as stopimg from pokestop LEFT JOIN trs_quest on pokestop.pokestop_id = trs_quest.GUID WHERE trs_quest.quest_pokemon_id=113 AND FROM_UNIXTIME(trs_quest.quest_timestamp) >='"+dateStr+"'"

mariadb_connection = mariadb.connect(host='192.168.1.128', user='userpass', password='userpass', database='DBNAME')
cursor = mariadb_connection.cursor()
cursor.execute(query)

for lat, lng, stopname, stopimg in cursor:
    bot_message="\n\nPokestop: {}\nReward: Chansey".format(stopname)
    titleStr="{}".format(stopname)
    urlStr="{}".format(stopimg)
    imgStr="https://raw.githubusercontent.com/nhumber/icons/master/icons/113.png"

    data = {
        "username": "Chansey",
        "avatar_url": imgStr,
        "title": titleStr,
        "icon_url": "https://raw.githubusercontent.com/nhumber/icons/master/icons/113.png",
        "content": bot_message+"\n"+urlStr,
        "thumbnail": "https://raw.githubusercontent.com/nhumber/icons/master/icons/113.png",
    }

    result = requests.post(chanseyURL, json=data)
    print(result)
    time.sleep(1)	


query="select pokestop.latitude as lat, pokestop.longitude as lng, pokestop.name as stopname, pokestop.image as stopimg from pokestop LEFT JOIN trs_quest on pokestop.pokestop_id = trs_quest.GUID WHERE trs_quest.quest_pokemon_id=327 AND FROM_UNIXTIME(trs_quest.quest_timestamp) >='"+dateStr+"'"

mariadb_connection = mariadb.connect(host='192.168.1.128', user='userpass', password='userpass', database='DBNAME')
cursor = mariadb_connection.cursor()
cursor.execute(query)

for lat, lng, stopname, stopimg in cursor:
    bot_message="\n\nPokestop: {}\nReward: Spinda".format(stopname)
    titleStr="{}".format(stopname)
    urlStr="{}".format(stopimg)
    imgStr="https://raw.githubusercontent.com/nhumber/icons/master/icons/327.png"

    data = {
        "username": "Spinda",
        "avatar_url": imgStr,
        "title": titleStr,
        "icon_url": "https://raw.githubusercontent.com/nhumber/icons/master/icons/327.png",
        "content": bot_message+"\n"+urlStr,
        "thumbnail": "https://raw.githubusercontent.com/nhumber/icons/master/icons/327.png",
    }

    result = requests.post(spindaURL, json=data)
    print(result)
    time.sleep(1)	

query="select pokestop.latitude as lat, pokestop.longitude as lng, pokestop.name as stopname, pokestop.image from pokestop LEFT JOIN trs_quest on pokestop.pokestop_id = trs_quest.GUID WHERE trs_quest.quest_item_id=708 AND FROM_UNIXTIME(trs_quest.quest_timestamp) >='"+dateStr+"'"

mariadb_connection = mariadb.connect(host='192.168.1.128', user='userpass', password='userpass', database='DBNAME')
cursor = mariadb_connection.cursor()
cursor.execute(query)

for lat, lng, stopname, stopimg in cursor:
    bot_message="\n\nPokestop: {}\nReward: 5x Silver Pinap".format(stopname)
    titleStr="{}".format(stopname)
    urlStr="{}".format(stopimg)
    imgStr="https://pokemongohub.net/wp-content/uploads/2018/08/Item_0707.png"	

    data = {
        "username": "SilverPinap",
        "avatar_url": imgStr,
        "title": titleStr,
        "icon_url": "https://pokemongohub.net/wp-content/uploads/2018/08/Item_0707.png",
        "content": bot_message+"\n"+urlStr,
        "thumbnail": {"url": "https://pokemongohub.net/wp-content/uploads/2018/08/Item_0707.png"} 
    }

    result = requests.post(silverpinapURL, json=data)
    print(result)
    time.sleep(1)
	
query="select pokestop.latitude as lat, pokestop.longitude as lng, pokestop.name as stopname, pokestop.image from pokestop LEFT JOIN trs_quest on pokestop.pokestop_id = trs_quest.GUID WHERE trs_quest.quest_item_id=1301 AND FROM_UNIXTIME(trs_quest.quest_timestamp) >='"+dateStr+"'"

mariadb_connection = mariadb.connect(host='192.168.1.128', user='userpass', password='userpass', database='DBNAME')
cursor = mariadb_connection.cursor()
cursor.execute(query)

for lat, lng, stopname, stopimg in cursor:
    bot_message="\n\nPokestop: {}\nReward: 3x  Rare Candy".format(stopname)
    titleStr="{}".format(stopname)
    urlStr="{}".format(stopimg)
    imgStr="https://pokemongohub.net/wp-content/uploads/2018/03/Item_1301.png"	

    data = {
        "username": "RareCandy",
        "avatar_url": imgStr,
        "title": titleStr,
        "icon_url": "https://pokemongohub.net/wp-content/uploads/2018/03/Item_1301.png",
        "content": bot_message+"\n"+urlStr,
        "thumbnail": {"url": "https://pokemongohub.net/wp-content/uploads/2018/03/Item_1301.png"} 
    }

    result = requests.post(rarecandyURL, json=data)
    print(result)
    time.sleep(1)

query="select pokestop.latitude as lat, pokestop.longitude as lng, pokestop.name as stopname, pokestop.image from pokestop LEFT JOIN trs_quest on pokestop.pokestop_id = trs_quest.GUID WHERE trs_quest.quest_item_id=706 AND FROM_UNIXTIME(trs_quest.quest_timestamp) >='"+dateStr+"'"

mariadb_connection = mariadb.connect(host='192.168.1.128', user='userpass', password='userpass', database='DBNAME')
cursor = mariadb_connection.cursor()
cursor.execute(query)

for lat, lng, stopname, stopimg in cursor:
    bot_message="\n\nPokestop: {}\nReward: Golden Berry".format(stopname)
    titleStr="{}".format(stopname)
    urlStr="{}".format(stopimg)
    imgStr="https://pokemongohub.net/wp-content/uploads/2017/06/item_0706.png"	

    data = {
        "username": "GoldenBerry",
        "avatar_url": imgStr,
        "title": titleStr,
        "icon_url": "https://pokemongohub.net/wp-content/uploads/2017/06/item_0706.png",
        "content": bot_message+"\n"+urlStr,
        "thumbnail": {"url": "https://pokemongohub.net/wp-content/uploads/2017/06/item_0706.png"} 
    }

    result = requests.post(goldenURL, json=data)
    print(result)
    time.sleep(1)

mariadb_connection.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
