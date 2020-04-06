---
title: mysql example MySQL (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'MySQL'

Functions in program: 
* `def main():`
* `def randomProduct():`
* `def avaragePrice():`
* `def MongoToMySQL(collection):`

Modules used in program: 
* `import random`
* `import mysql.connector`

## python MySQL

Python mysql example: MySQL

```python
import mysql.connector
from pymongo import MongoClient
import random

def MongoToMySQL(collection):
  for index, item in enumerate(collection.find()):
    if index < 50:
      sql = "INSERT INTO product (ProductID, Brand, Category, Price) VALUES (%s, %s, %s, %s)"
      values = (item["_id"],item["brand"],item["category"], item["price"]["selling_price"])
      mycursor.execute(sql,values)
    else:
      mydb.commit()
      break

def avaragePrice():
  totalPrice = 0
  mycursor.execute("SELECT Price FROM `product`")
  prices = mycursor.fetchall()
  for price in prices:
    totalPrice += price[0]
  return round(totalPrice/len(prices))

def randomProduct():
  mycursor.execute("SELECT * FROM `product`")
  importedProducts = mycursor.fetchall()
  randomNumber = random.randint(0, len(importedProducts))
  print("your random product is " + str(importedProducts[randomNumber]))
  currentProduct = importedProducts[0]
  for selectedProduct in importedProducts:
    if abs(importedProducts[randomNumber][3]-selectedProduct[3]) > abs(importedProducts[randomNumber][3] - currentProduct[3]):
      currentProduct = selectedProduct
  print("The product with the largest price difference is: " + str(currentProduct))

def main():
  print("(0) move products from mongo to mySQL")
  print("(1) get avarage price from mySQL")
  print("(2) random product and largest difference")
  number = input("Choose one option: ")
  if number == "0":
    MongoToMySQL(products)
  elif number == "1":
    print(avaragePrice())
  elif number == "2":
    randomProduct()
  else:
    print("Wrong input, enter again")
    main()

mydb = mysql.connector.connect(
  host="localhost",
  user="root",
  #passwd="root",
  port="3306",
  database="voordeelshop"
)
mycursor = mydb.cursor()

client = MongoClient()
products = client.voordeelshop.products

if __name__ == "__main__" :
  main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
