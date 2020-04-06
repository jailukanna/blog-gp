---
title: mysql example wordcloud (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'wordcloud'

Functions in program: 
* `def main():`
* `def specify_year_state(dddb, resultsDir, stopWords, year, state):`
* `def specify_year(dddb, resultsDir, stopWords, year, states):`
* `def printHelp():`
* `def create_results_dir(dirName, year, states):`
* `def get_words(cursor, stopList, year, state):`
* `def filter_words(inputStr, stopList):`
* `def parse_stopwords(filename):`

Modules used in program: 
* `import os`
* `import time`
* `import string`
* `import operator`
* `import sys`
* `import MySQLdb`

## python wordcloud

Python mysql example: wordcloud

```python
#!/usr/bin/env python
# -*- coding: utf8 -*-

from Database_Connection import mysql_connection
from nltk.tokenize import word_tokenize
from nltk.probability import FreqDist
from nltk.stem.lancaster import LancasterStemmer
from nltk import bigrams
from wordcloud import WordCloud
import MySQLdb
import sys
import operator
import string
import time
import os
from datetime import datetime

S_UTTERANCE = '''SELECT p.first, p.middle, p.last, u.text
                 FROM Legislator l JOIN Person p ON l.pid = p.pid
                                   JOIN Utterance u ON l.pid = u.pid
                                   JOIN Video v ON u.vid = v.vid
                                   JOIN Hearing h ON v.hid = h.hid
                 WHERE h.date BETWEEN %s AND %s
                 AND h.state = %s'''


#parse stop words file
def parse_stopwords(filename):
    result = []
    f = open(filename, 'r')
    for line in f.readlines():
        result.append(line.strip('\n'))
    return result

#break utterances into words and filter out stop words
def filter_words(inputStr, stopList):
    result = []
    if inputStr != None:
        temp = word_tokenize(inputStr)
        for t in temp:
            if (t not in string.punctuation) and ('\'' not in t) and (t.lower() not in stopList):
                newWord = t.lower()
                newWord = newWord.rstrip(string.punctuation)
                result.append(newWord)
            
    return result


'''
grabs all utterances from legislators
returns a dict {Key: word -> value: fequency}
'''
def get_words(cursor, stopList, year, state):
    startDate = year + "-01-01"
    currDate = time.strftime("%Y-%m-%d")
    d = FreqDist()
    ls = LancasterStemmer()
    stemDict = dict()
    cursor.execute(S_UTTERANCE, (startDate, currDate, state))
    if cursor.rowcount > 0:
        for r in cursor.fetchall():
            words = filter_words(r[3], stopList)
            for word in words:
                stemWord = ls.stem(word)
                if stemWord in stemDict:
                    d[stemDict[stemWord]] += 1
                else:
                    stemDict[stemWord] = word
                    d[stemDict[stemWord]] += 1

    return d    

'''
creates directories for results if it doesn't exist
creates a directory like dirName/year/state
'''
def create_results_dir(dirName, year, states):
    if not os.path.exists(dirName):
        os.makedirs(dirName)

    dirNameYear = dirName + "/" + year
    if not os.path.exists(dirNameYear):
        os.makedirs(dirNameYear)

    for state in states:
        dirNameYearState = dirNameYear + "/" + state
        if not os.path.exists(dirNameYearState):
            os.makedirs(dirNameYearState)


def printHelp():
    print("usage: python q4.py")
    print("usage: python q4.py [year]")
    print("usage: python q4.py [year state]")
    print("year must be between 2015-" + str(datetime.now().year))

'''
generate word cloud for a specific year
'''
def specify_year(dddb, resultsDir, stopWords, year, states):
    year = int(year)
    if year % 2 == 0:
            year = year - 1
    year = str(year)
    resultsDir = resultsDir + "/" + year
    for state in states:
        wordDict = get_words(dddb, stopWords, year, state)

        if len(wordDict) > 0:
            wordcloud = WordCloud(width=600, height=600).generate_from_frequencies(wordDict)
            wordcloud.to_file(resultsDir + "/" + state + "/q4.png")

'''
generate wordcloud for specific year and state
'''
def specify_year_state(dddb, resultsDir, stopWords, year, state):
    year = int(year)
    if year % 2 == 0:
            year = year - 1
    year = str(year)
    resultsDir = resultsDir + "/" + year
    wordDict = get_words(dddb, stopWords, year, state)

    if len(wordDict) > 0:
        wordcloud = WordCloud(width=600, height=600).generate_from_frequencies(wordDict)
        wordcloud.to_file(resultsDir + "/" + state + "/q4.png")


def main():
    ddinfo = mysql_connection(sys.argv)
    with MySQLdb.connect(host=ddinfo['host'],
                        user=ddinfo['user'],
                        db=ddinfo['db'],
                        port=ddinfo['port'],
                        passwd=ddinfo['passwd'],
                        charset='utf8') as dddb:

        resultsDir = "../results"
        year = datetime.now().year
        if year % 2 == 0:
            year = year - 1

        year = str(year)
        states = ["CA", "NY"]
        create_results_dir(resultsDir, year, states)

        stopWords = parse_stopwords("stopwords.txt")
        userWords = parse_stopwords("userStopwords.txt")
        if (len(userWords) > 0):
            stopWords.extend(userWords)

        #query 4 all utterances

        # check for commandline args, year has to be between 2015 and current year
        # otherwise run on current session year
        if (len(sys.argv) > 1):
            if (len(sys.argv) == 2):
                yearInput = sys.argv[1]
                if (yearInput.isdigit() and int(yearInput) >= 2015 and int(yearInput) <= datetime.now().year):
                    year = yearInput
                    specify_year(dddb, resultsDir, stopWords, year, states)
                else:
                    printHelp()
            elif (len(sys.argv) == 3):
                yearInput = sys.argv[1]
                stateInput = sys.argv[2]
                if (yearInput.isdigit() and int(yearInput) >= 2015 and int(yearInput) <= datetime.now().year):
                    year = yearInput
                    state = stateInput
                    specify_year_state(dddb, resultsDir, stopWords, year, state)
                else:
                    printHelp()
            else:
                printHelp()
        else:
            specify_year(dddb, resultsDir, stopWords, year, states)
            

if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
