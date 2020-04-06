---
title: mysql example Storage (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Storage'


Modules used in program: 
* `import urllib`
* `import sys`
* `import Analysis`
* `import Scraper`
* `import mysql.connector as mysql`
* `import squelch`

## python Storage

Python mysql example: Storage

```python
#Manipulate Database
import squelch
import mysql.connector as mysql
import Scraper
import Analysis
import sys
import urllib
#If it is the first time the program is started after reset, connect to database

class Storage():

    connected = False
    user = ''
    password = ''
    host = ''
    name = ''
    database = ''
    cursor = ''
    newUpdate = True
    stockList = []
    start = False
    
    def __init__(self):
        pass
        
    #If it is the first time the program is started after reset, connect to database
    
    def connect(self):
        
        if self.connected == False:

            self.database = mysql.connect(user = squelch.squelch.db_user,
                          password = squelch.squelch.db_pass,
                          host = squelch.squelch.db_host,
                          database = squelch.squelch.db_name)

            self.connected = True

        else:
            pass
        
        self.cursor = self.database.cursor(buffered=True)
        
    def checkStart(self):

        #Redundancy for hardcoding SPY
        if self.start == False:
            self.cursor.execute("SELECT Ticker FROM twitter "
                                "WHERE Ticker='SPY'")
            check = self.cursor.fetchone()
                
            if check == None:
                    
                self.cursor.execute("INSERT INTO twitter(Ticker, Views, Searched, mapass, pegpass, Frozen) "
                               "VALUES(%s, %s, %s, %s, %s, %s)", ("SPY", 0, 0, 0, 0, 0))
                self.database.commit()

                self.start = True
            
            else:
                
                pass
        
        
    def updateSearched(self):
        #Update stock being searched to true for searched
        
        self.cursor.execute("UPDATE twitter "
                           "SET Searched=%s "
                           "WHERE Ticker=%s",  (1, Scraper.scraper.stock))
        
        self.database.commit()
        
    def addNewStocks(self, newStocks):
        
        #Test to see if any parsed stocks are not in the database already
        for i in range(len(newStocks)):
            
            self.cursor.execute("INSERT INTO twitter"
                            "(Ticker, Views, Searched, mapass, pegpass, Frozen) "
                            "VALUES(%s, %s, %s, %s, %s, %s) "
                            "ON DUPLICATE KEY UPDATE Ticker=Ticker", (str(newStocks[i]), 0, 0, 0, 0, 0))
            
            self.database.commit()

            
    def populateViews(self):
        
        #Update total number of views              
        for i in range(len(Scraper.scraper.countTickerSymbols)):
            
            count = Scraper.scraper.ticker_symbols_found.count(Scraper.scraper.countTickerSymbols[i])
            self.cursor.execute("SELECT Views FROM twitter "
                           "WHERE Ticker=%s", (str(Scraper.scraper.countTickerSymbols[i]),))
            
            update = self.cursor.fetchone()[0]
            count += update
            
            #Update number of views, and if the other data points are true or false
            self.cursor.execute("UPDATE twitter "
                           "SET Views=%s "
                           "WHERE Ticker=%s", (count, str(Scraper.scraper.countTickerSymbols[i])))
            
        self.database.commit()


    #Find a new stock to search
        
    def updateStockToSearch(self):
            
        self.cursor.execute("SELECT Ticker FROM twitter "
                           "WHERE Searched=0")
        
        allStocks = self.cursor.fetchone()
        
        if allStocks == None:
            return "DONE"
        
        else:
            Scraper.scraper.stock = allStocks[0]

        self.database.commit()

    #After all stocks are parsed, run an analysis on the top 50 stocks
    def populateAnalysis(self):
        
        self.cursor.execute("SELECT Ticker FROM twitter "
                            "WHERE Frozen=0 "
                            "ORDER BY Views DESC limit 25")
        
        topStocks = self.cursor.fetchall()
        
        for i in range(len(topStocks)):
            
            stock = str(topStocks[i][0])
            
            try:
                Analysis.analysis.createDates(35)
                Analysis.analysis.movingAverage(stock, Analysis.analysis.dates[-1], Analysis.analysis.dates[0], 35)
                Analysis.analysis.createDates(350)
                Analysis.analysis.movingAverage(stock, Analysis.analysis.dates[-1], Analysis.analysis.dates[0], 350)
                movingAverage = Analysis.analysis.testMovingAverage(stock)
                PEG = Analysis.analysis.priceEarningsGrowth(stock)
                
            except (ValueError, urllib.error.HTTPError):
                print('ERROR IN GETTING MA AND PEG')
                continue
            
            print(stock + ': Price= ' + str(priceBought) + ' MA=' + str(movingAverage) +' PEG=' + str(PEG))
            
            #Update Moving Average and PEG in the database for the searched stock
            
            self.cursor.execute("UPDATE twitter "
                               "SET mapass=%s, pegpass=%s, "
                               "WHERE Ticker=%s", (movingAverage, PEG))
            
        self.newUpdate = True
        self.database.commit()
        
            
    def exportAndRestart(self):
        
        run = True
        
        while run == True:
            
            try:
                
                self.cursor.execute("INSERT INTO nunc(Ticker)"
                                     "SELECT Ticker FROM twitter "
                                     "WHERE mapass=1 and pegpass=1 "
                                     "ON DUPLICATE KEY UPDATE stock=twitter.Ticker")

                run = False
                
            except mysql.Error as e:
                print(e)
                

        self.cursor.execute("DELETE FROM twitter")
        Scraper.scraper.stock = "SPY"

        self.database.commit()
        
storage = Storage()
        
        

    


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
