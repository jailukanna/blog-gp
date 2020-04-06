---
title: mysql example simple repository (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'simple repository'

Functions in program: 
* `def get_images():`
* `def update(color: Color):`
* `def update_most_similar_tone_id(id: int, most_similar_tone_index: int):`
* `def update_color_index(id: int, color_index: int):`
* `def to_object(row) -> Color:`
* `def get_images_with_empty_hsv():`
* `def get_colors_interior():`
* `def get_colors_product():`
* `def save(row):`
* `def find_all():`
* `def get_connection():`

Modules used in program: 
* `import os`
* `import mysql.connector`

## python simple repository

Python mysql example: simple repository

```python
# ! /usr/bin/env python
# -*- coding: utf-8 -*-
from collections import defaultdict

from dotenv import load_dotenv
import mysql.connector
from mysql.connector import Error
import os

from entities.color import Color
from collections import defaultdict


load_dotenv()


def get_connection():
    return mysql.connector.connect(host=os.getenv('DB_HOST'),
                                   database=os.getenv('DB_DATABASE'),
                                   user=os.getenv('DB_USERNAME'),
                                   password=os.getenv('DB_PASSWORD'),
                                   use_pure=True)


def find_all():
    cnx = get_connection()
    try:
        sql_select_Query = """SELECT c.id, c.sku, c.type, c.color, c.weight, c.hsv 
                              FROM `colors` AS c 
                            """
        cursor = cnx.cursor(dictionary=True)
        cursor.execute(sql_select_Query)
        records = cursor.fetchall()

        print("Total number of rows in matches is - ", cursor.rowcount)
        cursor.close()
        return records
    except Error as e:
        print("Error while connecting to MySQL", e)
    finally:
        if (cnx.is_connected()):
            cnx.close()

def save(row):
    mydb = get_connection()
    try:

        mycursor = mydb.cursor()

        sql = "INSERT INTO colors (sku, type, color, weight) VALUES (%s, %s, %s, %s)"
        val = (row["sku"], row["type"], row["color"], row["weight"])
        mycursor.execute(sql, val)

        mydb.commit()
    except Error as e:
        print("Error while connecting to MySQL", e)
    finally:
        # closing database connection.
        if (mydb.is_connected()):
            mydb.close()
            print("MySQL connection is closed")


def get_colors_product():
    cnx = get_connection()
    try:
        sql_select_Query = """SELECT c.sku, c.type, c.color, c.weight 
                              FROM `colors` AS c 
                              WHERE c.type = 'product' 
                              ORDER BY c.sku ASC
                              LIMIT 2000
                            """
        cursor = cnx.cursor(dictionary=True)
        cursor.execute(sql_select_Query)
        records = cursor.fetchall()
        result = {}

        for idx, data in enumerate(records):
            result.setdefault(data['sku'], {})
            result[data['sku']][idx] = data['color'].replace('  ', ' ').replace('[', '').replace(']', '').split(' ')

        print("Total number of rows in matches is - ", cursor.rowcount)
        cursor.close()
        return result
    except Error as e:
        print("Error while connecting to MySQL", e)
    finally:
        if (cnx.is_connected()):
            cnx.close()
            print("MySQL connection is closed")


def get_colors_interior():
    cnx = get_connection()
    try:
        sql_select_Query = """SELECT c.sku, c.type, c.color, c.weight 
                              FROM `colors` AS c 
                              WHERE c.type = 'interior' 
                              ORDER BY c.sku ASC
                              LIMIT 2000
                            """
        cursor = cnx.cursor(dictionary=True)
        cursor.execute(sql_select_Query)
        records = cursor.fetchall()
        result = {}

        for idx, data in enumerate(records):
            result.setdefault(data['sku'], {})
            result[data['sku']][idx] = data['color'].replace('  ', ' ').replace('[', '').replace(']', '').split(' ')

        print("Total number of rows in matches is - ", cursor.rowcount)
        cursor.close()
        return result
    except Error as e:
        print("Error while connecting to MySQL", e)
    finally:
        if (cnx.is_connected()):
            cnx.close()
            print("MySQL connection is closed")


def get_images_with_empty_hsv():
    cnx = get_connection()
    try:
        sql_select_Query = """
                              SELECT c.id, c.sku, c.type, c.color, c.weight, c.hsv 
                              FROM `colors` c 
                              WHERE c.hsv IS NULL
                           """
        cursor = cnx.cursor(dictionary=True)
        cursor.execute(sql_select_Query)
        records = cursor.fetchall()
        result = []

        for idx, data in enumerate(records):
            result.append(to_object(data))

        print("Total number of rows in matches is - ", cursor.rowcount)
        cursor.close()
        return result
    except Error as e:
        print("Error while connecting to MySQL", e)
    finally:
        if cnx.is_connected():
            cnx.close()
            print("MySQL connection is closed")

def to_object(row) -> Color:
    return Color(row['id'], row['sku'], row['type'], row['color'], row['weight'], row['hsv'])

def update_color_index(id: int, color_index: int):
    db = get_connection()

    try:
        cursor = db.cursor()

        query = """UPDATE colors SET color_index = %s WHERE id = %s"""
        cursor.execute(query, (color_index, id))
        db.commit()
        cursor.close()
    except Error as e:
        print("Error while connecting to MySQL", e)
    finally:
        if db.is_connected():
            db.close()

def update_most_similar_tone_id(id: int, most_similar_tone_index: int):
    db = get_connection()

    try:
        cursor = db.cursor()

        query = """UPDATE colors SET most_similar_tone_index = %s WHERE id = %s"""
        cursor.execute(query, (most_similar_tone_index, id))
        db.commit()
        cursor.close()
    except Error as e:
        print("Error while connecting to MySQL", e)
    finally:
        if db.is_connected():
            db.close()

def update(color: Color):
    db = get_connection()

    try:
        cursor = db.cursor()

        query = """UPDATE colors SET type = %s, color = %s, weight = %s, hsv = %s, sku = %s WHERE id = %s"""
        cursor.execute(query, (color.type, color.color, color.weight, color.hsv, color.sku, color.id))
        db.commit()
        cursor.close()
    except Error as e:
        print("Error while connecting to MySQL", e)
    finally:
        if db.is_connected():
            db.close()

def get_images():
    cnx = get_connection()
    try:
        sql_select_Query = """SELECT c.sku, c.type, c.color, c.weight, i.image
                              FROM `colors` AS c
                              INNER JOIN `MY_TABLE` AS i 
                              ON c.sku = i.sku
                              ORDER BY c.sku ASC
                              LIMIT 4000
                            """
        cursor = cnx.cursor(dictionary=True)
        cursor.execute(sql_select_Query)
        records = cursor.fetchall()
        result = {}

        for idx, data in enumerate(records):
            result.setdefault(data['sku'], {})
            result[data['sku']] = data['image']

        print("Total number of rows in matches is - ", cursor.rowcount)
        cursor.close()
        return result
    except Error as e:
        print("Error while connecting to MySQL", e)
    finally:
        if (cnx.is_connected()):
            cnx.close()
            print("MySQL connection is closed")


# this means that if this script is executed, then
# main() will be executed
if __name__ == '__main__':
    save()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
