---
title: mysql example crawler (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'crawler'

Functions in program: 
* `def fill_course_and_recommendable_table():`
* `def fill_major_table():`
* `def fill_term_table():`
* `def make_distinctCode(code, instructor):`
* `def parse_time_day(str):`
* `def isday(compday):`
* `def ready_parse(str):`
* `def parselink(tr):`
* `def parsebase(tr):`

Modules used in program: 
* `import re`
* `import requests`
* `import mysql.connector`

## python crawler

Python mysql example: crawler

```python
import mysql.connector
from mysql.connector import errorcode
from bs4 import BeautifulSoup
import requests
import re

days = {"셀":"D0", "월":"D1", "화":"D2", "수":"D3", "목":"D4", "금":"D5", "토":"D6"}


url = 'http://sugang.inha.ac.kr/sugang/SU_51001/Lec_Time_Search.aspx'
r = requests.get(url)

soup = BeautifulSoup(r.text,'html.parser')
select = soup.find('select', id='ddlDept')
options = select.find_all('option')

majors = []

for o in options:
    majors.append({
        'name': o.text,
        'value': o['value']
    })

soup = BeautifulSoup(r.text,'html.parser')
select = soup.find('select', id='ddlKita')
options = select.find_all('option')

rests = []

for o in options:
    rests.append({
        'name': o.text,
        'value': o['value']
    })

# mysql connect
try:
    cnn = mysql.connector.connect(
        user = 'root',
        password = '**',
        database = 'inhatime'
    )
    print("It works")

except mysql.connector.Error as err:
    if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
        print("something is wrong with user or password")
    elif err.errno == errorcode.ER_BAD_DB_ERROR:
        print("Database Does not exist")
    else:
        print(err)

cursor = cnn.cursor(buffered=True)

def parsebase(tr):
    data = []
    tds = tr.find_all('td')
    for td in tds:
        data.append(td.text)
    return data


def parselink(tr):
    tds = tr.find('a')
    tds = str(tds)
    data = re.findall(r"'(.*)'", tds)
    data = str(data).replace("'","").replace("[","").replace("]","")
    return data


def ready_parse(str):
    br = str.strip()
    for day in days:
        for key in day:
            br = br.replace(key, key + ",")
    return br


def isday(compday):
    for day in days:
        for key in day:
            if compday == key:
                return True
    return False


def parse_time_day(str):
    ready_p = ready_parse(str)
    ans = {}
    btr = re.split(r"[\:\,\.\(\)\/\s]", ready_p)
    day_start = False
    bring_day = ""
    bring_time = ""
    bring_class = ""
    dayday = []
    classclass = []
    timetime = []

    for elem in btr:
        if elem == '':
            continue
        if isday(elem) == True:
            if day_start == False:
                day_start = True
            else:
                dayday.append(bring_day)
                timetime.append(bring_time)
                bring_time = ""
            bring_day = days[elem]
            continue
        if day_start == True:
            if elem.isdigit():
                bring_time = bring_time + "T" + elem
                # 숫자일 경우
            else:
                bring_class = elem
                dayday.append(bring_day)
                timetime.append(bring_time)
                for i in range(0, len(timetime) - len(classclass)):
                    classclass.append(bring_class)
                bring_day = ""
                bring_time = ""
                day_start = False
                # 장소일 경우

    if len(classclass) == 0:
        for a in range(0, len(dayday)):
            classclass.append('장소미정')

    for idx, a in enumerate(dayday):
        dd = dayday[idx]
        tt = timetime[idx]
        res1 = dd + tt
        ans[res1] = classclass[idx]
    return ans


def make_distinctCode(code, instructor):
    sub = code[0:8] + instructor
    return sub


def fill_term_table():
    year = 2016
    seme = ["spring","summer","fall","winter"]
    for a in range(0,10):
        for ss in seme:
            Query = """INSERT INTO Terms (year, semester) VALUES (%s, %s)"""
            cursor.execute(Query, (year, ss))
            cnn.commit()
        year = year + 1

def fill_major_table():
    Query = "INSERT INTO Majors (title) VALUES (%s)"
    for dept in majors:
        cursor.execute(Query, (dept["name"],))
        cnn.commit()
    for dept in rests:
        cursor.execute(Query, (dept["name"],))
        cnn.commit()



event_target = ''
event_argument = ''
last_focus = ''
view_state = soup.find('input',id='__VIEWSTATE')['value']
view_state_generator = soup.find('input',id='__VIEWSTATEGENERATOR')['value']
event_validation = soup.find('input',id='__EVENTVALIDATION')['value']


# rdoKwamokGubun : 99 전체 / 06 전선 / 05 전필 / 02 교선 / 01  교필 / 09 일선 / 08 교직

def fill_course_and_recommendable_table():
    for dept in majors:
        r = requests.post(url,data={
            'ddlDept':dept['value'],
            '__EVENTTARGET': event_target,
            '__EVENTARGUMENT': event_argument,
            '__LASTFOCUS': last_focus,
            '__VIEWSTATE': view_state,
            '__VIEWSTATEGENERATOR': view_state_generator,
            '__EVENTVALIDATION': event_validation,
            'rdoKwamokGubun': '99',
            'ibtnSearch1.x': '48',
            'ibtnSearch1.y': '12',
            'ibtnSearch1': '조회'
        })
        soup = BeautifulSoup(r.text, "html.parser")
        table = soup.find('table', id='dgList')
        trs = table.find_all('tr')
        for tr in trs:
            timeclass = ""
            code = ""
            split_class = ""
            title = ""
            grade = ""
            credit = 0
            class_type = ""
            instructor = ""
            eval = ""
            etc = ""
            pr = parsebase(tr)
            sub_link = parselink(tr)
            for idx, res in enumerate(pr):
                if idx == 0:
                    code = str(res)
                elif idx == 1:
                    split_class = str(res)
                elif idx == 2:
                    title = str(res)
                elif idx == 3:
                    grade = str(res)
                elif idx == 4:
                    credit = int(float(res))
                elif idx == 5:
                    class_type = str(res)
                elif idx == 6:
                    result = parse_time_day(res)
                    for key in result:
                        timeclass = timeclass + key + ":" + result[key] + ";"
                elif idx == 7:
                    instructor = str(res)
                elif idx == 8:
                    eval = str(res)
                elif idx == 9:
                    etc = str(res)

            # Courses 채우기
            termIds = cursor.execute("SELECT id FROM Terms where year = '2017' and semester = 'spring'")
            termId = cursor.fetchone()

            Query = """INSERT INTO  courses (grade, credit, classType, time, instructor, eval, etc, code, title, splitClass, active, url, TermId)
                        VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s)"""
            cursor.execute(Query, (grade, credit, class_type, timeclass, instructor, eval,etc,code, title, split_class, 1, url, termId[0]))

            cnn.commit()

            # MajorCourse 테이블 채우기
            lastId = cursor.lastrowid
            deptIds = cursor.execute("SELECT id FROM Majors where title = '%s'" % dept["name"])
            deptId = cursor.fetchone()

            Query = "INSERT INTO  MajorCourses (CourseId, MajorId) VALUES (%s, %s)"
            cursor.execute(Query, (lastId, deptId[0]))

            cnn.commit()

            # Recommendable table채우기
            distinctcode = make_distinctCode(code, instructor)
            isExists = cursor.execute("SELECT id FROM RecommendableCourses where distinctCode = '%s'" % distinctcode)
            isThere = cursor.fetchone()

            if isThere == None:
                Query = """INSERT INTO  RecommendableCourses (title, eval, instructor, grade, credit, classType, distinctCode, active)
                                       VALUES (%s, %s, %s, %s, %s, %s, %s, %s)"""
                cursor.execute(Query, (title, eval, instructor, grade, credit, class_type, distinctcode, 1))
                cnn.commit()


    for rest in rests:
        r = requests.post(url, data={
            'ddlKita':rest['value'],
            '__EVENTTARGET': event_target,
            '__EVENTARGUMENT': event_argument,
            '__LASTFOCUS': last_focus,
            '__VIEWSTATE': view_state,
            '__VIEWSTATEGENERATOR': view_state_generator,
            '__EVENTVALIDATION': event_validation,
            'rdoKwamokGubun': '99',
            'ibtnSearch2.x': '48',
            'ibtnSearch2.y': '12',
            'ibtnSearch2': '조회'
        })
        soup = BeautifulSoup(r.text, "html.parser")
        table = soup.find('table', id='dgList')
        trs = table.find_all('tr')
        year = "2017"
        semester = "spring"
        for tr in trs:
            timeclass = ""
            code = ""
            split_class = ""
            title = ""
            grade = ""
            credit = 0
            class_type = ""
            instructor = ""
            eval = ""
            etc = ""
            pr = parsebase(tr)
            sub_link = parselink(tr)
            for idx, res in enumerate(pr):
                if idx == 0:
                    code = str(res)
                elif idx == 1:
                    split_class = str(res)
                elif idx == 2:
                    title = str(res)
                elif idx == 3:
                    grade = str(res)
                elif idx == 4:
                    credit = int(float(res))
                elif idx == 5:
                    class_type = str(res)
                elif idx == 6:
                    result = parse_time_day(res)
                    for key in result:
                        timeclass = timeclass + key + ":" + result[key] + ";"
                elif idx == 7:
                    instructor = str(res)
                elif idx == 8:
                    eval = str(res)
                elif idx == 9:
                    etc = str(res)


            # Courses 채우기
            termIds = cursor.execute("SELECT id FROM Terms where year = '2017' and semester = 'spring'")
            termId = cursor.fetchone()
            Query = """INSERT INTO  courses (grade, credit, classType, time, instructor, eval, etc, code, title, splitClass, active, url, TermId)
                                   VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s)"""
            cursor.execute(Query, (grade, credit, class_type, timeclass, instructor, eval, etc, code, title, split_class, 1, url, termId[0]))

            cnn.commit()

            # MajorCourse 테이블 채우기
            lastId = cursor.lastrowid
            deptIds = cursor.execute("SELECT id FROM Majors where title = '%s'" % dept["name"])
            deptId = cursor.fetchone()

            Query = "INSERT INTO  MajorCourses (CourseId, MajorId) VALUES (%s, %s)"
            cursor.execute(Query, (lastId, deptId[0]))

            cnn.commit()

            # Recommendable table채우기

            distinctcode = make_distinctCode(code,instructor)
            isExists = cursor.execute("SELECT id FROM RecommendableCourses where distinctCode = '%s'" % distinctcode)
            isThere = cursor.fetchone()
            if isThere == None:
                Query = """INSERT INTO  RecommendableCourses (title, eval, instructor, grade, credit, classType, distinctcode, active)
                                       VALUES (%s, %s, %s, %s, %s, %s, %s, %s)"""
                cursor.execute(Query, (title, eval, instructor, grade, credit, class_type, etc, 1))
                cnn.commit()



# 1번 fill_term_table
fill_term_table()
# 2번 fill_major_table
fill_major_table()
# 3번 fill_course_recommendable_table
fill_course_and_recommendable_table()

cnn.close()


# 0 과목코드 / 1 분반그룹 / 2 과목명 / 3 학년 / 4 학점 / 5 과목구분 / 6 요일 및 강의실 / 7 담당교수 / 8 평가방식 / 9 비고
            # 10 ? / 11 ? / 12 요일 시간 / 13 ?

            # D0 : 웹강
            # D1 ~ D7 : 월 ~ 금
            # T1 ~ T25 : 1교시 ~ 25교시


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
