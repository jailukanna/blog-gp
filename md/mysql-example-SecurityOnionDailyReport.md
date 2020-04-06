---
title: mysql example SecurityOnionDailyReport (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'SecurityOnionDailyReport'


Modules used in program: 
* `import smtplib, ssl`
* `import numpy`
* `import mysql.connector`

## python SecurityOnionDailyReport

Python mysql example: SecurityOnionDailyReport

```python
import mysql.connector
import numpy
from datetime import datetime, timedelta

import smtplib, ssl
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

mydb = mysql.connector.connect(
  host="localhost",
  user="brandon",
  passwd="nope",
  database="securityonion_db"
)

#get time THIS IS PST FOR ALL YO PPL WHO WANT DIS EDIT as needed Security onion uses UTC by defualt 
CurrentTime = datetime.now() + timedelta(hours=-7)
TimeConvert = datetime.now() + timedelta(hours=-31)
#bullshit that dont work
#print(str(TimeConvert))
#'{:%H:%M:%S}'.format(TimeConvert)
#print((time.strftime("%Y-%m-%d %I:%M:%S")))


mycursor = mydb.cursor()

mycursor.execute("SELECT COUNT(*) AS cnt, signature, signature_id FROM event WHERE (priority=1 AND status=0 AND `timestamp` > \'" + str(TimeConvert) + "\') GROUP BY signature ORDER BY cnt DESC LIMIT 100")
myresult = mycursor.fetchall()

myarray = numpy.asarray(myresult)
#print((myarray))

count = len(myarray)
i = 0
s1 = """<table>
  <thead>
    <tr>
      <th>Count</th>
      <th>Alert</th>
      <th>Alert ID</th>
    </tr>
  </thead>
  <tbody>"""
s2 = ""
while i < count:
     a = myarray[i]
     s2 = s2 + """
    <tr>
      <td>""" + a[0] + """</td>
      <td>""" + a[1] + """</td>
      <td>""" + a[2] + """</td>
    </tr>"""
     i = i + 1
s3 = """  </tbody>
</table>"""

htmltxt = ("Summary of network attacks against the SakaServer network over the past 24 hours: <p> Logs from: <b>" + str(TimeConvert) + "</b> to <b>" + str(CurrentTime) + "</b> <p>" + s1 + s2 + s3)

print((htmltxt))


# EMAILER

sender_email = "nopesender@gmail.com"
receiver_email = "nope@gmail.com"
password = "nope"

message = MIMEMultipart("alternative")
message["Subject"] = "[SakaServer] Network Security Daily Report " + str(CurrentTime)
message["From"] = sender_email
message["To"] = receiver_email

# Create the plain-text and HTML version of your message
text = """\ """

html = str(htmltxt)

# Turn these into plain/html MIMEText objects
part1 = MIMEText(text, "plain")
part2 = MIMEText(html, "html")

# Add HTML/plain-text parts to MIMEMultipart message
# The email client will try to render the last part first
message.attach(part1)
message.attach(part2)

# Create secure connection with server and send email

s = smtplib.SMTP('smtp.gmail.com', 587) 
# start TLS for security 
s.starttls() 
# Authentication 
s.login(sender_email, password) 
# message to be sent 
message = message.as_string()
# sending the mail 
s.sendmail(sender_email, receiver_email, message) 
# terminating the session 
s.quit() 


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
