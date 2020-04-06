---
title: mysql example CollectionOne (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'CollectionOne'

Functions in program: 
* `def main():`
* `def processFile(workingFile, file_folder, file_name, sqlConnection):`
* `def dbConnection():`
* `def getFiles(currentWD):`

Modules used in program: 
* `import mysql.connector`
* `import re`
* `import os`

## python CollectionOne

Python mysql example: CollectionOne

```python
import os
import re
import mysql.connector
from mysql.connector import Error

def getFiles(currentWD):
    filesToRead = []
    for dirname, dirnames, filenames in os.walk(currentWD):
        for filename in filenames:
            # ignore some files we don't care about
            excludeList = ['errors', '.DS_Store', '._']
            if any(item in filename for item in excludeList):
                pass
            else:
                filePath = os.path.join(dirname, filename)
                filesToRead.append(filePath)
    return filesToRead

def dbConnection():
    # Connect to a MySQL instance
    try:
        print("[*] Establishing Connection to your MySQL Server...\n")

        connection = mysql.connector.connect(host='localhost',
                                             database='CollectionOne',
                                             user='root',
                                             password='4ZaCtr1wfY0M4zo')

        if connection.is_connected():
            dbInfo = connection.get_server_info()
            print("[*] You are connected to your MySQL Server. This server is running MySQL version: ", dbInfo)
            cursor = connection.cursor()
            cursor.execute("select database();")
            record = cursor.fetchone()
            print("[*] You are connected to database: ", record)

    except Error as e:
        print("[!] Error while connecting to MySQL - ", e)

    return connection

def processFile(workingFile, file_folder, file_name, sqlConnection):
# Process each file in the working directory

    # Assign some needed variables to capture the data being passed from other functions (probably not needed)
    connection = sqlConnection

    fileFolder = file_folder
    fileName = file_name

    # Open each file
    with open(workingFile) as workingFile:
        workingFile = workingFile.readlines()

    for line in workingFile:
        # Run some checks to see if the record being read is an email/password combo or a username/password combo

        # A simple check to see if the line may contain an email address (probably a better way to do this)
        if '@' in line:
            try:
                # strip out any whitespace and newline characters
                line.strip()

                # split the email and passwords by known delimiters
                splitLine = re.split(":|;|\|", line)

                # assigning each `part` to a variable and stripping again (had an issue where passwords were including
                # return characters (probably a better way to do this)
                email = splitLine[0].strip()
                password = splitLine[1].strip()

                # stripping the email username and domain from the email address and assigning each to a variable
                emailANDdomain = email.split('@')
                userName = emailANDdomain[0].strip()
                domain = emailANDdomain[1].strip()


                cursor = connection.cursor()

                # build your query
                insertQuery = """ INSERT INTO `emails_and_passwords`
                                          (`email`, `password`, `username`, `domain`, `folderName`, `fileName`) 
                                          VALUES (%s,%s,%s,%s,%s,%s)"""

                # assign appropriate variables to your query
                insertTuple = email, password, userName, domain, fileFolder, fileName

                # push and commit changes to the MySQL database
                result = cursor.execute(insertQuery, insertTuple)
                connection.commit()

            except Exception as error:
                # Handle any errors and roll back changes to the database to prevent corruption
                connection.rollback()
                print("[!] (PROCESSED AS EMAIL) Failed to insert record into MySQL database {}".format(error))

                # save any errors caught to a text file for each folder processed - This will be used to improve the
                # script at a later date
                textFileName = fileFolder + '_errors.txt'
                with open(textFileName, 'a') as errors:
                    # indicate the record was processed in the email statement
                    errors.write("[PROCESSED AS EMAIL] - "+line)
                    errors.close()
                # Keep it moving
                pass

        # if the above statement fails, process as a username/password combo
        else:
            try:
                # strip out any whitespace and newline characters
                line.strip()

                # split the email and passwords by known delimiters
                splitLine = re.split(":|;|\|", line)

                # assigning each `part` to a variable and stripping again (had an issue where passwords were including
                # return characters (probably a better way to do this)
                username = splitLine[0].strip()
                password = splitLine[1].strip()

                cursor = connection.cursor()

                # build your query
                insertQuery = """ INSERT INTO `usernames_and_passwords`
                                          (`username`, `password`, `folderName`, `fileName`) 
                                          VALUES (%s,%s,%s,%s)"""

                # assign appropriate variables to your query
                insertTuple = username, password, fileFolder, fileName

                # push and commit changes to the MySQL database
                result = cursor.execute(insertQuery, insertTuple)
                connection.commit()

            except Exception as error:
                # Handle any errors and roll back changes to the database to prevent corruption
                connection.rollback()
                print("[!] (PROCESSED AS U/N) Failed to insert record into MySQL database {}".format(error))

                # save any errors caught to a text file for each folder processed - This will be used to improve the
                # script at a later date
                textFileName = fileFolder + '_errors.txt'
                with open(textFileName, 'a') as errors:
                    # indicate the record was processed in the username/password statement
                    errors.write("[PROCESSED AS U/N] - "+line)
                    errors.close()
                # Keep it moving
                pass


def main():
    # Throw this script in the directory you want or use the below os.chdir to specify the directory you want to look
    # in (just make sure to comment line 148 out if you move the script to your working directory
    os.chdir('/Volumes/StorageDriv/CollectionOne')
    currentWD = os.getcwd()

    # Pass the current working directory to function to get a list of files to process and assign it to a variable
    filesToProcess = getFiles(currentWD)

    # Useful to see if you are looking at the right directory
    print("[****] Here are the files found for processing: [****]\n", filesToProcess)

    # Establish a variable to pass later on for the database connection
    sqlConnection = dbConnection()

    for file in filesToProcess:
        # For each file in the working directory - process each file
        print("[****] Processing file: ", file)

        # Get the file folder and file name for each file processed - assign to a variable to pass to on to the
        # processing function
        str_split = file.split('/')
        file_folder = str_split[len(str_split)-2]
        file_name =str_split[len(str_split)-1]

        # Invoke the processing function - Pass all needed data
        processFile(file, file_folder, file_name, sqlConnection)
main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
