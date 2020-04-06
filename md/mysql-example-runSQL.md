---
title: mysql example runSQL (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'runSQL'


Modules used in program: 
* `import re`
* `import random`
* `import mysql.connector`

## python runSQL

Python mysql example: runSQL

```python
import mysql.connector
import random
import re

class runSQL():
    def __init__(self):
        self.__mydb = mysql.connector.connect(host = self.__myhost, user = self.__myuser, passwd = self.__mypasswd, database = self.__dbname)

    # return 0 = "OK" 1 = "add user error" 2 = "username exist" 3 = "username or password error"
    def newUser(self, username, password):
        try:
            if self.__validStr(username + password) and len(username) <= 16 and len(password) <= 16:
                print("password OK")
                mycursor = self.__mydb.cursor()
                # username not exist
                sqlCommand = "SELECT COUNT(username) FROM userAccount WHERE username='%s';" % (username)
                mycursor.execute(sqlCommand)
                myresult = mycursor.fetchone()
                if int(myresult[0]) != 0:
                    return 2 # username exist
                print("username OK")
                # generate not exist ID
                while True:
                    ID = self.randStr()
                    sqlCommand = "SELECT COUNT(ID) FROM userAccount WHERE ID='%s';" % (ID)
                    mycursor.execute(sqlCommand)
                    myresult = mycursor.fetchone()
                    if int(myresult[0]) == 0:
                        break
                print("ID OK")
                # add account
                sqlCommand = "INSERT INTO userAccount(ID, username, password) VALUES(%s, %s, %s);"
                values = (ID, username, password)
                mycursor.execute(sqlCommand, values)
                self.__mydb.commit()
                return 0
            else:
                return 3
        except:
            return 1

    def randStr(self, size = 16):
        s = ""
        letters = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789'
        for i in range(0, size):
            s += letters[random.randint(0, len(letters) - 1)]
        return s

    def randNumStr(self, size = 5):
        n = ''
        num = '0123456789'
        n += num[random.randint(1, len(num) - 1)]
        for i in range(1, size):
            n += num[random.randint(0, len(num) - 1)]
        return n

    def __validStr(self, inputStr = ""):
        if re.match('^[A-Za-z0-9]*$', inputStr):
            return True
        else:
            return False

    __mydb = 0
    __myhost = "localhost"
    __myuser = "username"
    __mypasswd = "passwd"
    __dbname = "project1"

if __name__ == "__main__":
    r = runSQL()
    print(r.newUser("test" + r.randNumStr(3), "test123"))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
