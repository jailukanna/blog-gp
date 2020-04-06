---
title: mysql example Orgstats-program (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Orgstats-program'


Modules used in program: 
* `import plotly.graph_objs as go`
* `import plotly`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import sqlalchemy as db`
* `import datetime`
* `import MySQLdb `
* `import mysql.connector`
* `import time`
* `import os`
* `import pandas as pd `
* `import re`

## python Orgstats-program

Python mysql example: Orgstats-program

```python
 
# # OrgStats Program 

#  ###  The OrgStats program was designed to work with the CSU Involve website. It is designed to pull organization names and membership enrollment and generate reports on student engagment in campus organizations 

# The program uses both Selenium and BeautifulSoup to navigate and parse the pages of the CSUInvolve website. Selenium is necessary since the website is almost completely comprised of JavaScript enabled content, which BeautifulSoup cannot obtain data from by itself. The program also uses Plotly and Matplotlib to generate beautiful, interactive tables and charts to visually display the data.

#  First the packages needed are installed


from selenium import webdriver
from selenium.webdriver.common.keys import Keys 
from bs4 import BeautifulSoup
import re
import pandas as pd 
import os
import time
import mysql.connector
from pandas.io import sql
import MySQLdb 
import datetime
import sqlalchemy as db
#get_ipython().run_line_magic('matplotlib', 'widget')
import matplotlib.pyplot as plt
import numpy as np
import plotly
from plotly.offline import iplot, plot, init_notebook_mode
import plotly.graph_objs as go


# Selenium is used to launch the CSUInvolve organization page through the Firefox browser

url = "https://columbusstate.campuslabs.com/engage/organizations"
driver = webdriver.Firefox()
driver.implicitly_wait(30)
driver.get(url)
# After the webpage opens, Selenium navigates the user to the login page.  This step is necessary in order to see the number of members in each organization

python_button = driver.find_element_by_css_selector('#react-app div:nth-child(3) > div > a > div > span')
python_button.click()


# The user will have 30 seconds to enter their credentials into the login fields. After 30 seconds, Selenium automatically clicks the "Sign In" button to log the user in.

time.sleep(30)
python_button = driver.find_element_by_css_selector('.wr-btn')
python_button.click()


# After the login button is clicked, Selenium will click the "Load More" button until all organizations are loaded
 
time.sleep(2)

python_button = driver.find_element_by_xpath('//*[@id="react-app"]/div/div[2]/div/div/div[2]/div/div[2]/div[2]/button')

count = 1

while count < 15 :
	python_button.click()
	count += 1
	time.sleep(2)

# Now BeautifulSoup will grab all the links to each organization's page

soup_level1 = BeautifulSoup(driver.page_source, 'html')
links = []
x = 1 #counter


# For now, the names of the organizations and the number of the members are stored into lists. 

org_name = []

org_membs = []


# The code below has several steps:
# 1. Selenium clicks to each organization's page one by one  
# 2. BeautifulSoup then takes the organization name from the page and adds it to the org_name list.
# 3. Selenium clicks the "View Full Roster" button  
# 4. BeautifulSoup pulls the number of members from the roster and adds them to the org_membs list  
# 5. Selenium navigates the browser back twice to get to the organizations page and the process is looped until all individual organization pages are visited and scraped of the requested data

for link in soup_level1.find_all("div" , id ="org-search-results") :
    for b in link.findAll('a'):
        python_button = driver.find_element_by_xpath('//*[@id="org-search-results"]/div/div/div[' + str(x) +']/a')
        #selenium will click through each organization's page
        python_button.click()
        time.sleep(2)
        soup_level2 = BeautifulSoup(driver.page_source, 'html')
        #add organization name to org_name list
        org_name.append(soup_level2.find('h1').text.strip())
        # Selenium clicks on the "View Full Roster" button
        python_button = driver.find_element_by_css_selector('#react-app > div:nth-child(1) > div:nth-child(2) > div:nth-child(1) > div:nth-child(1) > div:nth-child(5) > div:nth-child(1) > div:nth-child(1) > div:nth-child(1) > div:nth-child(1) > div:nth-child(3) > a:nth-child(1)')
        python_button.click()
        time.sleep(1)
        # pull number of members, add to org_membs list
        soup_level3 = BeautifulSoup(driver.page_source, 'html')
        org_membs.append(soup_level3.select_one('html body div#react-app div div div div div div span span').text[0:3].strip(' T'))
        driver.back()
        driver.back()
        x += 1
        time.sleep(1)
#Close the browser now that we have the information we need
driver.quit()


# Now the list of numbers for the members enrolled in each organization is casted to type int for calculation purposes (will be used in the report generation )

for i in range(len(org_membs)):
	org_membs[i] = int(org_membs[i])


# Both lists will now be converted into a Python dictionary which will then be converted into a Pandas dataframe

orgData = {'Name': org_name, 'Membership': org_membs}

orgData_df = pd.DataFrame.from_dict(orgData)


# A database has already been created for storing runs of this program. This is so historical data can be compared to new runs to calculate changes in membership. The following code is to connect to the database

engine = db.create_engine('mysql://csuinvolve:csuinvolve@localhost/orgdatabase')

connection = engine.connect()


# The name of each table in the database will be the date the program is run

now = time.strftime('%Y-%m-%d')


# The following code saves the dataframe to the MySQL database. If the program is run more than once per day the previous results will be overwritten.

orgData_df.to_sql(now, con = engine, if_exists ='replace')


# The following code will pull entries from the database and add them to a dataframe in order to display results and plot a chart. For now, the dates have been manually entered. Future updates for the program will involve this process being done automatically.

df = pd.read_sql_query(sql= "SELECT Name, Membership AS 'Mar_10_19' FROM `2019-04-10` ", con= connection)
df ['Mar_13_19'] = pd.read_sql_query(sql="SELECT Membership FROM `2019-04-13`", con= connection)
df ['Mar_21_19'] = pd.read_sql_query(sql="SELECT Membership FROM `2019-04-21`", con= connection)


# A second dataframe is created by making a copy of the first dataframe, adding a column the the calculation of the standard deviation of each row. Then a third dataframe is created only keeping the organizations that had a standard deviation greater than 0 as this indicates there was a change in the number of members between the current date and the previous dates the program was run

df2 = df
df2['std_dev'] = df2.std(axis= 1)
memb_chg_df = df2[df2.std_dev > 0]


# Now both dataframes can be displayed neatly as tables using Plotly 

#init_notebook_mode(connected=True)

trace1 = go.Table(
    header=dict(values=list(df.columns[0:4]),
                fill = dict(color='#C2D4FF'),
                align = ['left'] * 5),
    cells=dict(values=[df.Name, df.Mar_10_19, df.Mar_13_19, df.Mar_21_19],
               fill = dict(color='#F5F8FF'),
               align = ['left'] * 5))

data1= [trace1]

plot(data1)

trace2 = go.Table(
    header=dict(values=list(memb_chg_df.columns[0:4]),
                fill = dict(color='#C2D4FF'),
                align = ['left'] * 5),
    cells=dict(values=[memb_chg_df.Name, memb_chg_df.Mar_10_19, memb_chg_df.Mar_13_19, memb_chg_df.Mar_21_19],
               fill = dict(color='#F5F8FF'),
               align = ['left'] * 5))

data2 = [trace2]
plot(data2)


# Now a graph is created to visually display the changes in membership 

ax = plt.gca()
memb_chg_df.plot(kind='line', x='Name', y='Mar_10_19',ax=ax)
memb_chg_df.plot(kind='line', x='Name', y='Mar_13_19', color='green', ax=ax)
memb_chg_df.plot(kind='line', x='Name', y='Mar_21_19', color='red', ax=ax)
plt.xticks( np.arange(len(memb_chg_df)), memb_chg_df['Name'], rotation='vertical')
ax.xaxis.set_major_locator(plt.MaxNLocator(21))
plt.margins(0.5)
plt.subplots_adjust(bottom=0.5)
plt.ylim(0,560)
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
