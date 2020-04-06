---
title: mysql example LibraryManagementSystem app (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'LibraryManagementSystem app'

Functions in program: 
* `def test():`
* `def test2():`
* `def main():`
* `def puppiesFunction():`

## python LibraryManagementSystem app

Python mysql example: LibraryManagementSystem app

```python
from flask import Flask, render_template, request
from flaskext.mysql import MySQL

app = Flask(__name__)
mysql = MySQL()

# MySQL configurations
app.config['MYSQL_DATABASE_USER'] = 'root'
app.config['MYSQL_DATABASE_PASSWORD'] = 'nikhil'
app.config['MYSQL_DATABASE_DB'] = 'libsys'
app.config['MYSQL_DATABASE_HOST'] = 'localhost'
mysql.init_app(app)

# Create the appropriate app.route functions,
# test and see if they work

# Make an app.route() decorator here for when the client sends the URI "/puppies"
@app.route('/puppies')
def puppiesFunction():
    return "Yes, puppies!"

@app.route("/")
def main():
    return render_template('index.html')

@app.route("/test2", methods = ['GET'])
def test2():
    json = request.args.getlist('searchitem')
    return "Yes!"

# Make another app.route() decorator here that takes in an integer named 'id' for when the client visits a URI like "/puppies/5"
@app.route('/test')
def test():
    conn = mysql.connect()
    cursor = conn.cursor()
    query = "select * from book"
    cursor.execute(query)
    data = cursor.fetchall()
    return "This method will act on the puppy with id %s" % id


if __name__ == '__main__':
    app.debug = True
    app.run(host='0.0.0.0', port=5000)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
