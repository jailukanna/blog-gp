---
title: mysql example get old tweets (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'get old tweets'


Modules used in program: 
* `import json`
* `import mysql.connector as mysql`

## python get old tweets

Python mysql example: get old tweets

```python
#!/usr/bin/env python

"""
Fetch historic tweets containing a given hashtag and insert them into a database
Settings in config.json
"""

from twitterscraper import query_tweets
import mysql.connector as mysql
from datetime import datetime
import json

# load credentials
try:
    with open('config.json') as f:
        config = json.load(f)
except Exception as e:
    print(e)


print('####################################################\n#')
print('# Fetch historic tweets for #{} !\n#'.format(config['hashtag']))
print('####################################################')

# create db connection
try:
    db = mysql.connect(
        host=config['db_host'],
        user=config['db_user'],
        passwd=config['db_passwd'],
        database=config['db_name']
    )

except mysql.Error:
    print("Error connecting to Database")
    quit()

# create cursor instance
cursor = db.cursor()

# create table if it dosen't exist
cursor.execute(f"""CREATE TABLE IF NOT EXISTS `{config['db_table']}` (
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `user_id` bigint(20) NOT NULL,
    `tweet_id` bigint(20) NOT NULL,
    `tweet` mediumtext NOT NULL,
    `timestamp` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB  DEFAULT CHARSET=latin1 AUTO_INCREMENT=3 ;
""")

db.commit()

# Get tweets
tweets = query_tweets(config['hashtag'], 100000)
# todo move limit to config

print("Scraped {} tweets containing #{}\n".format(
    len(tweets), config['hashtag']))

# define the query
query = 'INSERT INTO {} (id, user_id, tweet_id, tweet, timestamp) VALUES (NULL, %s, %s, %s, %s)'.format(
    config['db_table'])


for t in tweets:
    # only save if the string is a hashtag not just a text string
    if('#{}'.format(config['hashtag']) in t.text.lower()):

        print('Inserting tweet {} from {} ( @{} )'.format(
            t.user_id, t.tweet_id, t.username, t.screen_name))

        # convert the timestamp in order to allow it to be JSON encoded
        t.timestamp = datetime.strftime(t.timestamp, '%Y-%m-%d %H:%M:%S')
        values = (t.user_id, t.tweet_id, json.dumps(t.__dict__), t.timestamp)

        # execute the query
        cursor.execute(query, values)

# commit to db
db.commit()

print(cursor.rowcount, "record inserted")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
