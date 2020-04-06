---
title: mysql example test dovi kulina (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'test dovi kulina'

Functions in program: 
* `def delete_user(id):`
* `def update_user():`
* `def edit_view(id):`
* `def users():`
* `def add_user():`

## python test dovi kulina

Python mysql example: test dovi kulina

```python
from tables import Results
from db_config import mysql
from flask import request, Flask
		
@app.route('/add', methods=['POST'])
def add_user():
    
	try:
        id = request.args.get('id')
        order_id = request.args.get('order_id')
        product_id = request.args.get('product_id')
        user_id = request.args.get('user_id')
        rating = request.args.get('rating')
        review = request.args.get('review')
		
		if id and order_id and product_id and user_id and rating and review and request.method == 'POST':
			
			sql = "INSERT INTO user_review(id, order_id, product_id, user_id, rating, review) VALUES(%s, %s, %s, %s, %s, %s)"
			data = (id, order_id, product_id, user_id, rating, review)
			conn = mysql.connect()
			cursor = conn.cursor()
			cursor.execute(sql, data)
			conn.commit()
			flash('User added successfully!')
			return redirect('/')
		else:
			return 'Error while adding user'
	except Exception as e:
		print(e)
	finally:
		cursor.close() 
		conn.close()
		
@app.route('/')
def users():
	try:
		conn = mysql.connect()
		cursor = conn.cursor(pymysql.cursors.DictCursor)
		cursor.execute("SELECT * FROM user_review")
		rows = cursor.fetchall()
		table = Results(rows)
		table.border = True
	except Exception as e:
		print(e)
	finally:
		cursor.close() 
		conn.close()

    return flask.jsonify(table)

@app.route('/edit/<int:id>')
def edit_view(id):
	try:
		conn = mysql.connect()
		cursor = conn.cursor(pymysql.cursors.DictCursor)
		cursor.execute("SELECT * FROM user_review WHERE id=%s", id)
		row = cursor.fetchone()
	except Exception as e:
		print(e)
	finally:
		cursor.close()
		conn.close()

@app.route('/update', methods=['POST'])
def update_user():
	try:
        id = request.args.get('id')
        order_id = request.args.get('order_id')
        product_id = request.args.get('product_id')
        user_id = request.args.get('user_id')
        rating = request.args.get('rating')
        review = request.args.get('review')

		if id and order_id and product_id and user_id and rating and review and request.method == 'POST':

			sql = "UPDATE user_review SET rating=%s, review=%s WHERE id=%s AND order_id=%s AND product_id=%s AND user_id=%s"
			data = (rating, review, id, order_id, product_id, user_id)
			conn = mysql.connect()
			cursor = conn.cursor()
			cursor.execute(sql, data)
			conn.commit()
			flash('User updated successfully!')
			return redirect('/')
		else:
			return 'Error while updating user'
	except Exception as e:
		print(e)
	finally:
		cursor.close() 
		conn.close()
		
@app.route('/delete/<int:id>')
def delete_user(id):
	try:
        id = request.args.get('id')
		conn = mysql.connect()
		cursor = conn.cursor()
		cursor.execute("DELETE FROM user_review WHERE id=%s", (id,))
		conn.commit()
		flash('User deleted successfully!')
		return redirect('/')
	except Exception as e:
		print(e)
	finally:
		cursor.close() 
		conn.close()
		
if __name__ == "__main__":
    app = Flask(__name__)
    app.run()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
