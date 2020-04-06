---
title: mysql example streaming streamer (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'streaming streamer'


Modules used in program: 
* `import mysql.connector`
* `import json`

## python streaming streamer

Python mysql example: streaming streamer

```python
from tweepy.streaming import StreamListener
from tweepy import OAuthHandler
from tweepy import Stream
import json
import mysql.connector



cnx = mysql.connector.connect(user='dennis', password='Focus8Strength',host='127.0.0.1', database='tweetsdb')
cursor = cnx.cursor()

access_token = "589487277-kF1RKMVP91zIuifdicCzfnT7OVXiF4CTpJKOOXfZ"
access_token_secret = "Fxz2P2YLImxzearST7KSDpTRukcwS0RZNqpmqbjAUMbkD"
consumer_key = "f5cX2EZb7j3mppoeUpVJlfS0I"
consumer_secret = "fpMwjSKqImz5gjjFBz4YkK7xyEgvt4taGnOGcRECplMseggO6r"


# This is a basic listener that just prints received tweets to stdout.
class StdOutListener(StreamListener):

    def on_data(self, data):
        tweet = json.loads(data)
        for word in tweet['text'].split():
            cursor.execute('INSERT INTO tweets (word,cnt) VALUES (%s, %s) ON DUPLICATE KEY UPDATE cnt = cnt+1', (word,1))
            cursor.execute('COMMIT')
        print(tweet['text'].split())
        return True

    def on_error(self, status):
        print(status)


if __name__ == '__main__':

    l = StdOutListener()
    auth = OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_token_secret)
    stream = Stream(auth, l)

    # This line filter Twitter Streams to capture data by the keywords: 'python', 'javascript', 'ruby'
    stream.filter(track=['python', 'javascript', 'ruby'])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
