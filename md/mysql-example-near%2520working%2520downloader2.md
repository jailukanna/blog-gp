---
title: mysql example near%2520working%2520downloader2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'near%2520working%2520downloader2'


Modules used in program: 
* `import urllib`
* `import datetime`
* `import urllib.request`
* `import bs4 as BeautifulSoup`

## python near%2520working%2520downloader2

Python mysql example: near%2520working%2520downloader2

```python
import bs4 as BeautifulSoup
#import pandas as pd
#import numpy as np
import urllib.request
import datetime
import urllib

#%y gives date in 2 digits and %Y gives data in 4 digits    
Date = datetime.datetime.now().strftime("%d-%m-%Y")
Date = str(Date)
print(Date)
   
Download_page='https://posoco.in/reports/daily-reports/daily-reports-2017-18'

    
    
page= urllib.request.urlretrieve(Download_page)
prossd_page= BeautifulSoup(page)

for div in prossd_page.findAll('div', attrs={'class':'image'}):
     print(div.find('a')['href'])
     print(div.find('a').contents[0])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
