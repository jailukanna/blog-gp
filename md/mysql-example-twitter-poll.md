---
title: mysql example twitter-poll (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'twitter-poll'

Functions in program: 
* `def get_category():`

Modules used in program: 
* `import random`
* `import mysql.connector`
* `import sys`
* `import json`

## python twitter-poll

Python mysql example: twitter-poll

```python
# Original: https://paste.wuffs.org/raw/160201.220633.4garttu6

from twitter import *
import json
import sys
import mysql.connector
import random

conn = mysql.connector.connect(user='wikistuff', password='ffutsikiw', unix_socket='/var/run/mysqld/mysqld.sock', database='wikistuff')
c = conn.cursor()

def get_category():
	category_count = 7743729
	category_id = random.randint(0, category_count)
	c.execute('select cl_to from categorylinks limit %d,1' % category_id)
	category_name = c.fetchone()[0].decode('utf-8')

	c.execute('select page_title from categorylinks left join page on cl_from = page_id where cl_to = %s and length(page_title) <= 20 and page_namespace = 0 order by rand() limit 4', (category_name,))
	pages = [x[0].decode('utf-8').replace('_',' ') for x in c]

	return (category_name.replace('_',' '), pages)


while True:
	name, pages = get_category()
	if 'stubs' in name:
		continue
	lname = name.lower()
	if 'deaths' in lname or 'murder' in lname:
		continue
	if len(pages) == 4:
		break


class SpoofOAuth(OAuth):
	def __init__(self, *args, **kwargs):
		OAuth.__init__(self, *args, **kwargs)

	def generate_headers(self):
		# note: the X-Twitter fields may not actually be necessary
		hdr = {
			'User-Agent': 'Twitter-iPhone/6.45 iOS/9.0.2 (Apple;iPhone8,2;;;;;1)',
			'X-Twitter-Client': 'Twitter-iPhone',
			'X-Twitter-API-Version': '5',
			'X-Twitter-Client-Language': 'en',
			'X-Twitter-Client-Version': '6.45',
			}
		return hdr

ACCESS_TOKEN = '[redacted]'
ACCESS_TOKEN_SECRET = '[redacted]'
IPHONE_CONSUMER_KEY = 'IQKbtAYlXLripLGPWd0HUA'
IPHONE_CONSUMER_SECRET = '[redacted]'

auth = SpoofOAuth(ACCESS_TOKEN, ACCESS_TOKEN_SECRET, IPHONE_CONSUMER_KEY, IPHONE_CONSUMER_SECRET)

api = Twitter(auth=auth)
caps_api = Twitter(domain='caps.twitter.com', api_version='v2', auth=auth)

card = {
'twitter:string:choice1_label': pages[0],
'twitter:string:choice2_label': pages[1],
'twitter:string:choice3_label': pages[2],
'twitter:string:choice4_label': pages[3],
'twitter:long:duration_minutes': 1440,
'twitter:api:api:endpoint': '1',
'twitter:card': 'poll4choice_text_only',
}

# https://caps.twitter.com/v2/cards/create.json?card_data=...&send_error_codes=1
card_data = caps_api.cards.create(card_data=json.dumps(card), send_error_codes=1)
print(repr(card_data))

tweet_text = '[Poll] Today we\'re ranking %s. What\'s your pick? Cast your vote below!' % name

print(tweet_text)
print(json.dumps(card))

# https://api.twitter.com/1.1/statuses/update.json?status=...&card_uri=...&cards_platform=iPhone-13&include_cards=1
api.statuses.update(status=tweet_text, card_uri=card_data['card_uri'], cards_platform='iPhone-13', include_cards=1)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
