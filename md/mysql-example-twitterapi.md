---
title: mysql example twitterapi (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'twitterapi'

Functions in program: 
* `def connect(username, created_at, tweet, retweet_count, place , location):`

Modules used in program: 
* `import subprocess`
* `import os`
* `import time`
* `import json`
* `import tweepy`
* `import mysql.connector`

## python twitterapi

Python mysql example: twitterapi

```python
import mysql.connector
from mysql.connector import Error
import tweepy
import json
from dateutil import parser
import time
import os
import subprocess

# replace pw1 through pw4 with your keys and tokens
consumer_key = 'pw1'
consumer_secret = 'pw2'
access_token = 'pw3'
access_token_secret = 'pw4'


def connect(username, created_at, tweet, retweet_count, place , location):
	"""
	connect to MySQL database and insert twitter data
	"""
	try:
		con = mysql.connector.connect(host = 'localhost',
		database='twitterdb', user='root', password = 'PASSWORD', charset = 'utf8')


		if con.is_connected():
			"""
			Insert twitter data
			"""
			cursor = con.cursor()

			query = "INSERT INTO twitterapi (username, created_at, tweet, retweet_count,place, location) VALUES (%s, %s, %s, %s, %s, %s)"
			cursor.execute(query, (username, created_at, tweet, retweet_count, place, location))
			con.commit()
			cursor.close()
			con.close()

	except Error as e:
		print(e)



# Tweepy class to access Twitter API
class Streamlistener(tweepy.StreamListener):


	def on_connect(self):
		print("You are connected to the Twitter API")


	def on_error(self):
		if status_code != 200:
			print("error found")
			# returning false disconnects the stream
			return False

	"""
	This method reads in tweet data as Json
	and extracts the data we want.
	"""
	def on_data(self,data):

		try:
			raw_data = json.loads(data)

			if 'text' in raw_data:

				username = raw_data['user']['screen_name']
				created_at = parser.parse(raw_data['created_at'])
				tweet = raw_data['text']
				retweet_count = raw_data['retweet_count']

				if raw_data['place'] is not None:
					place = raw_data['place']['country']
					print(place)
				else:
					place = None


				location = raw_data['user']['location']

				#insert data just collected into MySQL database
				connect(username, created_at, tweet, retweet_count, place, location)
				print("Tweet colleted at: {} ".format(str(created_at)))
		except Error as e:
			print(e)


if __name__== '__main__':

	# authentification so we can access twitter
	auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
	auth.set_access_token(access_token, access_token_secret)
	api =tweepy.API(auth, wait_on_rate_limit=True)

	# create instance of Streamlistener
	listener = Streamlistener(api = api)
	stream = tweepy.Stream(auth, listener = listener)

	track = ['rock climbing', 'mountaineering', 'ice climbing', 'bouldering', 'sport climbing']
	#track = ['nba', 'cavs', 'celtics', 'basketball']
	# choose what we want to filter by
	stream.filter(track = track, languages = ['en'])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
