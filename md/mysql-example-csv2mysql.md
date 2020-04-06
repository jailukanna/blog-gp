---
title: mysql example csv2mysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'csv2mysql'

Functions in program: 
* `def update_table(filename):`
* `def update_csv(filename):`

Modules used in program: 
* `import csv`
* `import mysql.connector`

## python csv2mysql

Python mysql example: csv2mysql

```python
import mysql.connector
from mysql.connector import Error
import csv

""" These functions were for INSERTing into a table using a CSV column.

However, some of the fields needed for the INSERT had to be queried from another table, so this queries
the other table first and adds the needed info to the CSV, and then INSERTs into the
other table.

This has not been totally generalized, so for reuse, you have to fill in your own database config,
the column numbers for your columns in the csv, and your search and insert queries."""


""" Add column to CSV with the IDs for database table fields that need to be updated. """
def update_csv(filename):
    # open csv
    with open (filename, 'rb') as readfile:
        reader = csv.reader(readfile)

        new_filename = 'updated_' + filename
        with open(new_filename, 'wb') as writefile:

            writer = csv.writer(writefile)

            # copy headers from old file
            headers = next(reader, None)
            writer.writerow(headers)

            # for each row in the csvfile, query for needed values and update csv
            for row in reader:

                # get cells needed for query from csv
                mlid = row[0]

                # find the data needed for insert using the CSV cells
                search_query = """ SELECT resource
                                   FROM resource_data
                                   WHERE resource_type_field = 88 and value = """ + mlid

                # read database configuration
                try:
                    conn = mysql.connector.connect(host='localhost',
                                                    database='DBNAME',
                                                    user='USER',
                                                    password='PASSWORD')
                    cursor = conn.cursor(buffered = True)
                    cursor.execute(search_query)
                    dbrow = cursor.fetchone()
                    all_rsids = []

                    while dbrow is not None:
                        all_rsids.append(dbrow[0])
                        dbrow = cursor.fetchone()

                    # write the queried data out to a new copy of the csv
                    row.append(all_rsids)
                    writer.writerow(row)

                except Error as error:
                    print(error)

                finally:
                    cursor.close()
                    conn.close()

        writefile.close()
    readfile.close()

""" Update table in MySQL database using CSV """
def update_table(filename):
    # open csv
    with open (filename, 'rb') as readfile:
        reader = csv.reader(readfile)

        # skip headers from old file
        headers = next(reader, None)

        for row in reader:

            # get data from csv
            rsids = row[107].strip("[]").split(",") # column format is [1, 2, 3]
            zipcode = row[5]

            if zipcode != "":

                for rsid in rsids:
                    rsid = rsid.strip()
                    print(rsid + " - " + zipcode)

                    # insert data for ALL matching rsids
                    insert_query = """ INSERT INTO resource_data (resource, resource_type_field, value)
                                       VALUES (""" + rsid + ', 3, "' + zipcode + '")'

                    # read database configuration
                    try:
                        conn = mysql.connector.connect(host='localhost',
                                                        database='DATABASE',
                                                        user='USER',
                                                        password='PASSWORD')
                        cursor = conn.cursor(buffered = True)
                        cursor.execute(insert_query)
                        conn.commit()

                    except Error as error:
                        print(error)

                    finally:
                        cursor.close()
                        conn.close()

    readfile.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
