---
title: mysql example indexUpdate (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'indexUpdate'

Functions in program: 
* `def makehtml():`

Modules used in program: 
* `import mysql.connector`
* `import time`

## python indexUpdate

Python mysql example: indexUpdate

```python
#! /usr/bin/python2.7

import time
import mysql.connector
read_data = ""
file_path ="/var/www/html/index.html"
connector = mysql.connector.connect(user='cowrie',
				    password='Kaltopmih',
				    host='127.0.0.1',
				    database='cowrie',
				    autocommit=True)

query="select ip, username, password, timestamp from sessions, auth where sessions.id = session order by timestamp DESC LIMIT 300;"
base ="""<html>
<style>
.btn-group button {
    background-color: LimeGreen;
    border: 1px solid green;
    color: black;
    padding: 10px 24px;
    cursor: pointer;
    float:left;
	position:relative;
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
<meta http-equiv="refresh" content="5">
<head>
  <title>The Se7en honeypot</title>
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

<h1>Honeypot statistics</h1>
<p>The IP:s, credentials and login times of our honorable guests.</p>
<table align="center" border="1" bordercolor="LimeGreen" cellspacing="0" cellpadding="1" style="table-layout:fixed;vertical-align:bottom;font-size:14px;font-family:verdana,sans,sans-serif;border-collapse:collapse;border:2px solid LimeGreen">
<tr>
  <td align="left" style="padding:1px 4px">
    <b>[IP]</b>
  </td>
  <td>
    <b>[USERNAME]</b>
  </td>
  <td>
    <b>[PASSWORD]</b>
  </td>
  <td>
    <b>[TIME]</b>
  </td>
</tr>
"""
def makehtml():
	cursor = connector.cursor()
	cursor.execute(query)
	html = base
	for session in cursor:
                ip = str(session[0])
                login = (session[1]).encode('ascii', 'ignore').decode('ascii')
                password = (session[2]).encode('ascii', 'ignore').decode('ascii')
                date = str(session[3])
		if login == 'root' and password == '1234':
                	info = "<tr><td>" + ip + \
                       		"</td><td><font color='red'>" + login + \
                       		"</font></td><td><font color='red'>" + password + \
                         	"</font></td><td>" + date + "</td></tr>\n"
		else:
                        info = "<tr><td>" + ip + \
                                "</td><td>" + login + \
                                "</td><td>" + password + \
                                "</td><td>" + date + "</td></tr>\n"

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
