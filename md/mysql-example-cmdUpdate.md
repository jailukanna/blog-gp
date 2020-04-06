---
title: mysql example cmdUpdate (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'cmdUpdate'

Functions in program: 
* `def makehtml():`

Modules used in program: 
* `import mysql.connector`
* `import time`

## python cmdUpdate

Python mysql example: cmdUpdate

```python
#! /usr/bin/python2.7

import time
import mysql.connector
read_data = ""
file_path ="/var/www/html/cmd.html"
connector = mysql.connector.connect(user='cowrie',
				    password='Kaltopmih',
				    host='127.0.0.1',
				    database='cowrie',
				    autocommit=True)

query="select ip, input, timestamp from sessions, input where sessions.id = session order by timestamp DESC LIMIT 300;"
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
         color: LimeGreen;
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
<!--<meta http-equiv="refresh" content="5">-->
<head>
  <title>Commands Log</title>
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

<h1>Honeypot command log</h1>
<p>But what on earth are our guests trying to do? Find out below!</p>
<table align="center"  border="1" bordercolor="LimeGreen" cellspacing="0" cellpadding="1"  style="table-layout:fixed;width:75%;vertical-align:bottom;font-size:14px;font-family:verdana,sans,sans-serif;border-collapse:collapse;border:2px solid LimeGreen">
<tr>
  <td align="left" style="padding:1px 4px" width="11%" >
    <b>[IP]</b>
  </td>
  <td width="%">
    <b>[COMMAND]</b>
  </td>
  <td width="13%">
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
                command = (session[1]).encode('ascii', 'ignore').decode('ascii')
                date = str(session[2])
                info = "<tr><td>" + ip + \
                       "</td><td>" + command + \
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
