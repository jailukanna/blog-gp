---
title: mysql example fill-url (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'fill-url'


Modules used in program: 
* `import mysql.connector`

## python fill-url

Python mysql example: fill-url

```python
syntax: markdown
### Автоматическое заполнение полей URL A и URL B инвентарных данных заббикса

Для удобства, создать папку для скриптов в домашней папке пользователя:
`cd ~/`
`mkdir scripts`

Создаём файл в созданной папке:
`cd scripts`
`nano fill_url.py`

С таким содержимым:

```python
#!/usr/bin/env python3

"""
Необходиыме пакеты:
    sudo apt-get install -y python3.6 python3-pip python3-pip
    pip3 install mysql-connector-python

Скрипт заполняет стоблцы "url_a" и "url_b" в таблице "host_inventory" данными
из столбца "ip" таблицы "interface", добавив перед "ip" с помощью функции
concat строку 'http://' и 'telnet://'.
"""
import mysql.connector

# подключение к базе, надо заменить своими данными для подключения
mydb = mysql.connector.connect(
    host='localhost',
    user='zabbix',
    passwd='password',
    database='zabbix'
)
# создаём курсов, который будет вводить команды
mycursor = mydb.cursor()
# собственно сами команды
# можно под себя команды переделать, например изменить строки 'http://',
# 'telnet://' на например 'ssh://', 'whatever'
# или диапазон применения с '192.168.176.%' на '192.168.%.%' | '172.16.%.%'
# и т.д.
fill_a = (
    "UPDATE host_inventory "
    "LEFT JOIN interface ON host_inventory.hostid = interface.hostid "
    "SET host_inventory.url_a=concat('http://',interface.ip) "
    "WHERE interface.ip LIKE '192.168.176.%'; "
)
fill_b = (
    "UPDATE host_inventory "
    "LEFT JOIN interface ON host_inventory.hostid = interface.hostid "
    "SET host_inventory.url_b=concat('telnet://',interface.ip) "
    "WHERE interface.ip LIKE '192.168.176.%'; "
)
# выполнить команды
mycursor.execute(fill_a)
mycursor.execute(fill_b)
# коммит изменений
mydb.commit()
print(mycursor.rowcount, "URLs: record(s) affected")
# закрыть подключени к БД
mydb.close()
```

Я учу python, поэтому всё на нём пишу.
Чтобы скрипт заработал, надо установить пакеты:
`sudo apt-get install -y python3.6 python3-pip python3-pip`
`pip3 install mysql-connector-python`

Сделать файл исполняемым:
`sudo chmod +x fill_url.py`

Добавить скрипт в крон:
`crontab -e`
в конец файла:
`*/5 * * * * python3 /home/username/scripts/fill_url.py`
скрипт будет запускаться каждые 5 минут, для меня это оптимальное время.

На всякий можно проверить в логах, выполняется ли скрипт cron'ом:

```bash
tail -f /var/log/syslog | grep CRON
Apr 16 05:25:01 zabbix CRON[7288]: (username) CMD (python3 /home/username/scripts/fill_url.py)
```

Узлы могут добавляться вручную, по-одному. Решил, что удобнее запускать через cron,
а не через заббикс.

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
