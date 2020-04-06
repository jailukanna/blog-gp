---
title: mysql example Analysis (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Analysis'

Functions in program: 
* `def load(varName):`
* `def save(var, varName):`
* `def toc():`
* `def tic():`

Modules used in program: 
* `import time`
* `import json`

## python Analysis

Python mysql example: Analysis

```python
#!/usr/bin/python3

import json
import time

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

print('Loading Distance Matrix')
compsList = load('compsList')
degrees = load('degrees')
toc()

for x in range(6):
    compCount = 0
    for i in range(len(degrees)):
        for j in range(len(degrees[i])):
            if degrees[i][j] == x:
                compCount += 1
    print('Distance: {:.0f}    Number of competition combinations: {:8.0f}, ({:6.2f} %)'.format(x, compCount, compCount/(len(compsList)*(len(compsList)-1))*100))
toc()


maxDegree = max([max(d) for d in degrees])
print('Maximum degree of separation: {}'.format(maxDegree))


print('Searching for longest chains')
longest = 5
chainIndexes = []
for i in range(len(degrees)-1):
    for j in range(i+1, len(degrees[i])):
# for i in range(len(degrees)):
#     for j in range(len(degrees[i])):
        if degrees[i][j] >= longest:
            chainIndexes.append((i,j))
            print('Distance:', degrees[i][j], '\t', i, compsList[i], '-->', compsList[j], j)
        if degrees[i][j] == 0 and not i==j:
            print(degrees[i][j], i, compsList[i], '-->', compsList[j], j, sep='\t')
save(chainIndexes, 'chainIndexes')           
toc()


print('Longest Chains')
chains = []
for i,j in chainIndexes:
#for i,j in [(2936,2937)]:
    
    thisChain = [] #[[] for _ in range(maxDegree+1)]
    for dist in range(maxDegree+1):
        for link in range(len(degrees)):
            if degrees[i][link] == dist and degrees[j][link] == maxDegree-dist:
                thisChain.append(link)
                break
    
    chains.append(thisChain)
    print(compsList[i], '-->', compsList[j])
    print('    ('+' -- '.join([compsList[link] for link in thisChain])+')')
    print()
print()
save(chains,'chains')
toc()




print('Maximum distance from other competitions')
maxdistances = [max(d) for d in degrees]
for i in range(1,6):
    count = sum([1 for d in maxdistances if d == i])
    print(('Distance: {:.0f}\t Number of competitions: {:.0f}'.format(i, count)))
toc()

print('Average distance from other competitions')
averagedistances = [sum(d)/len(d) for d in degrees]
averagedistances = [(i, averagedistances[i]) for i in range(len(averagedistances))]
averagedistances.sort(key=lambda x: x[1])
for i in range(20):
    x, dist = averagedistances[i]
    print(i+1,'.\t', compsList[x],' '*(30-len(compsList[x])), 'Average:', round(dist,3) ,' \tMax:',maxdistances[x])
print('...')
for i in range(len(averagedistances)-20, len(averagedistances)):
    x, dist = averagedistances[i]
    print(i+1,'.\t', compsList[x],' '*(30-len(compsList[x])), 'Average:', round(dist,3) ,' \tMax:',maxdistances[x])

toc()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
