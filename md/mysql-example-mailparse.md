---
title: mysql example mailparse (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mailparse'


Modules used in program: 
* `import time`
* `import mysql.connector`
* `import time`
* `import os`

## python mailparse

Python mysql example: mailparse

```python
import os
import time
import mysql.connector
from validate_email import validate_email
import time

Server = mysql.connector.connect(host="127.0.0.1", user="root", passwd="root", db="pwdquery", port=8889)
Cursor = Server.cursor()

# Get a list of files to read 
fileCount = 1
for root, dirs, files in os.walk('/Volumes/dansku/tempDump/data'):
    for file in files:

        #Let's not import emails starting with symbols
        if file != "symbols":

            filePath = os.path.join(root, file)
            
            #Open File
            f = open(filePath, "r")

            #read all lines into a list
            emails = f.readlines()

            for email in emails:
                

                emailError = False

                try:
                    email = email.replace(" ", "").replace(";",":").rstrip()
                    emailAddress = email.split(":")[0]
                    password = email.split(":")[1]
                    password = password.replace('"', '\\"').replace("'", "\\'")

                except:
                    emailError = True
                    print("ERROR WITH " + email)
                
                if validate_email(emailAddress) is True and emailError is False:

                    fileCount += 1

                    print(str(fileCount) + " -> Saving: " + emailAddress + " : " + password)

                    #CHECK IF EMAIL EXISTS
                    Cursor.execute('''SELECT * FROM email WHERE email = "%s"''' % (emailAddress))
                    userInfo = Cursor.fetchall()

                    # IF EMAIL EXISTS
                    if len(userInfo) > 0:
                        for user in userInfo:
                            userID = user[0]

                        # NOT INSERT PASSWORD TWICE
                        Cursor.execute('''SELECT * FROM password WHERE email_id = "%s" AND password = "%s"''' % (userID, password))
                        passwordInfo = Cursor.fetchall()

                        # ONLY SAVE PASSWORD IF THAT PASSWORD AND USER_ID DOESNT EXIST
                        if len(passwordInfo) == 0:

                            # IF EMAIL ALREADY IN DATABASE, ONLY SAVE PASSWORD
                            Cursor.execute('''INSERT INTO password (email_id, password) values ("%d", "%s")''' % (userID, password))
                            Server.commit()
                    
                    else:

                        # IF EMAIL NOT IN DATABASE, ADD NEW EMAIL AND SAVE PASSWORD
                        Cursor.execute('''INSERT INTO email (email) values ("%s")''' % (emailAddress))
                        Server.commit()

                        Cursor.execute('''SELECT LAST_INSERT_ID()''')
                        lastID = Cursor.fetchall()

                        # Get recent user ID
                        for user in lastID:
                            userID = user[0]

                        # Finally save password to user
                        Cursor.execute('''INSERT INTO password (email_id, password) values ("%d", "%s")''' % (userID, password))
                        Server.commit()





           

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
