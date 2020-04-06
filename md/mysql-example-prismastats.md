---
title: mysql example prismastats (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'prismastats'

Functions in program: 
* `def extractandplot(select, eventId, maxTime, minTime, secondSpacingMajor, secondSpacingMinor, datelimits, monthSpacingMajor, monthSpacingMinor):`

Modules used in program: 
* `import pylab`
* `import mysql.connector`
* `import subprocess`
* `import datetime`
* `import csv`

## python prismastats

Python mysql example: prismastats

```python
#!/usr/bin/python3

#=======================================================================================================================
# Constants
#=======================================================================================================================

prismaCsvFiles = ['CSVDumps/Prisma_Titan.csv', 'CSVDumps/Prisma_Iapetus.csv', 'CSVDumps/Prisma_Encaladus.csv']
androidCSVFiles = ['CSVDumps/3 x 3 x 3 Cube - S 11, 2014 - 09.22.25 PM.csv', 'CSVDumps/2 x 2 x 2 Cube - A 01, 2014 - 11.53.51 AM.csv']
androidMonthstart = [9,8]
androidcategories = ["Rubik's cube", "2x2x2 cube"]

select = "5x5x5 cube"
eventId = '555'

personId = '2014GRAY03'

maxTime = "3:00.00"
minTime = "1:00.00"
secondSpacingMajor = 20
secondSpacingMinor = 2

#=======================================================================================================================
# Imports
#=======================================================================================================================
 
import csv
import datetime

import subprocess
import mysql.connector

import pylab

#=======================================================================================================================
# Loading data from files
#=======================================================================================================================

print('Reading')

allSolutions = []
categories = []

for csvFile in prismaCsvFiles:
    print('Reading',csvFile)

    with open(csvFile) as thisFile:
        thisCSVFile = csv.reader(thisFile)
        
        for solution in thisCSVFile:
                        
            if solution[0] == 'SOLUTION_ID':
                continue # Skip header line
                        
            start = datetime.datetime.strptime(solution[4], '%Y-%m-%d %H:%M:%S.%f')
            end = datetime.datetime.strptime(solution[5], '%Y-%m-%d %H:%M:%S.%f')
            try:
                penalty = datetime.timedelta(seconds=int(solution[6]))
            except:
                penalty = datetime.timedelta(seconds=0)
               
            time = (end - start) + penalty
            category = solution[10]
            categories.append(category)
            
            allSolutions.append([start, time, category, penalty])


for i in range(len(androidCSVFiles)):
    csvFile = androidCSVFiles[i]
    print('Reading',csvFile)
    
    with open(csvFile) as thisFile:
        thisCSVFile = csv.reader(thisFile)

        # Dates from SpeedCube Timer on Android are ambiguous
        # This assumes dates are in reverse chronological order starting from September, 
        # all in the same year, and with no months missing, and with the first day of
        # each month less than the last day of the previous month.
        
        month = androidMonthstart[i]+1  
        
        last = 0
        zerotime = datetime.datetime.strptime('00:00.00', '%M:%S.%f')
        
        for solution in thisCSVFile:
            if solution[0] == 'Date & Time':
                continue # Skip header line
            
            if int(solution[0][2:4]) > last:
                month -= 1
            last = int(solution[0][2:4])
            
            date = solution[0][5:9]+'-'+'{:02d}'.format(month)+'-'+solution[0][2:4]+' '+solution[0][10:]
            start = datetime.datetime.strptime(date, '%Y-%m-%d %I:%M:%S %p')
            
            time = (datetime.datetime.strptime(solution[1], '%M:%S.%f') - zerotime)
            
            allSolutions.append([start, time, androidcategories[i], 0])
            categories.append(androidcategories[i])
            
#=======================================================================================================================
# Summary
#=======================================================================================================================

uniqueCategories = sorted(list(set(categories)))
print('Categories\n----------\n    '+'\n    '.join(uniqueCategories),'\n\n')

print('Read in', len(allSolutions), 'solutions')
    
#=======================================================================================================================
# Function for reuse
#=======================================================================================================================
            
def extractandplot(select, eventId, maxTime, minTime, secondSpacingMajor, secondSpacingMinor, datelimits, monthSpacingMajor, monthSpacingMinor):

    #=======================================================================================================================
    # Exctract data for current event
    #=======================================================================================================================
           
    theseSolutions = [sol for sol in allSolutions if sol[2] == select]
    print('Selected',len(theseSolutions),'solutions for',select)
    
    theseSolutions.sort(key=lambda x: x[0])
    
    #=======================================================================================================================
    # Calculate averages
    #=======================================================================================================================
        
    print('\nCalculating averages for',select)
    print('\n')
    
    averages = {5:[[],[]]
    #             12:[[],[]]
                }
    
    means = {1:[[],[]],
    #          3:[[],[]], 
    #          10:[[],[]], 
    #          25:[[],[]], 
    #          50:[[],[]], 
            100:[[],[]],
    #          500:[[],[]],
            1000:[[],[]]
    #          5000:[[],[]],
    #          10000:[[],[]]
             }
    
    pbs = {1:[[],[]],
    #        3:[[],[]],
            5:[[],[]],
    #        10:[[],[]],
    #        12:[[],[]],
    #        25:[[],[]], 
    #        50:[[],[]], 
    #         100:[[],[]],
    #        500:[[],[]],
    #         1000:[[],[]],
    #        5000:[[],[]],
    #        10000:[[],[]]
           }
        
    def sumt(times):
        total = datetime.timedelta(seconds=0)
        for t in times:
            total += t
        return total
                
    def average(times):
        return (sumt(times)-max(times)-min(times)) / (len(times)-2)
        
    def mean(times):
        return sumt(times) / len(times)
        
    def pbhist(data):
        pb = [[],[]]
        for i in range(len(data[0])):
            if i>0:
                pb[0].append(data[0][i])
                pb[1].append(min([data[1][i], pb[1][-1]]))
            else:
                pb = [[data[0][i]], [data[1][i]]]
        return pb
        
    
    for i in range(len(theseSolutions)):
        for ao in averages.keys():
            if i+1 >= ao: 
                times = [sol[1] for sol in theseSolutions[i-ao+1:i+1]]
                averages[ao][0].append(theseSolutions[i][0])
                averages[ao][1].append(average(times))
        for mo in means.keys():
            if i+1 >= mo: 
                times = [sol[1] for sol in theseSolutions[i-mo+1:i+1]]
                means[mo][0].append(theseSolutions[i][0])
                means[mo][1].append(mean(times))
        
        
    for ao in averages.keys():
        if len(averages[ao][1]) > 0:
            thistime = (min(averages[ao][1])+datetime.datetime.strptime('00:00.00', '%M:%S.%f')).strftime('%M:%S.%f')
            print('Best average of {:.0f}: '.format(ao) + thistime)
    for mo in means.keys():
        if len(means[mo][1]) > 0:        
            thistime = (min(means[mo][1])+datetime.datetime.strptime('00:00.00', '%M:%S.%f')).strftime('%M:%S.%f')
            print('Best mean of {:.0f}: '.format(mo) + thistime)
    
    
    #=======================================================================================================================
    # Fetch Official times
    #=======================================================================================================================
    
    print('\n\nReading official competition times for',select)
    
    connection = mysql.connector.connect(host = "localhost",user="root",passwd="root", db="mysql")
    cursor = connection.cursor()
    
    
    cursor.execute('SELECT DISTINCT eventId FROM Results;')
    allEvents = [elem for row in cursor.fetchall() for elem in row]
    print('Official Category IDs\n---------------------\n    '+'\n    '.join(sorted(allEvents)),'\n\n')
        
    sqlQuery = """
    SELECT 
        year, month, day, value1, value2, value3, value4, value5 
    FROM Results 
        LEFT JOIN Competitions 
        ON Results.competitionId=Competitions.id 
    WHERE personId="1234ABCD01" AND eventId="xxxxx";
    """
    
    cursor.execute(sqlQuery.replace('1234ABCD01', personId).replace('xxxxx', eventId))
    wcaResults = cursor.fetchall()
    
    connection.close()
    
    
    compdata = [[],[]]
    
    for result in wcaResults:
        for i in range(3,8):
            date = datetime.datetime(result[0],result[1],result[2])
            time = datetime.timedelta(seconds = result[i]/100)
            
            compdata[0].append(date)
            compdata[1].append(time)
                
    #=======================================================================================================================
    # Plot the graph
    #=======================================================================================================================
    
    print('\n\nPlotting graph for',select)
    
    def d(datestr):
        return datetime.datetime.strptime(datestr, "%Y-%m-%d")
    
    def t(timestr):
        return datetime.datetime.strptime(timestr, "%M:%S.%f")
    
    def val(times):
        zerotime = datetime.datetime.strptime('00:00.00', '%M:%S.%f')
        return list(map(lambda x: zerotime+x, times))
        
    
    fig = pylab.figure()
    fig.patch.set_facecolor([.1,.1,.1])
    
    graylevel = 0.5
    singlecol = [0,1,0]
    
    
    # Plot data
    
    if len(means[1][1])>10000:
        alpha = 0.2
    elif len(means[1][1])>1000:
        alpha = 0.3
    else:
        alpha = 0.8
            
    pylab.plot(means[1][0], val(means[1][1]),'.',mfc=singlecol, mec=[0,0,0], label="Single times", alpha=alpha)
    pylab.plot(means[100][0], val(means[100][1]),'-', color=[0,1,0], linewidth=3, label="Mean of 100")
    pylab.plot(means[1000][0], val(means[1000][1]),'-', color=[1,0,0], linewidth=2, label="Mean of 1000")
    
    single = pbhist(means[1])
    pylab.plot(single[0], val(single[1]), '-', color=[0,1,.5], linewidth=1, label="Best Single")
    
    ao5 = pbhist(averages[5])
    pylab.plot(ao5[0], val(ao5[1]), 'b-', linewidth=1, label="PB Avg of 5")
    
    pylab.plot(compdata[0], val(compdata[1]),'o',mfc=[1,.5,0], mec="black", label="Official competition times")
    
    # Graph appearance
    
    pylab.grid('on')
    leg = pylab.legend(loc="upper right", fontsize=9)#, fancybox=True)
    leg.get_frame().set_facecolor([.3,.3,.3])
    leg.get_frame().set_alpha(0.5)
    for text in leg.get_texts():
        text.set_color("white")
    
    pylab.grid(b=True, which='major', color=[.3,.3,.3], linestyle='-', linewidth=1)
    pylab.grid(b=True, which='minor', color=[.3,.3,.3], linestyle=':')
    
    ax1 = pylab.gca()
    ax1.set_axis_bgcolor([0.05,0.05,0.05])
    
    ax1.xaxis.set_major_formatter(pylab.DateFormatter('%b %Y'))
    ax1.xaxis.set_major_locator(pylab.MonthLocator(interval=monthSpacingMajor))
    ax1.xaxis.set_minor_formatter(pylab.DateFormatter(''))
    ax1.xaxis.set_minor_locator(pylab.MonthLocator(interval=monthSpacingMinor))
    
    ax1.yaxis.set_major_formatter(pylab.DateFormatter('%M:%S.00'))
    ax1.yaxis.set_major_locator(pylab.SecondLocator(interval=secondSpacingMajor))
    ax1.yaxis.set_minor_formatter(pylab.DateFormatter(''))
    ax1.yaxis.set_minor_locator(pylab.SecondLocator(interval=secondSpacingMinor))
    
    if datelimits:
        pylab.xlim([d(datelimits[0]),d(datelimits[1])])
    
    pylab.ylim([t(minTime),t(maxTime)])
    
    
    ax2 = pylab.gcf().add_subplot(111, sharey=ax1, frameon=False)
     
    ax2.yaxis.tick_right()
    ax2.xaxis.set_ticks([])
    
    ax1.tick_params(axis='both', which='major', labelsize=9, colors="white")
    ax1.tick_params(axis='both', which='minor', labelsize=9, colors="white")
    ax2.tick_params(axis='both', which='major', labelsize=9, colors="white")
    ax2.tick_params(axis='both', which='minor', labelsize=9, colors="white")
    
    
    pylab.title(select+' progress ('+str(len(means[1][1]))+' solves)', color="white")
    
    
    print('\n\nSaving and displaying graph')
    # fig.savefig('Images/times-'+eventId+'-'+datetime.date.today().strftime('%Y-%m-%d')+'.png', facecolor=fig.get_facecolor(), edgecolor='none')
    fig.savefig('Images/times-'+eventId+'.png', facecolor=fig.get_facecolor(), edgecolor='none')
    # pylab.show()
    print('\n\nDone.')

    
#=======================================================================================================================
# Loop and plot all
#=======================================================================================================================

# select, eventId, maxTime, minTime, secondSpacingMajor, secondSpacingMinor

inputs = [["Rubik's cube", '333', "1:00.00", "0:00.00", 10, 1, None, 4, 1],
          ["2x2x2 cube", '222', "0:16.00", "0:00.00", 2, 1, None, 4, 1],
          ["4x4x4 cube", '444', "4:00.00", "1:00.00", 30, 5, None, 4, 1],
          ["5x5x5 cube", '555', "6:00.00", "2:00.00", 30, 5, None, 3, 1],
          ["6x6x6 cube", '666', "12:00.00", "4:00.00", 60, 10, None, 3, 1],
          ["7x7x7 cube", '777', "20:00.00", "5:00.00", 120, 20, None, 3, 1],
          ["Rubik's cube one-handed", '333oh', "2:00.00", "0:00.00", 10, 2, None, 4, 1],
          ["Rubik's cube with feet", '333ft', "15:00.00", "1:00.00", 60, 20, ["2015-04-01", "2016-04-01"], 2, 1],
          ["Pyraminx", 'pyram', "0:30.00", "0:00.00", 5, 1, None, 4, 1],
          ["Skewb", 'skewb', "0:25.00", "0:00.00", 2, 1, None, 3, 1],
          ["Square-1", 'sq1', "3:00.00", "0:00.00", 20, 5, None, 2, 1],
          ["Megaminx", 'minx', "7:00.00", "2:00.00", 30, 5, None, 3, 1],
          ["Rubik's clock", 'clock', "1:00.00", "0:00.00", 10, 2, None, 3, 1]]

for args in inputs:
    extractandplot(*args)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
