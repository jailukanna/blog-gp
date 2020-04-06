---
title: mysql example test%2520new (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'test%2520new'


Modules used in program: 
* `import csv`
* `import urllib`
* `import datetime`
* `import urllib.request`

## python test%2520new

Python mysql example: test%2520new

```python

#This program downloads the pdf file from the NLDC website automatically by finding the link of the first file. it needs to be run on daily basis and can fetch data for 2017-18
#for next year one can use different year but it will only pick first element of first table in the page
#one needs to have anoconda or install lxml package by default to run this code.

from bs4 import BeautifulSoup as BS
import urllib.request
import datetime
import urllib

#to import it into excel csv using
import csv

#%y gives date in 2 digits and %Y gives data in 4 digits    
Date = datetime.datetime.now().strftime("%d-%m-%y")
Date = str(Date)
print(Date)

#this step give the link of webpage b y which we will import the html text to parse
Download_page='https://posoco.in/reports/daily-reports/daily-reports-2017-18'

#this step is to open the webpage and reading is done and it is saved in the variable named page
page= urllib.request.urlopen(Download_page).read()
#this step will parse the data using BS4 module
prossd_page= BS(page, 'lxml')
#now we will find all a tags inside the parsed data
dwnld_links = prossd_page.findAll("a", href= True)
#this step will give the info within first cell of table
dwnld_link_td = prossd_page.td.a
#this will convert the bs4.Resultelement into str
dwnld_str= str(dwnld_link_td)
#this will split the string further to get groups of strings seperated by white spaces
element= dwnld_str.split()
#this will pick up the first element of the element list generated above
dwn_lnk= str(element[1])
#this will precisely pick the dynamic download link
act_dwn_lnk= dwn_lnk[6:64]
#this function will download the file form the text generated from the scraping 
urllib.request.urlretrieve(act_dwn_lnk, filename= "NLDC.pdf")



#open a csv file with append, so old data will not be erased
#with open(‘index.csv’, ‘a’) as csv_file:
 #writer = csv.writer(csv_file)
# writer.writerow([name, price, datetime.now()]) 


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
