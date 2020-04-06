---
title: mysql example mongo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mongo'

Functions in program: 
* `def find_Latest(chid):`
* `def find_WatchedNum(chid):`
* `def insert_to_sql():`

Modules used in program: 
* `import json`
* `import requests`
* `import _mysql`
* `import sys`

## python mongo

Python mysql example: mongo

```python
#coding=utf8
import sys
import _mysql
import requests
from pymongo import MongoClient
import json

# SELECT *
# FROM  `group_strong`
# WHERE cid =1131
# ORDER BY TIME DESC

#file_name = sys.argv[1]+".csv"
courses = [986,989,996,999,1000,1026,1029,1031,1041,1131,1136]
client = MongoClient()
client = MongoClient("mongodb://104.155.227.109:27017")
db_mongo = client['test']
def insert_to_sql():

    # db=_mysql.connect(host="104.199.207.25",user='root',
    #               passwd="netxtream",db="SignalSystem")
    for cid in courses:
        # cid = 986
        db1_sharecourse=_mysql.connect(host="173.194.241.28",user='signalsystem',passwd='netxtream',db='db_sharecourse')
        #db1_sharecourse=_mysql.connect(host="173.194.241.28",user='Data-center',passwd='hsnl33564',db='db_sharecourse')
        #db1_sharecourse=_mysql.connect(host="173.194.241.28",user='keyword-cloud',passwd='hsnl33564',db='db_sharecourse')
        sql = "SELECT id FROM chapter WHERE cid = "+(str)(cid)+" AND deleted = 0"
        db1_sharecourse.query(sql)
        result=db1_sharecourse.store_result()
        chapter_list = result.fetch_row(0,1)
        print(chapter_list)
        db1_sharecourse.close()

        #done = 0

        for obj in chapter_list:
            #print((obj))
            chapterId = obj['id']
            print('----------------------')
            print(("Now cid = "+str(cid)))
            print(("Now chid = "+str(chapterId)))
            WatchedNum = find_WatchedNum(chapterId)
            # print('WatchedNum = ')
            # print(WatchedNum)
            print("Total "+str(WatchedNum[0])+" watched")
            if (WatchedNum[0]==0):
                continue
            lastest_list = find_Latest(chapterId)
            # print('lastest_list = ')
            #
            # print(lastest_list)

            if WatchedNum[0]!=0:
                db_test_m1=_mysql.connect(host="104.199.207.25",user='root',passwd='netxtream',db='SignalSystem')
                #db_test_m1=_mysql.connect(host="104.199.207.25",user='root',passwd='netxtream',db='SignalSystem')

                sql2 = ("SELECT * FROM SignalSystem.Signal_info WHERE chid ="+str(chapterId))
                db_test_m1.query(sql2)
                result2=db_test_m1.store_result()
                if(len(result2.fetch_row(0,1))==0):
                    print(("DB沒有要新增"))
                    sql1 = ("INSERT INTO  SignalSystem.Signal_info (cid ,chid ,watched_number ,name1 ,name2 ,name3 ,name4 ,lastest_time)"+
                        "VALUES ("+str(cid)+","+str(chapterId)+","+str(WatchedNum[0])+",\""+str(lastest_list[0])+"\",\""+str(WatchedNum[1])+"\",\""+str(WatchedNum[2])+"\",\""+ str(WatchedNum[3])+"\",\""+str(lastest_list[1])+"\")")
                    print((sql1))
                    db_test_m1.set_character_set('utf8')
                    db_test_m1.query(sql1)
                    db_test_m1.close()
                else:
                    print(("DB有要update"))
                    sql1 = ("UPDATE Signal_info SET cid ="+str(cid)+",chid ="+str(chapterId)+",watched_number ="+str(WatchedNum[0])+",name1 = \""+str(lastest_list[0])+"\""+",name2 =  \""+str(WatchedNum[1])+"\""+",name3 =  \""+
                    str(WatchedNum[2])+"\""+",name4 =  \""+str(WatchedNum[3])+"\""+", lastest_time =  \""+str(lastest_list[1])+"\" WHERE chid="+str(chapterId)+" AND cid = "+str(cid))
                    print((sql1))
                    db_test_m1.set_character_set('utf8')
                    db_test_m1.query(sql1)
                    db_test_m1.close()
            else:
                continue

# sql = " INSERT INTO group_strong VALUES (666,666) "
def find_WatchedNum(chid):
    cursor = db_mongo.video_records.distinct("userId",{'chapterId':int(chid),'action':'play'})
    count = len(cursor)
    return_list =[]
    return_list.append(count)
    if count ==2:
        for index in range(2):
            request_url = 'http://www.sharecourse.net/sharecourse/api/android/getUserInfo/' + str(cursor[index]);
            r = requests.get(request_url)
            return_list.append(r.json()['show_name'])
        return_list.append('none')
    elif count ==1:
        for index in range(1):
            request_url = 'http://www.sharecourse.net/sharecourse/api/android/getUserInfo/' + str(cursor[index]);
            r = requests.get(request_url)
            return_list.append(r.json()['show_name'])
        for index in range(2):
            return_list.append('none')
    elif count ==0:
        for index in range(3):
            return_list.append('none')
    elif count >=3:
        for index in range(3):
            request_url = 'http://www.sharecourse.net/sharecourse/api/android/getUserInfo/' + str(cursor[index]);
            r = requests.get(request_url)
            return_list.append(r.json()['show_name'])
    print((return_list))
    return return_list



def find_Latest(chid):
    cursor = list(db_mongo.video_records.find({'chapterId':int(chid),'action':'play'}).sort([('time',-1)]).limit(1))
    return_list =[]
    if len(cursor) ==0:
        return_list.append('none')
        return_list.append('none')
    else:
        for d in cursor:
            request_url = 'http://www.sharecourse.net/sharecourse/api/android/getUserInfo/' + str(d['userId']);
            r = requests.get(request_url)
            print((r.json()))
            return_list.append(r.json()['show_name'])
            return_list.append(cursor[0]['time'])
    print((return_list))
    return return_list
insert_to_sql()
# find_Latest(6356)
# find_WatchedNum(6356)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
