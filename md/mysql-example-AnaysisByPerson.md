---
title: mysql example AnaysisByPerson (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'AnaysisByPerson'

Functions in program: 
* `def timef(s):`
* `def load(varName):`
* `def save(var, varName):`
* `def tocx():`
* `def toc():`
* `def tic():`

Modules used in program: 
* `import math`
* `import time`
* `import json`

## python AnaysisByPerson

Python mysql example: AnaysisByPerson

```python
#!/usr/bin/python3

import json
import time
import math


def tic():
    tic.start = time.time()
def toc():
    print('Time elapsed:', round(time.time() - tic.start, 3),'s\n')
    return (time.time() - tic.start)
def tocx():
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

compsIndex = {compsList[i]: i for i in range(len(compsList))}
nCompConnections = [sum([1 for d in dists if d == 1]) for dists in degrees]

toc()


print('Loading competition attendance data')
compsAttended = load('compsAttended')
compReg = load('compReg')
toc()


print('Assigning competitions to competitors')
compsPerPerson = []
people = []
for p in sorted(compsAttended.keys()):
    people.append(p)
    compsPerPerson.append([])
    for comp in compsAttended[p]:
        compsPerPerson[-1].append(compsIndex[comp])
print('Number of people:', len(people))
toc()


print('Assigning competitors to competitions')
peoplePerComp = []
peopleIndex = {people[i]: i for i in range(len(people))}
for c in compsList:
    peoplePerComp.append([])
    for p in compReg[c]:
        peoplePerComp[-1].append(peopleIndex[p])
toc()



print('Grouping people by identical comp attendance')
i = 0
peopleGroupedDict = {}
compsPerPeopleGroup = {}
for p in range(len(people)):
    compsid = str(sorted(compsPerPerson[p]))
    if not compsid in peopleGroupedDict.keys():
        peopleGroupedDict[compsid] = []
        compsPerPeopleGroup[compsid] = sorted(compsPerPerson[p], key = lambda c: -nCompConnections[c])
    peopleGroupedDict[compsid].append(people[p])

compCombos = sorted(peopleGroupedDict.keys())
nCombos = len(compCombos)
print(nCombos, 'unique comp combinations')

save(compCombos, 'compCombos')
save(peopleGroupedDict, 'peopleGroupedDict')
save(compsPerPeopleGroup, 'compsPerPeopleGroup')

peopleGroups = []
compsLookup = []
for pg in compCombos:
    peopleGroups.append(peopleGroupedDict[pg])
    compsLookup.append(compsPerPeopleGroup[pg])

toc()

    


print('Average distance per competitor')
avgDistByPeople = []

def timef(s):
    s = round(s)
    return '{:.0f}:{:02.0f}:{:02.0f}'.format(math.floor(s/3600), math.floor(s/60)%60, s%60)

counter = 0
total = len(peopleGroups)*len(peopleGroups)
ttotal = 6000
peopleDistanceCounts = []

try:
    peopleDistanceCounts = load('peopleDistanceCounts')
    avgDistByPeople = load('avgDistByPeople')
except FileNotFoundError:
    for pgrp1 in range(len(peopleGroups)):
        theseDists = [0 for _ in range(6)]
        for pgrp2 in range(len(peopleGroups)):
            if pgrp1 == pgrp2:
                theseDists[0] += len(peopleGroups[pgrp2])
                continue
            key = (min(pgrp1, pgrp2), max(pgrp1, pgrp2))
            
            dist = 5
            for c1 in compsLookup[pgrp1]:
                if c1 in compsLookup[pgrp2]:
                    dist = 0
                    break
            else:
                for c1 in compsLookup[pgrp1]:
                    for c2 in compsLookup[pgrp2]:
                        if degrees[c1][c2] < dist:
                            dist = degrees[c1][c2]
                            if dist == 1:
                                break
                    if dist == 1:
                        break
               
            theseDists[dist] += len(peopleGroups[pgrp2])
            counter += 1
            if not counter%1000000:
                ttotal = tpassed/fractionComplete
            if not counter%200000:
                fractionComplete = counter/total
                tpassed = tocx()
                tremain = ttotal - tpassed
                print(round(fractionComplete*100,3), '%\t', 
                      timef(tpassed), 'passed', timef(ttotal), 'total', timef(tremain), 'remaining.   ', 
                      'Rng', (round(min(avgDistByPeople),3), round(max(avgDistByPeople),3)))
        
        peopleDistanceCounts.append(theseDists)
        avgDistByPeople.append(sum([i*theseDists[i] for i in range(6)])/sum(theseDists))


save(peopleDistanceCounts, 'peopleDistanceCounts')
save(avgDistByPeople, 'avgDistByPeople')

print('Range:', (round(min(avgDistByPeople),3), round(max(avgDistByPeople),3)))

toc()


print('Expanding counts to individual competitors')
distByPeople = {}
for pg in range(len(peopleGroups)):
    for p in peopleGroups[pg]:
        distByPeople[p] = peopleDistanceCounts[pg]
toc()




print('Competitors sorted by maximum distance')
maxDists = [sum([1 for d in distByPeople[p] if d > 0])-1 for p in distByPeople.keys()]
for i in range(1,6):
    count = sum([1 for d in maxDists if d == i])
    print(('Distance: {:.0f}\t Number of people: {:.0f}'.format(i, count)))
toc()


print('Competitors sorted by average distance')
sortedPeople = sorted(distByPeople.keys(), key = lambda p: sum([i*distByPeople[p][i] for i in range(6)])/sum(distByPeople[p]))
for p in range(50):
    avgDist = sum([i*distByPeople[sortedPeople[p]][i] for i in range(6)])/sum(distByPeople[sortedPeople[p]])
    print(p+1, '.\t', sortedPeople[p], '  Average: ', round(avgDist, 3), '\tDistribution: ', distByPeople[sortedPeople[p]], sep='')
print('...')
for p in range(len(sortedPeople)-20, len(sortedPeople)):
    avgDist = sum([i*distByPeople[sortedPeople[p]][i] for i in range(6)])/sum(distByPeople[sortedPeople[p]])
    print(p+1, '.\t', sortedPeople[p], '  Average: ', round(avgDist, 3), '\tDistribution: ', distByPeople[sortedPeople[p]], sep='')
toc()

print("Competitors sorted by number of people they've been to competitions with")
sortedPeople = sorted(distByPeople.keys(), key = lambda p: -distByPeople[p][0])
for p in range(50):
    avgDist = sum([i*distByPeople[sortedPeople[p]][i] for i in range(6)])/sum(distByPeople[sortedPeople[p]])
    print(p+1, '.\t', sortedPeople[p], '  Average: ', round(avgDist, 3), '\tDistribution: ', distByPeople[sortedPeople[p]], sep='')
print('...')
for p in range(len(sortedPeople)-20, len(sortedPeople)):
    avgDist = sum([i*distByPeople[sortedPeople[p]][i] for i in range(6)])/sum(distByPeople[sortedPeople[p]])
    print(p+1, '.\t', sortedPeople[p], '  Average: ', round(avgDist, 3), '\tDistribution: ', distByPeople[sortedPeople[p]], sep='')
toc()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
