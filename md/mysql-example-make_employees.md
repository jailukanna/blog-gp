---
title: mysql example make employees (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'make employees'

Functions in program: 
* `def main():`
* `def genstring(lim_down=3, lim_up=9):`
* `def rnd_user(num=1000001, threadid=1):`
* `def create_table():`

Modules used in program: 
* `import threading`
* `import mysql.connector`
* `import string`
* `import time`
* `import random`
* `import os, sys`

## python make employees

Python mysql example: make employees

```python
#!/usr/bin/python

import os, sys
import random
import time
import string
import mysql.connector
import threading

from mysql.connector import errorcode
from mysql.connector.errors import Error

# For use in signaling
shutdown_event = threading.Event()

# Set username, password, etc
dbconfig = {
  "user":"imdb",
  "password":"imdb",
  "database":"imdb"
}  

def create_table():
  query = u"""CREATE TABLE IF NOT EXISTS imdb.employees (
    id INT NOT NULL AUTO_INCREMENT,
    fname VARCHAR(30),
    lname VARCHAR(30),
    hired DATE NOT NULL DEFAULT '1970-01-01',
    separated DATE NULL,
    job_code CHAR(3) NOT NULL,
    store_id INT NOT NULL,
    PRIMARY KEY (id))"""

  cnx = mysql.connector.connect(**dbconfig)
  cursor = cnx.cursor()
  cursor.execute(query)
  cursor.close()
  cnx.close()
  print("Created table")

def rnd_user(num=1000001, threadid=1):
  query = u"INSERT INTO imdb.employees (fname, lname, hired, job_code, store_id) VALUES ('%(fname)s','%(lname)s','%(hired)s','%(jobcode)s','%(storeid)s');"
  cnx = mysql.connector.connect(**dbconfig)
  cnx.autocommit = True
  cursor = cnx.cursor()

  def rnd_date():
    return time.strftime("%Y-%m-%d", (random.randrange(2000,2016), random.randrange(1,12), random.randrange(1,28), 0, 0, 0, 0, 1, -1))

  for x in range(num):
    if not shutdown_event.is_set():
      fname = genstring(3, 9)
      lname = genstring(4, 12)
      hired = rnd_date()
      jobcode = genstring(3, 3).upper()
      storeid = random.randrange(1, 20)

      cursor.execute(query % {u'fname': fname, u'lname': lname, u'hired': hired, u'jobcode': jobcode, u'storeid': storeid})

      if x % 1000 == 0:
        print("[%2d] Inserted %d rows" % (threadid, x))

  cnx.close()

def genstring(lim_down=3, lim_up=9):
  alpha = random.randint(lim_down, lim_up)
  vowels = ['a','e','i','o','u']
  consonants = [a for a in string.ascii_lowercase if a not in vowels]

  def a_part(slen):
    ret = ''
    for i in range(slen):
      if i%2 == 0:
        randid = random.randint(0,20) #number of consonants
        ret += consonants[randid]
      else:
        randid = random.randint(0,4) #number of vowels
        ret += vowels[randid]
    return ret

  fpl = alpha/2
  if alpha % 2 :
    fpl = int(alpha/2) + 1
  lpl = alpha - fpl
  
  start = a_part(fpl)
  end = a_part(lpl)
  
  return "%s%s" % (start.capitalize(), end)

def main():

  # Make sure user account works
  try:
    cnx = mysql.connector.connect(**dbconfig)
  except mysql.connector.Error as err:
    if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
      print("Something is wrong with your user name or password.")
    elif err.errno == errorcode.ER_BAD_DB_ERROR:
      print("Database does not exist")
    else:
      print("Other Error: %s" % err)
    os._exit(1)
  else:
    cnx.close()

  # Create the table
  create_table()

  # Hold threads
  threads = []
  threadId = 1

  # Loop/create/start threads
  for x in range(8):
    t = threading.Thread(target=rnd_user, args=(125000,threadId,))
    t.start()
    threads.append(t)
    threadId += 1
  
  print("Waiting for threads to complete...")

  try:
    for i in threads:
      i.join(timeout=1.0)
  except (KeyboardInterrupt, SystemExit):
    print("Caught Ctrl-C. Cleaning up. Exiting.")
    shutdown_event.set()

main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
