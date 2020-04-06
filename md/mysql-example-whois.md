---
title: mysql example whois (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'whois'

Functions in program: 
* `def makehtml():`

Modules used in program: 
* `import mysql.connector`
* `import time`

## python whois

Python mysql example: whois

```python
#! /usr/bin/python2.7

import time
import mysql.connector
read_data = ""
file_path ="/var/www/html/whois.html"
connector = mysql.connector.connect(user='cowrie',
				    password='Kaltopmih',
				    host='127.0.0.1',
				    database='cowrie',
				    autocommit=True)

query="select ip from sessions, auth where sessions.id = session order by timestamp DESC LIMIT 300;"
base ="""<html>
<style>
.btn-group button {
    background-color: LimeGreen;
    border: 1px solid green; 
    color: black; 
    padding: 10px 24px; 
    cursor: pointer; 
    float:left;
    position: relative;
    left:40%;

}

.btn-group:after {
    content: "";
    clear: both;
    display: table;
}

.btn-group button:not(:last-child) {
    border-right: none;
}

.btn-group button:hover {
    background-color: green;
}

iframe {
  background-color:black;
  height: 200%;
  width: 80%;
  resize: both;
  overflow: auto;
}
body {
        background-color:black;
        color:LimeGreen;
}
h1, p {
        color:LimeGreen;
        font-family:verdana,sans,sans-serif;
        text-align:center;
}
p {
        padding-bottom:50px;
}
input {
        background-color:LimeGreen;
}
</style>
<head>
  <title>Who Is</title>
</head>
<div class="btn-group">
<form action="index.html" method="get">
  <button>Login log</button>
</form>
<form action="cmd.html" method="get">
  <button>Command log</button>
</form>
<form action="whois.html" method="get">
  <button>Who is lookup</button>
</form>
</div>

<h1>Who is lookup</h1>
<p>But where are our guests coming from?</p>
<table align="right" border="1" bordercolor="LimeGreen" cellspacing="0" cellpadding="1"
 style="margin-right:40px; table-layout:fixed;vertical-align:bottom;font-size:14px;font-family:verdana,sans,sans-serif;border-collapse:collapse;border:2px solid LimeGreen">
<iframe src="https://www.ultratools.com/tools/ipWhoisLookup"></iframe>
<tr>
  <td align="left" style="padding:1px 4px">
    <b>[Recent IPs]</b>
  </td>
</tr>
"""
def makehtml():
	cursor = connector.cursor()
	cursor.execute(query)
	html = base
	for session in cursor:
                ip = str(session[0])
                info = "<tr><td>" + ip
                html = html + info
	html = html + "</html>"
	cursor.close()
	return html

while True:
	time.sleep(5)
	html = makehtml()
	with open(file_path, 'w') as f:
		f.write(html)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
