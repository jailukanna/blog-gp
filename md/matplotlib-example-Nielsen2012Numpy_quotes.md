---
title: matplotlib example Nielsen2012Numpy quotes (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Nielsen2012Numpy quotes'

Functions in program: 
* `def sentiment(text, norm='sqrt'):`

Modules used in program: 
* `import copy`
* `import re`
* `import numpy as np`
* `import nltk.corpus `
* `import matplotlib.finance`
* `import matplotlib.dates`
* `import datetime`
* `import dateutil.parser`
* `import simplejson as json`
* `import urllib, urllib2`

## python Nielsen2012Numpy quotes

Python matplotlib example: Nielsen2012Numpy quotes

```python
import urllib, urllib2
import simplejson as json
import dateutil.parser
import datetime
import matplotlib.dates
import matplotlib.finance
from matplotlib import pyplot as plt
import nltk.corpus 
import numpy as np
import re
import copy


companies = {
    'Novo Nordisk': {'stock': 'NVO', 'wikipedia': 'Novo_Nordisk'},
    'Pfizer': {'stock': 'PFE', 'wikipedia': 'Pfizer'} 
    }


filebase = '/home/fn/'

# Sentiment word list
# AFINN-111 is as of June 2011 the most recent version of AFINN
filename_afinn = filebase + '/data/AFINN/AFINN-111.txt'
afinn = dict(map(lambda (w, s): (unicode(w, 'utf-8'), int(s)), [ 
            ws.strip().split('\t') for ws in open(filename_afinn) ]))

stopwords = nltk.corpus.stopwords.words('english')
stopwords = dict(zip(stopwords, stopwords))


# Word splitter pattern
pattern_split = re.compile(r"[^\w-]+", re.UNICODE)

def sentiment(text, norm='sqrt'):
    """
    Sentiment analysis.
    (sentiment, arousal, ambivalence, positive, negative) = sentiment(test)
    """
    words_with_stopwords = pattern_split.split(text.lower())
    # Exclude stopwords:
    words = filter(lambda w: not stopwords.has_key(w), words_with_stopwords)
    sentiments = map(lambda word: afinn.get(word, 0), words)
    keys = ['sentiment', 'arousal', 'ambivalence', 'positive', 'negative']
    if sentiments:
        sentiments = np.asarray(sentiments).astype(float)
        sentiment = np.sum(sentiments)
        arousal = np.sum(np.abs(sentiments))
        ambivalence = arousal - np.abs(sentiment)
        positive = np.sum(np.where(sentiments>0, sentiments, 0))
        negative = - np.sum(np.where(sentiments<0, sentiments, 0))
        result = np.asarray([sentiment, arousal, ambivalence, positive, negative])
        if norm == 'mean':
            result /= len(sentiments)
        elif norm == 'sum':
            pass
        elif norm == 'sqrt':
            result /= np.sqrt(len(sentiments))
        else:
            raise("Wrong ''norm'' argument")
    else:
        result = (0, 0, 0, 0, 0)
    return dict(zip(keys, result))


today = datetime.date.today()

# Matplotlib x-axis date formatting
days_locations = matplotlib.dates.DayLocator()
months_locations = matplotlib.dates.MonthLocator()
months_formatter = matplotlib.dates.DateFormatter("%Y %b")

# Prepare URL and download for Wikipedia
opener = urllib2.build_opener()
opener.addheaders = [('User-agent', 'Finn Aarup Nielsen, +45 45 25 39 21')]
urlbase = "http://en.wikipedia.org/w/api.php?"


for company, fields in companies.items():
    wikipedia_revisions = []
    urlparam = {'action': 'query', 
                'format': 'json', 
                'prop': 'revisions',
                'rvlimit': 50,
                'rvprop': 'ids|timestamp|content', 
                'titles': fields['wikipedia']}
    for i in range(7):
        url = urlbase + urllib.urlencode(urlparam)
        wikipedia_result = json.load(opener.open(url))
        wikipedia_revisions.extend(wikipedia_result['query']['pages'].values()[0]['revisions'])
        print("%s: %d" % (company, len(wikipedia_revisions)))
        if 'query-continue' in wikipedia_result: 
            urlparam.update(wikipedia_result['query-continue']['revisions'])
        else:
            break
    wikipedia_last_timestamp = wikipedia_revisions[-1]['timestamp']
    wikipedia_last_datetime = dateutil.parser.parse(wikipedia_last_timestamp)
    wikipedia_last_date = datetime.datetime.date(wikipedia_last_datetime)
    for n, revision in enumerate(wikipedia_revisions):
        wikipedia_revisions[n].update(sentiment(revision['*']))
    companies[company].update({'wikipedia_revisions': copy.deepcopy(wikipedia_revisions)})
    companies[company].update({'quotes': matplotlib.finance.quotes_historical_yahoo(fields['stock'], wikipedia_last_date, today)})
    xaxis_range = matplotlib.dates.date2num(wikipedia_last_date), matplotlib.dates.date2num(today)
    fig = plt.figure() 
    for i in range(1,3):
        ax = fig.add_subplot(2, 1, i)
        ax.xaxis.set_major_locator(months_locations)
        ax.xaxis.set_minor_locator(days_locations)
        ax.xaxis.set_major_formatter(months_formatter)
        if i == 1:
            quotes = companies[company]['quotes']
            h = matplotlib.finance.candlestick(ax, quotes)
            h = plt.ylabel('Stock prize')
            h = plt.title(company)
        else:
            x = map(lambda fs: matplotlib.dates.date2num(dateutil.parser.parse(fs['timestamp'])), wikipedia_revisions) 
            y = map(lambda fs: fs['sentiment'], wikipedia_revisions)
            h = plt.plot(x, y)
            h = plt.xlabel('Date')
            h = plt.ylabel('Wikipedia sentiment')
        h = ax.set_xlim(xaxis_range)
        fig.autofmt_xdate()
    plt.show()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
