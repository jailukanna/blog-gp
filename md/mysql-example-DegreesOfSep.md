---
title: mysql example DegreesOfSep (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'DegreesOfSep'

Functions in program: 
* `def getBackwardBuffer():`
* `def getForwardBuffer(level):`
* `def isFirstConnection(n1, n2):`
* `def isFirstConnectionByPerson(comp1, comp2):`
* `def load(varName):`
* `def save(var, varName):`
* `def toc():`
* `def tic():`

Modules used in program: 
* `import mysql.connector`
* `import time`
* `import json`

## python DegreesOfSep

Python mysql example: DegreesOfSep

```python
#!/usr/bin/python3
import json
import time

import mysql.connector

def tic():
    tic.start = time.time()
def toc():
    print('Time elapsed:', round(time.time() - tic.start, 3),'s\n')
    return (time.time() - tic.start)
    
def save(var, varName):
    with open(varName+'.json','w') as saveFile:
        json.dump(var, saveFile)
        
def load(varName):
    with open(varName+'.json') as loadFile:
        result = json.load(loadFile)
    return result
    
tic()

conn = mysql.connector.connect(host = "localhost",user="root",passwd="root", db="mysql")
cursor = conn.cursor()

# print('Creating Competitor Table...')
# cursor.execute('DROP TABLE Competitors;')
# cursor.execute('CREATE TABLE Competitors AS SELECT DISTINCT PersonId, CompetitionId FROM Results;')
# toc()

print('Finding competitions...')
cursor.execute('SELECT DISTINCT CompetitionId FROM Competitors;')
compsList = [c[0] for c in cursor.fetchall()]
save(compsList, 'compsList')
toc()

print('Fetching Competitor Attendance...')
counter = 0

try:
    compRegCount = load('compRegCount')
    compReg = load('compReg')
except FileNotFoundError:
    compRegCount = {}
    compReg = {}

for comp in compsList:
    if comp not in compReg.keys():
        cursor.execute('SELECT PersonId FROM Competitors WHERE CompetitionId=%s;', (comp,))
        compReg[comp] = [p[0] for p in cursor.fetchall()]
        compRegCount[comp] = len(compReg[comp])
    counter += 1
    if not counter%1000:
        print('{:.0f}/{:.0f} ({:.2f} %)'.format(counter, len(compsList), counter/len(compsList)*100))
toc()

print('Saving...')
save(compReg, 'compReg')
save(compRegCount, 'compRegCount')
toc()


compsAttended = {}
counter = 0
print('Fetching Competitions per Person...')
for comp in compsList:
    for person in compReg[comp]:
        if not person in compsAttended.keys():
            compsAttended[person] = []
        compsAttended[person].append(comp)
    
    counter += 1
    if not counter%1000:
        print('{:.0f}/{:.0f} ({:.2f} %)'.format(counter, len(compsList), counter/len(compsList)*100))
save(compsAttended, 'compsAttended')
toc()


def isFirstConnectionByPerson(comp1, comp2):
    for person in compReg[comp1]:
        if person in compReg[comp2]:
            return True
    return False



def isFirstConnection(n1, n2):
    return n2 in connections[n1]


print('Finding Connections...')
try:
    connections = load('connections')
except FileNotFoundError:
    connections = [[] for _ in compsList]
    counter = 1
    for i in range(len(compsList)-1):
        for j in range(i+1, len(compsList)):
            if isFirstConnection(compsList[i], compsList[j]):
                connections[i].append(j)
                connections[j].append(i)
            counter += 1
            if not counter%50000:
                print('{:.0f}/{:.0f} ({:.2f} %)'.format(counter, len(compsList)*len(compsList)/2, counter/(len(compsList)*len(compsList)/2)*100))
save(connections, 'connections')
nConnections = [len(c) for c in connections]
toc()


# print('Checking first degree connectedness...')
# try:
#     firstdegrees = load('firstdegrees')
# except FileNotFoundError:
#     connections = load('connections')
#     counter = 0
#     firstdegrees = [[0 for _ in range(len(compsList))] for _ in range(len(compsList))]
#     for i in range(len(compsList)-1):
#         for j in range(i+1, len(compsList)):
#             if isFirstConnection(i, j):
#                 firstdegrees[i][j] = 1
#                 firstdegrees[j][i] = 1
#             counter += 1
#             if not counter%200000:
#                 print('{:.0f}/{:.0f} ({:.2f} %)'.format(counter, len(compsList)*(len(compsList)-1)/2, counter/(len(compsList)*(len(compsList)-1)/2)*100))
# save(firstdegrees, 'firstdegrees')
# toc()
# 
#         
# print('{:.2f} % first degree connections.'.format(sum([sum(d) for d in firstdegrees])/(len(compsList)*(len(compsList)-1))*100))
# 
# 
# print('Checking full connectedness...')
# counter = 0
# degrees = [[1 if firstdegrees[i][j] else 0 for i in range(len(compsList))] for j in range(len(compsList))]
# 
# 
# def connectionDegree(n1, n2):
#     degree = 0
#     buffer = [n1]
#     connectionFound = False
#     while not connectionFound:
#         nextBuffer = []
#         for b in buffer:
#             if degrees[b][n2] > 0:
#                 return degree + degrees[b][n2]
#             else:
#                 nextBuffer += connections[b]
#         degree += 1
#         buffer = list(set(nextBuffer))
#         if degree > 4:
#             raise Exception('Degree greater than max allowed: ({}, {})'.format(n1,n2))
# 
# 
# try:
#     degrees = load('degrees')
# except FileNotFoundError:
#     for i in range(len(compsList)-1):
#         for j in range(i+1, len(compsList)):
#             thisDegree = connectionDegree(i,j)
#             degrees[i][j] = thisDegree
#             degrees[j][i] = thisDegree        
#             counter += 1
#             if not counter%50000:
#                 print('{:.0f}/{:.0f} ({:.2f} %)'.format(counter, len(compsList)*(len(compsList)-1)/2, counter/(len(compsList)*(len(compsList)-1)/2)*100))
# toc()
# save(degrees, 'degrees')


print('Filling distance table...')
distances = [[0 if i==j else -1 for i in range(len(compsList))] for j in range(len(compsList))]

def getForwardBuffer(level):
    return [(i,j) for i in range(len(compsList)) for j in range(i,len(compsList)) if distances[i][j] == level]

def getBackwardBuffer():
    return [(i,j) for i in range(len(compsList)) for j in range(i,len(compsList)) if distances[i][j] == -1]


filled = 0
level = 0
total = len(compsList)*len(compsList)
while level<10:
    if filled < total*0.33:
        buffer = getForwardBuffer(level)
        print('Level: {:.0f}, buffer length: {:.0f}. Forward search'.format(level, len(buffer)))
        counter = 0
        toc()
        for i,j in buffer:
            for k in connections[i]:
                if distances[j][k] == -1:
                    distances[j][k] = level+1
                    distances[k][j] = level+1
                    filled += 1
            for k in connections[j]:
                if distances[i][k] == -1:
                    distances[i][k] = level+1
                    distances[k][i] = level+1
                    filled += 1
            counter += 1
            if not counter%20000:
                print('filled: {:.0f}/{:.0f}\tlevel: {:.0f}\t({:.2f} % total, {:.2f} % of level)'.format(filled, total, level, filled/total*100, counter/len(buffer)*100))
    else:
        buffer = getBackwardBuffer()
        print('Level: {:.0f}, buffer length: {:.0f}. Backward search'.format(level, len(buffer)))
        counter = 0
        for i,j in buffer:
            for k in connections[i]:
                if distances[j][k] == level and distances[i][j] == -1:
                    distances[i][j] = level+1
                    distances[j][i] = level+1
                    filled += 1
            for k in connections[j]:
                if distances[i][k] == level and distances[i][j] == -1:
                    distances[i][j] = level+1
                    distances[j][i] = level+1
                    filled += 1
            counter += 1
            if not counter%20000:
                print('filled: {:.0f}/{:.0f}\tlevel: {:.0f}\t({:.2f} % total, {:.2f} % of level)'.format(filled, total, level, filled/total*100, counter/len(buffer)*100))
    level += 1
toc()  

degrees = distances
save(degrees, 'degrees')

for x in range(15):
    compCount = 0
    for i in range(len(degrees)):
        for j in range(len(degrees[i])):
            if distances[i][j] == x:
                compCount += 1
    print('By competition: Distance: {:3.0f}, Count: {:8.0f}, ({:6.2f} %)'.format(x, compCount, compCount/(len(compsList)*(len(compsList)-1))*100))


print('Maximum degree of separation: {}'.format(max([max(d) for d in degrees])))

conn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
