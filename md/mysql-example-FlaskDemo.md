---
title: mysql example FlaskDemo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'FlaskDemo'


## python FlaskDemo

Python mysql example: FlaskDemo

```python
from flask import Flask
from flask_restful import reqparse, abort, Api, Resource
from flask.ext.mysql import MySQL

mysql = MySQL()
app = Flask(__name__)

app.config['MYSQL_DATABASE_USER'] = 'your_db_user'
app.config['MYSQL_DATABASE_PASSWORD'] = 'your_db_password'
app.config['MYSQL_DATABASE_DB'] = 'FlaskDemo'
app.config['MYSQL_DATABASE_HOST'] = 'localhost'
app.config['MYSQL_DATABASE_PORT'] = your_db_port


mysql.init_app(app)
api = Api(app)

class HelloWorld(Resource):
    def get(self):
        return {'Message': 'Hello World'}

class CreateNewPokemon(Resource):
	def post(self):
			parser = reqparse.RequestParser()
			parser.add_argument('name', type=str, help='Name to create Pokemon name')
			parser.add_argument('cp', type=str, help='Cp to create cp of Pokemon')
			args = parser.parse_args()

			conn = mysql.connect()
			cursor = conn.cursor()
			cursor.callproc('spCreatePokemon',(args['name'],args['cp']))
			data = cursor.fetchall()

			if len(data) is 0:
				conn.commit()
				return {'Message': 'Pokemon creation success'}
			else:
				return {'Message': 'Fail'}


api.add_resource(HelloWorld, '/HelloWorld')
api.add_resource(AddNewName, '/AddNewName')
api.add_resource(CreateNewPokemon, '/CreateNewPokemon')

if __name__ == '__main__':
    app.run(debug=True)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
