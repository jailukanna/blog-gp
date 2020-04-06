---
title: mysql example seo-crawler (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'seo-crawler'

Functions in program: 
* `def lanzaCrawler():`
* `def ejecutaCrawleo(dominioExacto, url, subdominio, protocolo, mycursor, mydb):`
* `def insertaSCRAP(mycursor, mydb, dominio, url, etiqueta, atributo, valor, texto, control):`
* `def insertaNuevosDominios(url, mycursor, mydb):    `

Modules used in program: 
* `import tldextract`
* `import mysql.connector`
* `import requests`

## python seo-crawler

Python mysql example: seo-crawler

```python
import requests
from urllib.parse import urljoin
from bs4 import BeautifulSoup
import mysql.connector
from urllib.parse import urlparse
import tldextract

def insertaNuevosDominios(url, mycursor, mydb):    
    ext = tldextract.extract(url)
    parsed_uri = urlparse(url)
    uri=parsed_uri
    dominio = uri.netloc
    dominio = dominio.replace(ext.domain+'.', '')
    sql = "INSERT IGNORE INTO dominios (dominio, protocolo, subdominio) VALUES (%s, %s, %s)"
    val = (ext.domain +'.'+ ext.suffix, uri.scheme, ext.subdomain)
    mycursor.execute(sql, val)
    mydb.commit()

def insertaSCRAP(mycursor, mydb, dominio, url, etiqueta, atributo, valor, texto, control):
    try:
        sql = "INSERT IGNORE INTO scrap (dominio, url, etiqueta, atributo, valor, texto, control) VALUES (%s, %s, %s, %s, %s, %s, %s)"
        val = (dominio, url,etiqueta, atributo, valor, texto , control)
        mycursor.execute(sql, val)
        mydb.commit()
        print('IN: ' + url + " " + etiqueta + ": " + valor+ " - " + texto)
    except requests.exceptions.RequestException as e:
        print(e)
        print("error insertaSCRAP")
    
def ejecutaCrawleo(dominioExacto, url, subdominio, protocolo, mycursor, mydb):
    if subdominio == "":
        subdominio = ""
    else:
        subdominio = subdominio + "."
    dominio = protocolo +'://'+ subdominio + dominioExacto + '/'
    urlCompleta = urljoin(dominio, url)
    headers = {
            'User-Agent': 'MollaBot 0.1',
            'From': 'contacto@victormolla.com'
    }
    print("EJE: "  + urlCompleta)
    try:
        req = requests.get(urlCompleta, headers=headers)
        soup = BeautifulSoup(req.text, "lxml") 
        
        for etiqueta in soup.find_all('h1'):
            insertaSCRAP(mycursor, mydb, dominioExacto, urlCompleta,'h1', '', '', etiqueta.text, dominio + url + 'h1' + etiqueta.text)
            
        for etiqueta in soup.find_all('title'):
            insertaSCRAP(mycursor, mydb, dominioExacto, urlCompleta,'title', '', '', etiqueta.text, dominio + url + 'title' + etiqueta.text)
            
   
        for a in soup.find_all('a', href=True):
            urlCompleta = urljoin(dominio, url)
            insertaSCRAP(mycursor, mydb, dominioExacto, urlCompleta,'a', 'href', a['href'], a.text, dominio + url + 'a' + 'href' + a['href'] + a.text)
            
            if '://' in a['href']:
                insertaNuevosDominios( a['href'], mycursor, mydb)

    except requests.exceptions.RequestException as e:
        print(e)
        print("error ejecutaCrawleo")
        
def lanzaCrawler():
    mydb = mysql.connector.connect(
    host="HOST",
    user="USER",
    passwd="PASS", 
    database="BD"
    )
    mycursor = mydb.cursor()
    mycursor.execute("SELECT dominio, estado, subdominio, protocolo FROM DOMINIOS WHERE estado != 1;")     
    myresult = mycursor.fetchall()  
    for x in myresult:
        dominio = x[0]
        estado = x[1]
        subdominio = x[2]
        protocolo = x[3]
        if estado == -1:
            ejecutaCrawleo(dominio, '', subdominio, protocolo, mycursor, mydb)
            sql = "UPDATE dominios SET estado = '0' WHERE dominio = '"+dominio+"'"
            mycursor.execute(sql)
            mydb.commit()
        else:             
            sql = "SELECT DISTINCT valor,dominio FROM SCRAP WHERE valor NOT IN (select url FROM SCRAP) AND (valor LIKE '%"+dominio+"%') AND valor NOT LIKE '%#%' AND valor LIKE '%/%';"
            mycursor.execute(sql) 
            myresult = mycursor.fetchall()
            totalUrls = len(myresult)
            print("totalUrls: " + str(totalUrls))
            if totalUrls > 0:
                for x in myresult:
                    ejecutaCrawleo(x[1], x[0], subdominio, protocolo, mycursor, mydb)
            else:
                sql = "UPDATE dominios SET estado = '1' WHERE dominio = '"+dominio+"'"
                mycursor.execute(sql)
                mydb.commit()
    mycursor.close()
            
while True:
    lanzaCrawler()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
