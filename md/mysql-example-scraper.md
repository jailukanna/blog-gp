---
title: mysql example scraper (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scraper'

Functions in program: 
* `def findChatbots(html_input, results):`
* `def getHTML(url, driver, output_name):`

Modules used in program: 
* `import mysql.connector`
* `import requests`

## python scraper

Python mysql example: scraper

```python
from selenium import webdriver
from lxml import html
import requests
from lxml import etree
import mysql.connector


timeout_duration = 15

#establish database connection
mydb = mysql.connector.connect(
  host="localhost",
  user="root",
  passwd=""
)



def getHTML(url, driver, output_name):
    driver.get(url)
    # This will get the initial html - before javascript
    html1 = driver.page_source
    # This will get the html after on-load javascript
    html2 = driver.execute_script("return document.documentElement.innerHTML;")
    #file1 = open("html_no_js.html", "w")
    outfile = open(output_name, "w")
    outfile.write(html2)
    outfile.close()
    return html2

def findChatbots(html_input, results):
    html = etree.HTML(html_input)
    ids = html.xpath('//@id')
    element_sources = html.xpath("//@src")
    classes = html.xpath('//@class')

    #check for ids in elements
    for id in ids:
        #print((id))
        if "intercom-container" in id:
            results.append("intercom")
        if "drift-widget-container" in id:
            results.append("drift")
        if "collect-chat-frame-container" in id:
            results.append("collect.chat")
        if "fxo-widget-iframe" in id:
            results.append("flowXO")
        if "fc_widget" in id:
            results.append("freshchat")
        if "botsify-chat-widget-div" in id:
            results.append("botsify")
        if "app-landbot" in id:
            results.append("landbot")
        if "engt-container" in id:
            results.append("engati")
        if "botstar-widget-container" in id:
            results.append("botstar")


    #check for classes
    for element in classes:
        if "LandbotLauncher" in element:
            results.append("landbot")


    if html_input.find("js.driftt.com") != -1:
        results.append("drift")
    if (html_input.find("collectcdn.com") != -1 )or (html_input.find("collect.chat") != -1):
        results.append("collect.chat")
    if html_input.find("botsify.com/web-bot") != -1:
        results.append("bostify")
    if html_input.find("hellotars.com") != -1:
        results.append("tars")
    if html_input.find("widget.intercom.io") != -1:
        results.append("intercom")
    if html_input.find("widget.flowxo.com") != -1:
        results.append("flowXO")
    if html_input.find("app.wotnot.io") != -1:
        results.append("wotnot")
    if html_input.find("landbot.io") != -1:
        results.append("landbot")
    if html_input.find("app.engati.com") != -1:
        results.append("engati")
    if html_input.find("wchat.freshchat.com") != -1:
        results.append("freshchat")
    if html_input.find("ada.support") != -1:
        results.append("ada")
    if html_input.find("widget.botstar.com") != -1:
        results.append("botstar")


    #identify ada platform
    iframes = html.xpath('//iframe/@src')
    print(("iframes ", len(iframes)))
    for iframe in iframes:
        if ".ada.support/chat" in iframe:
            results.append("ada")

    print((results))
    return results



cursor = mydb.cursor(buffered=True)
cursor.execute("USE chatbot")
cursor.execute("SELECT * FROM urls")
cursor3 = mydb.cursor(buffered=True)
cursor3.execute("SELECT value FROM data WHERE attribute = 'current_record'")
for x in cursor3:
    current_record = x[0]
    print((current_record))
for record in cursor:
    if record[0] < current_record:
        print("skipping url")
        continue
    current_record = record[0]
    #sql = ("UPDATE data SET value = %s WHERE attribute = 'current_record';" %(current_record))
    #print((sql))
    cursor3.execute("UPDATE data SET value = {} WHERE attribute = 'current_record';".format(str(current_record)))
    mydb.commit()
    print("procesing url no: ", record[0], " url: ", record[1])
    url = record[1]
    #url = "surfstartupwave.vn/"    
    #add "http://"
    if (url[:7] != "http://") or (url[:8] != "https://"):
        url = "http://" + url
    results = []
    options = webdriver.ChromeOptions()
    options.add_argument('headless')
    driverChrome = webdriver.Chrome(chrome_options=options)
    driverChrome.implicitly_wait(30)
    driverPhantom = webdriver.PhantomJS()
    driverPhantom.set_page_load_timeout(30)
    try:
        htmlChrome = getHTML(url, driverChrome, "html.html")
        results = findChatbots(htmlChrome, results)
    except:
        print("chrome error")
    try:
        htmlPhantom = getHTML(url, driverPhantom, "html2.html")
        results = findChatbots(htmlPhantom, results)
    except:
        print("phantom error")
    results = set(results)
    cursor2 = mydb.cursor(buffered=True)
    for result in results:
        sql = "INSERT INTO chatbotForUrl (url_id, chatbot) VALUES (%s, %s);"
        values = (record[0], result)
        cursor2.execute(sql, values)
        mydb.commit()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
