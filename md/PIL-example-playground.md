---
title: PIL example playground (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'playground'

Functions in program: 
* `def read_photos():`
* `def get_iata_to_city():`
* `def get_tweets():`

Modules used in program: 
* `import pandas as pd`
* `import PIL.ExifTags`
* `import PIL.Image`
* `import tweepy`
* `import matplotlib.ticker as tkr`
* `import matplotlib.pyplot as plt`
* `import re`
* `import json`
* `import glob`

## python playground

Python PIL example: playground

```python
from datetime import datetime, timedelta
import glob
import json
import re
import matplotlib.pyplot as plt
import matplotlib.ticker as tkr
import tweepy
import PIL.Image
import PIL.ExifTags
import pandas as pd
from . import classify_image


pd.set_option('display.max_colwidth', -1)
pd.set_option('display.max_columns', None)

TWITTER_CONSUMER_KEY = ''
TWITTER_CONSUMER_SECRET = ''
TWITTER_ACCESS_TOKEN = ''
TWITTER_ACCESS_TOKEN_SECRET = ''
USER_ID = '21653573'
MARKER = '✈'


def get_tweets():
    auth = tweepy.OAuthHandler(TWITTER_CONSUMER_KEY, TWITTER_CONSUMER_SECRET)
    auth.set_access_token(TWITTER_ACCESS_TOKEN, TWITTER_ACCESS_TOKEN_SECRET)
    api = tweepy.API(auth)
    cursor = tweepy.Cursor(api.user_timeline,
                           user_id=USER_ID,
                           exclude_replies='false',
                           include_rts='false',
                           count=200)
    return cursor.items()


# Get tweets about flights
all_tweets = pd.DataFrame(
    [(tweet.text, tweet.created_at) for tweet in get_tweets()],
    columns=['text', 'created_at'])
tweets_in_dates = all_tweets[
    (all_tweets.created_at > datetime(2018, 9, 8)) & (all_tweets.created_at < datetime(2018, 9, 30))]
flights_tweets = tweets_in_dates[tweets_in_dates.text.str.upper() == tweets_in_dates.text]

flights_tweets = flights_tweets.assign(start=lambda df: df.text.str.split(MARKER).str[0])
flights_tweets = flights_tweets.assign(finish=lambda df: df.text.str.split(MARKER).str[-1])

flights = flights_tweets[['start', 'finish', 'created_at']]
flights = flights.sort_values('created_at')


def get_iata_to_city():
    with open('airports.json') as f:
        data = json.load(f)
        return {airport['iata']: airport['city']
                for airport in data.values()
                if airport['iata']}

iata_to_city = get_iata_to_city()
iata_to_city['EZE'] = 'Buenos-Aires'

flights = flights.assign(
    start=flights.start.apply(lambda code: iata_to_city[re.sub(r'\W+', '', code)]),
    finish=flights.finish.apply(lambda code: iata_to_city[re.sub(r'\W+', '', code)]))

cities = flights.assign(
    spent=flights.created_at - flights.created_at.shift(1),
    city=flights.start,
    arrived=flights.created_at.shift(1),
)[["city", "spent", "arrived"]]
cities = cities.assign(left=cities.arrived + cities.spent)[cities.spent.dt.days > 0]


formatter = tkr.FuncFormatter(lambda x, _: str(timedelta(seconds=x / 1000000000)))

cities.plot(x="city", y="spent", kind="bar",
            legend=False, title='Cities') \
      .yaxis.set_major_formatter(formatter)
plt.tight_layout()



def read_photos():
    for name in glob.glob('photos/*.jpg'):
        img = PIL.Image.open(name)
        exif = {
            PIL.ExifTags.TAGS[k]: v
            for k, v in img._getexif().items()
            if k in PIL.ExifTags.TAGS
        }
        yield name, datetime.strptime(exif['DateTime'], '%Y:%m:%d %H:%M:%S')


raw_photos = pd.DataFrame(list(read_photos()), columns=['name', 'created_at'])

photos_cities = raw_photos.assign(key=0).merge(cities.assign(key=0), how='outer')
photos = photos_cities[
    (photos_cities.created_at >= photos_cities.arrived)
    & (photos_cities.created_at <= photos_cities.left)
]

photos_by_city = photos \
    .groupby(by='city') \
    .agg({'name': 'count'}) \
    .rename(columns={'name': 'photos'}) \
    .reset_index()

photos_by_city.plot(x='city', y='photos', kind="bar",
                    title='Photos by city', legend=False)
plt.tight_layout()


classify_image.init()
tags = photos.name\
    .apply(lambda name: classify_image.run_inference_on_image(name, 1)[0]) \
    .apply(pd.Series)
tags.columns = ['tag', 'score']

tagged_photos = photos.copy()
tagged_photos[['tag', 'score']] = tags.apply(pd.Series)
tagged_photos['tag'] = tagged_photos.tag.apply(lambda tag: tag.split(', ')[0])

photos_by_tag = tagged_photos[['tag', 'name']] \
    .groupby(by='tag') \
    .agg({'name': 'count'}) \
    .rename(columns={'name': 'photos'}) \
    .reset_index() \
    .sort_values('photos', ascending=False) \
    .head(10)

photos_by_tag.plot(x='tag', y='photos', kind='bar',
                   legend=False, title='Popular tags'); plt.tight_layout()


popular_tags = photos_by_tag.head(5).tag
popular_tagged = tagged_photos[tagged_photos.tag.isin(popular_tags)]
not_popular_tagged = tagged_photos[~tagged_photos.tag.isin(popular_tags)].assign(
    tag='other')
by_tag_city = popular_tagged \
    .append(not_popular_tagged) \
    .groupby(by=['city', 'tag']) \
    .count()['name'] \
    .unstack(fill_value=0)


by_tag_city.plot(kind='bar', stacked=True)
plt.tight_layout()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
