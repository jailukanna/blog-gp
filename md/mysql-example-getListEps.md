---
title: mysql example getListEps (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'getListEps'

Functions in program: 
* `def getListEps(url):`

Modules used in program: 
* `import mysql.connector`
* `import base64`
* `import re`
* `import requests`

## python getListEps

Python mysql example: getListEps

```python
from datetime import date
from bs4 import BeautifulSoup
import requests
import re
import base64
import mysql.connector

id_anime = "4"

def getListEps(url):
    # Request halaman utama anime berisi list eps
    content = requests.get(url)
    soup = BeautifulSoup(content.text, 'html.parser')
    # Mengambil list episodes
    result = soup.find('div', 'episodelist').find("ul").findChildren("a")
    link = []
    eps = []
    list_link = []
    for child in result:
        # Memasukkan link dan eps ke list
        eps.append(child['href'].split('-')[-1].replace("/", ""))
        link.append(child['href'])
    # Menghapus duplikat link dan eps
    link = list(dict.fromkeys(link))
    eps = list(dict.fromkeys(eps))
    # Menggabungkan link dan eps ke satu list
    for i in range(len(eps)):
        list_link.append([eps[i], link[i]])
    a=0
    # Mengambil link google drive dari masing2 eps
    while a < len(list_link):
        connect = mysql.connector.connect(host="localhost", user="amusfq", passwd="1412", database="anime_seasons")
        cursor = connect.cursor()
        eps = list_link[a][0]
        link = list_link[a][1]
        query = "SELECT * FROM list_eps WHERE eps='{}'".format(eps)
        cursor.execute(query)
        result = cursor.fetchall()
        # Mengecek data sudah ada di db atau tidak
        # Kalau ada skip
        if len(result) < 1:
            # Mengakses link dari episode yang akan diambil base link nya
            content = requests.get(url)
            soup = BeautifulSoup(content.text, 'html.parser')
            try:
                # Kalau sukses ambil link dari samehada (base link) masukkin ke variable
                # Kalau tidak null
                result = soup.find('div', 'download-eps').find_all("a", text="GD2")
                try:
                    # Mencoba membypass safelink system
                    # Kalau gagal link google drive == null
                    gd = result[-1]['href'].replace("https://anjay.info?id=", "")
                    url = "https://www.anjay.info/spatial-coexistence/"
                    # Mengakses safe link pertama
                    content = requests.get(gd)
                    soup = BeautifulSoup(content.text, 'html.parser')
                    post = {'eastsafelink_id': b64}
                    # mengakses safelink kedua
                    x = requests.post(url, data = post)
                    soup = BeautifulSoup(x.text, 'html.parser')
                    #print(the response text (the content of the requested file):)
                    regex = r'(?:https\:\/\/www\.ahexa\.com\/njir\/.*.\')'
                    z = re.findall(regex, soup.find(string=re.compile("ahexa")))
                    z = z[0][:-1]
                    # Mengakses safelink ketiga
                    x = requests.get(z)
                    soup = BeautifulSoup(x.text, 'html.parser')
                    regex = r"(?:aHR0cHM.*.\==\")"
                    z = re.search(regex, soup.prettify())
                    # Mencari link dengan 2 regex berbeda
                    if z is not None:
                        z = z.group()[:-1]
                        z = base64.b64decode(z).decode("utf-8")
                        print(z)
                        regex = r"(?:d\/.*\/)"
                        z = re.search(regex, z).group()[2:-1]
                    else:
                        regex = r"(?:open\?id\=.*\")"
                        z = re.search(regex, soup.prettify()).group()[8:-1]
                    gd = z
                except:
                    # gk dapet link T.T
                    gd = ""
            except:
                # gk dapet link T.T
                gd = ""
            result = soup.find_all("img")
            img = base64.b64encode(result[1]['src'].encode("utf-8")).decode("utf-8")
            # Masukkin data yg udh didapat id_anime, eps, eps url, google drive link, img per looping
            query = "INSERT INTO list_eps (id_anime, eps, url, gd, img, isDownloaded) VALUES ('{}', '{}', '{}', '{}', '{}', '0')".format(id_anime, eps, link, gd, img)
            try:
                cursor.execute(query)
            except:
                print(query)
            if cursor.rowcount > 0:
                connect.commit()
                print("Eps {} berhasil ditambah".format(eps))
            else:
                connect.rollback()
                print("Eps {} gagal ditambah".format(eps))
        # menambah increment looping
        a += 1

getListEps('https://www.samehadaku.tv/anime/black-clover/')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
