---
title: mysql example School Management (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'School Management'

Functions in program: 
* `def edit_staff():`
* `def edit_student():`
* `def staff_enquiry():`
* `def student_enquiry():`

Modules used in program: 
* `import tkinter as tk`
* `import mysql.connector as sq`

## python School Management

Python mysql example: School Management

```python
import mysql.connector as sq
import tkinter as tk
std = sq.connect(host="localhost", user="root", passwd="1234", database="sakila")
if std.is_connected() == False:
    print("Not connected to database")
cursor = std.cursor()
cursor.execute("create table  if not exists basic_info_highschool(sid int(11) NOT NULL PRIMARY KEY,father char(50),"
               "mother char(50),class char(3) NOT NULL,sports char(50),sex char(1) NOT NULL,name char(50) NOT NULL,"
               "dob char(100) NOT NULL)")
cursor.execute("create table  if not exists basic_info_intermidiate(sid int(11) NOT NULL PRIMARY KEY,father char(50),"
               "mother char(50),class char(3) NOT NULL,sex char(1) NOT NULL,stream char(20) NOT NULL,sports char(50),"
               "name char(50) NOT NULL,dob char(100) NOT NULL)")
cursor.execute("create table  if not exists class_highschool(class char(3) NOT NULL PRIMARY KEY,class_teacher char(50) "
               "NOT NULL,class_teacherID int(20) UNIQUE,strength int(100) NOT NULL)")
cursor.execute("create table  if not exists class_intermidiate(class char(3) NOT NULL PRIMARY KEY,class_"
               "teacher char(50) NOT NULL,class_teacherID int(20) NOT NULL UNIQUE,stream char(20) NOT NULL,"
               "strength int(100) NOT NULL)")
cursor.execute("create table  if not exists teaching_staff(tid int(20) NOT NULL PRIMARY KEY,paygrade char(1),"
               "subject char(50) NOT NULL,sex char(1) NOT NULL,tname char(50) NOT NULL,dob char(100) NOT NULL)")
cursor.execute("create table  if not exists teaching_staff(stid int(20) NOT NULL PRIMARY KEY,stname char(50),"
               "designation char(70) NOT NULL,salary char(100) NOT NULL,sex char(1) NOT NULL,dob char(50) NOT NULL)")
std.commit()
w = tk.Tk()
def student_enquiry():
    box1=tk.Tk()
    def highschool():
        box2=tk.Tk()
        def student1():
            box3=tk.Tk()
            tk.Label(box3,text="Enter student name -->").grid(row=0,column=0)
            tk.Label(box3,text="Enter student ID -->").grid(row=1,column=0)
            def choice1():
                ch1=name1.get()
                ch2=sid1.get()
                show1="SELECT* FROM basic_info_highschool WHERE sid=%s and name='%s'"%(ch2,ch1)
                cursor.execute(show1)
                list1=["Student ID -- ","Father's Name -- ",
                    "Mother's Name -- ","Class -- ","Sports -- ","Sex -- ","Name -- ","DOB --"]
                data1=cursor.fetchall()
                for row in data1:
                    length1=len(row)
                    rowcount1 = 4
                    for i in range(0,length1):
                        tk.Label(box3,text=list1[i],font="cooperblack").grid(row=rowcount1,column=0)
                        tk.Label(box3,text=row[i],font="copperblack").grid(row=rowcount1,column=1)
                        rowcount1 +=1
                tk.Button(box3,text="QUIT",bg="red",font="arialblack",command=box3.quit).grid(row=2,column=1)
            name1=tk.Entry(box3,width=50)
            name1.grid(row=0,column=1)
            sid1=tk.Entry(box3,width=50)
            sid1.grid(row=1,column=1)
            tk.Button(box3,text="Next",bg="green",font="arialblack",command=choice1).grid(row=2,column=0)
            box3.mainloop()
        def classes1():
            box4=tk.Tk()
            tk.Label(box4,text="Enter class -->").grid(row=0)
            def choice2():
                ch3=class_name1.get()
                show3="SELECT* FROM class_highschool WHERE class='%s'"%(ch3)
                cursor.execute(show3)
                list3=["Class -- ","Class Teacher -- ","Class Teacher ID -- ","Strength -- "]
                data3=cursor.fetchall()
                for row in data3:
                    length2=len(row)
                    rowcount3=4
                    for i in range(0,length2):
                        tk.Label(box4, text=list3[i], font="cooperblack").grid(row=rowcount3, column=0)
                        tk.Label(box4, text=row[i], font="copperblack").grid(row=rowcount3, column=1)
                        rowcount3+=1
                tk.Button(box4, text="QUIT", command=box4.quit, bg="red").grid(row=2, column=1)
            class_name1=tk.Entry(box4,width=20)
            class_name1.grid(row=0,column=1)
            tk.Button(box4,text="Next",command=choice2).grid(row=2,column=0)
            box4.mainloop()
        tk.Checkbutton(box2, text="Student details", command=student1).grid(row=0)
        tk.Checkbutton(box2, text="Class details", command=classes1).grid(row=1)
        box2.mainloop()

    def intermidiate():
        box5= tk.Tk()
        def student2():
            box6= tk.Tk()
            tk.Label(box6, text="Enter student name -->").grid(row=0, column=0)
            tk.Label(box6, text="Enter student ID -->").grid(row=1, column=0)
            def choice3():
                ch4 = name2.get()
                ch5 = sid2.get()
                show2= "SELECT* FROM basic_info_intermidiate WHERE sid=%s and name='%s'" % (ch5, ch4)
                cursor.execute(show2)
                list2 = ["Student ID -- ", "Father's Name -- ",
                         "Mother's Name -- ", "Class -- ", "Sex -- ","Stream--", "Sports-- ", "Name -- ", "DOB --"]
                data2 = cursor.fetchall()
                for row in data2:
                    length3 = len(row)
                    rowcount2 = 4
                    for i in range(0, length3):
                        tk.Label(box6, text=list2[i], font="cooperblack").grid(row=rowcount2, column=0)
                        tk.Label(box6, text=row[i], font="copperblack").grid(row=rowcount2, column=1)
                        rowcount2 += 1
                tk.Button(box6, text="QUIT", bg="red", font="arialblack", command=box6.quit).grid(row=2, column=1)
            name2= tk.Entry(box6, width=50)
            name2.grid(row=0, column=1)
            sid2= tk.Entry(box6, width=50)
            sid2.grid(row=1, column=1)
            tk.Button(box6, text="Next", command=choice3).grid(row=2)
            box6.mainloop()
        def classes2():
            box7= tk.Tk()
            tk.Label(box7, text="Enter class -->").grid(row=0)
            def choice4():
                ch6 = class_name2.get()
                show4 = "SELECT* FROM class_intermidiate WHERE class='%s'" % (ch6)
                cursor.execute(show4)
                list4 = ["Class -- ", "Class Teacher -- ", "Class Teacher ID -- ","Stream -- ", "Strength -- "]
                data4 = cursor.fetchall()
                for row in data4:
                    length4 = len(row)
                    rowcount4 = 4
                    for i in range(0, length4):
                        tk.Label(box7, text=list4[i], font="cooperblack").grid(row=rowcount4, column=0)
                        tk.Label(box7, text=row[i], font="copperblack").grid(row=rowcount4, column=1)
                        rowcount4 += 1
                tk.Button(box7,text="QUIT",command=box7.quit,bg="red").grid(row=2,column=1)
            class_name2= tk.Entry(box7, width=20)
            class_name2.grid(row=0, column=1)
            tk.Button(box7, text="Next", command=choice4).grid(row=2,column=0)
            box7.mainloop()
        tk.Checkbutton(box5, text="Student details", command=student2).grid(row=0)
        tk.Checkbutton(box5, text="Class details", command=classes2).grid(row=1)
        box5.mainloop()
    tk.Checkbutton(box1,text="Highschool",command=highschool).grid(row=0)
    tk.Checkbutton(box1, text="Intermidiate", command=intermidiate).grid(row=1)
    box1.mainloop()
def staff_enquiry():
    box8=tk.Tk()
    def ten():
        box23=tk.Tk()
        tk.Label(box23,text="Teacher ID -->").grid(row=0,column=0)
        tk.Label(box23, text="Teacher Name -->").grid(row=1, column=0)
        def ten1():
            en30=tid.get()
            en31=tname.get()
            show5="SELECT* FROM teaching_staff WHERE tid=%s and tname='%s'"%(en30,en31)
            cursor.execute(show5)
            list5=["Teacher ID -- ","Paygrade -- ","Subject -- ","Sex -- ","Teacher Name -- ","DOB -- "]
            data5=cursor.fetchall()
            for row in data5:
                length5=len(row)
                rowcount5=4
                for i in range(0,length5):
                    tk.Label(box23, text=list5[i], font="cooperblack").grid(row=rowcount5, column=0)
                    tk.Label(box23, text=row[i], font="copperblack").grid(row=rowcount5, column=1)
                    rowcount5+=1
            tk.Button(box23,text="QUIT",command=box23.quit,bg="red").grid(row=2,column=1)
        tid=tk.Entry(box23,width=60)
        tid.grid(row=0,column=1)
        tname=tk.Entry(box23,width=60)
        tname.grid(row=1,column=1)
        tk.Button(box23,text="Next",command=ten1).grid(row=2,column=0)
    def nten():
        box24 = tk.Tk()
        tk.Label(box24, text="Teacher ID -->").grid(row=0, column=0)
        tk.Label(box24, text="Staff Name -->").grid(row=1, column=0)

        def nten1():
            en32 = tid.get()
            en33 = tname.get()
            show6 = "SELECT* FROM nonteaching_staff WHERE stid=%s and stname='%s'"%(en32, en33)
            cursor.execute(show6)
            list6 = ["Staff ID -- ", "Staff Name -- ", "Designation -- ", "Salary -- ", "Sex -- ", "DOB -- "]
            data6 = cursor.fetchall()
            for row in data6:
                length6 = len(row)
                rowcount6 = 4
                for i in range(0,length6):
                    tk.Label(box24, text=list6[i], font="cooperblack").grid(row=rowcount6, column=0)
                    tk.Label(box24, text=row[i], font="copperblack").grid(row=rowcount6, column=1)
                    rowcount6 += 1
            tk.Button(box24, text="QUIT", command=box24.quit, bg="red").grid(row=2, column=1)

        tid = tk.Entry(box24, width=60)
        tid.grid(row=0, column=1)
        tname = tk.Entry(box24, width=60)
        tname.grid(row=1, column=1)
        tk.Button(box24, text="Next", command=nten1).grid(row=2, column=0)
    tk.Checkbutton(box8,text="Teaching Staff",command=ten).grid(row=0,column=0)
    tk.Checkbutton(box8, text="Non-Teaching Staff", command=nten).grid(row=1, column=0)
    box8.mainloop()
def edit_student():
    box9=tk.Tk()
    def add1():
        box10=tk.Tk()
        def addstu1():
            box11=tk.Tk()
            tk.Label(box11,text="Add student data",bg="magenta").grid(row=0,column=0)
            tk.Label(box11,text="Student ID -- ").grid(row=2,column=0)
            tk.Label(box11,text="Student Name -- ").grid(row=3,column=0)
            tk.Label(box11,text="Father's Name -- ").grid(row=4,column=0)
            tk.Label(box11,text="Mother's Name --" ).grid(row=5,column=0)
            tk.Label(box11,text="Class -- ").grid(row=6,column=0)
            tk.Label(box11,text="Sex -- ").grid(row=7,column=0)
            tk.Label(box11,text="Sports -- ").grid(row=8,column=0)
            tk.Label(box11,text="DOB -- ").grid(row=9,column=0)
            def stu1():
                en1=sid1.get()
                en2=fname1.get()
                en3=mname1.get()
                en4=classname1.get()
                en5=sports1.get()
                en6=sex1.get()
                en7=sname1.get()
                en8=dob1.get()
                ins1="INSERT INTO basic_info_highschool(sid,father,mother,class,sports,sex,name,dob)" \
                        "VALUES({},'{}','{}','{}','{}','{}','{}','{}')".format(en1,en2,en3,en4,en5,en6,en7,en8)
                cursor.execute(ins1)
                std.commit()
                tk.Button(box11,text="QUIT",bg="red",command=box11.quit).grid(row=10,column=1)
                tk.Label(box11, text="**Successfully Enrolled**", bg="light blue").grid(row=11, column=1)
            sid1=tk.Entry(box11,width=60)
            sid1.grid(row=2,column=1)
            sname1=tk.Entry(box11,width=60)
            sname1.grid(row=3,column=1)
            fname1=tk.Entry(box11,width=60)
            fname1.grid(row=4,column=1)
            mname1=tk.Entry(box11,width=60)
            mname1.grid(row=5,column=1)
            classname1=tk.Entry(box11,width=60)
            classname1.grid(row=6,column=1)
            sex1=tk.Entry(box11,width=60)
            sex1.grid(row=7,column=1)
            sports1=tk.Entry(box11,width=60)
            sports1.grid(row=8,column=1)
            dob1=tk.Entry(box11,width=60)
            dob1.grid(row=9,column=1)
            tk.Button(box11,text="SUBMIT",command=stu1,bg="blue").grid(row=10,column=0)
            box11.mainloop()
        def addcla1():
            box14=tk.Tk()
            tk.Label(box14,text="Class --").grid(row=0,column=0)
            tk.Label(box14, text="Class Teacher --").grid(row=1, column=0)
            tk.Label(box14, text="Class Teacher ID--").grid(row=2, column=0)
            tk.Label(box14, text="Strength --").grid(row=3, column=0)
            def cla1():
                en18=classs1.get()
                en19=classt1.get()
                en20=classtid1.get()
                en21=strength1.get()
                ins5 = "INSERT INTO class_highschool(class,class_teacher,class_teacherID,strength)" \
                           "VALUES('{}','{}',{},{})".format(en18, en19, en20, en21)
                cursor.execute(ins5)
                std.commit()
                tk.Button(box14, text="QUIT", bg="red", command=box14.quit).grid(row=4, column=1)
                tk.Label(box14, text="**Successfully Enrolled**", bg="light blue").grid(row=5, column=1)
            classs1=tk.Entry(box14,width=60)
            classs1.grid(row=0,column=1)
            classt1=tk.Entry(box14,width=60)
            classt1.grid(row=1,column=1)
            classtid1=tk.Entry(box14,width=60)
            classtid1.grid(row=2,column=1)
            strength1=tk.Entry(box14,width=60)
            strength1.grid(row=3,column=1)
            tk.Button(box14,text="SUBMIT",command=cla1,bg="blue").grid(row=4,column=0)
            box14.mainloop()
        tk.Button(box10,text="Add student information",command=addstu1).grid(row=0,column=0)
        tk.Button(box10,text="Add class information",command=addcla1).grid(row=1,column=0)
        box10.mainloop()

    def add2():
        box12 = tk.Tk()
        def addstu2():
            box13 = tk.Tk()
            tk.Label(box13, text="Add student data", bg="magenta").grid(row=0)
            tk.Label(box13, text="Student ID -- ").grid(row=2, column=0)
            tk.Label(box13, text="Student Name -- ").grid(row=3, column=0)
            tk.Label(box13, text="Father's Name -- ").grid(row=4, column=0)
            tk.Label(box13, text="Mother's Name --").grid(row=5, column=0)
            tk.Label(box13, text="Class -- ").grid(row=6, column=0)
            tk.Label(box13, text="Sex -- ").grid(row=7, column=0)
            tk.Label(box13, text="Sports -- ").grid(row=8, column=0)
            tk.Label(box13,text="Stream -- ").grid(row=9,column=0)
            tk.Label(box13, text="DOB -- ").grid(row=10, column=0)

            def stu2():
                en9 = sid2.get()
                en10 = fname2.get()
                en11 = mname2.get()
                en12 = classname2.get()
                en13 = sex2.get()
                en14 = stream1.get()
                en15 = sports2.get()
                en16 = sname2.get()
                en17 = dob2.get()

                ins2= "INSERT INTO basic_info_intermidiate(sid,father,mother,class,sex,stream,sports,name,dob)" \
                      "VALUES({},'{}','{}','{}','{}','{}','{}','{}','{}')".format(en9, en10, en11, en12, en13, en14, en15, en16,en17)
                cursor.execute(ins2)
                std.commit()
                tk.Button(box13, text="QUIT", bg="red", command=box13.quit).grid(row=11, column=1)
                tk.Label(box13, text="**Successfully Enrolled**", bg="light blue").grid(row=12, column=1)
            sid2 = tk.Entry(box13, width=60)
            sid2.grid(row=2, column=1)
            sname2 = tk.Entry(box13, width=60)
            sname2.grid(row=3, column=1)
            fname2 = tk.Entry(box13, width=60)
            fname2.grid(row=4, column=1)
            mname2 = tk.Entry(box13, width=60)
            mname2.grid(row=5, column=1)
            classname2 = tk.Entry(box13, width=60)
            classname2.grid(row=6, column=1)
            sex2 = tk.Entry(box13, width=60)
            sex2.grid(row=7, column=1)
            sports2 = tk.Entry(box13, width=60)
            sports2.grid(row=8, column=1)
            stream1=tk.Entry(box13,width=60)
            stream1.grid(row=9,column=1)
            dob2 = tk.Entry(box13, width=60)
            dob2.grid(row=10, column=1)
            tk.Button(box13, text="SUBMIT", command=stu2,bg="blue").grid(row=11, column=0)
            box13.mainloop()

        def addcla2():
            box15 = tk.Tk()
            tk.Label(box15, text="Class --").grid(row=0, column=0)
            tk.Label(box15, text="Class Teacher --").grid(row=1, column=0)
            tk.Label(box15, text="Class Teacher ID--").grid(row=2, column=0)
            tk. Label(box15, text="Stream --").grid(row=3, column=0)
            tk.Label(box15, text="Strength --").grid(row=4, column=0)

            def cla2():
                en22 = classs2.get()
                en23 = classt2.get()
                en24 = classtid2.get()
                en25= stream1.get()
                en26 = strength2.get()
                ins6 = "INSERT INTO class_intermidiate(class,class_teacher,class_teacherID,stream,strength)" \
                       "VALUES('{}','{}',{},'{}',{})".format(en22, en23, en24, en25,en26)
                cursor.execute(ins6)
                std.commit()
                tk.Button(box15, text="QUIT", bg="red", command=box15.quit).grid(row=5, column=1)
                tk.Label(box15, text="**Successfully Enrolled**", bg="light blue").grid(row=6, column=1)
            classs2 = tk.Entry(box15, width=60)
            classs2.grid(row=0, column=1)
            classt2 = tk.Entry(box15, width=60)
            classt2.grid(row=1, column=1)
            classtid2 = tk.Entry(box15, width=60)
            classtid2.grid(row=2, column=1)
            stream1=tk.Entry(box15,width=60)
            stream1.grid(row=3,column=1)
            strength2 = tk.Entry(box15, width=60)
            strength2.grid(row=4, column=1)
            tk.Button(box15, text="SUBMIT", command=cla2,bg="blue").grid(row=5, column=0)
            box15.mainloop()
        tk.Button(box12, text="Add student information", command=addstu2).grid(row=0, column=0)
        tk.Button(box12,text="Add class information",command=addcla2).grid(row=1,column=0)
        box12.mainloop()
    def delete1():
        box16=tk.Tk()
        def studel1():
            box17=tk.Tk()
            tk.Label(box17,text="Student ID --").grid(row=0,column=0)
            tk.Label(box17,text="Student Name --").grid(row=1,column=0)
            def del1():
                ent13 = stid1.get()
                ent14 = stname1.get()
                dele1="DELETE FROM basic_info_highschool WHERE sid={}".format(ent13)
                cursor.execute(dele1)
                std.commit()
                tk.Button(box17, text="QUIT", command=box17.quit, bg="red").grid(row=2, column=1)
                tk.Label(box17,text="**Successfully deleted the student information!!**",bg="light blue").grid(row=3,column=1)
            stid1=tk.Entry(box17,width=60)
            stid1.grid(row=0,column=1)
            stname1=tk.Entry(box17,width=60)
            stname1.grid(row=1,column=1)
            tk.Button(box17,text="DELETE INFO",command=del1,bg="red").grid(row=2,column=0)
            box17.mainloop()
        def classdel1():
            box18=tk.Tk()
            tk.Label(box18,text="Class --").grid(row=0,column=0)
            def del2():
                ent15=cl1.get()
                dele2="DELETE FROM class_highschool WHERE class='{}'".format(ent15)
                cursor.execute(dele2)
                std.commit()
                tk.Label(box18,text="**Successfully deleted class information**",bg="light blue").grid(row=2,column=1)
                tk.Button(box18,text="QUIT",command=box18.quit,bg="red").grid(row=1,column=1)
            cl1=tk.Entry(box18,width=60)
            cl1.grid(row=0,column=1)
            tk.Button(box18,text="DELETE INFO",command=del2,bg="red").grid(row=1,column=0)
        tk.Checkbutton(box16, text="Delete Student Information", bg="yellow",command=studel1).grid(row=0, column=0)
        tk.Checkbutton(box16, text="Delete Class Information", bg="green",command=classdel1).grid(row=1, column=0)
        box16.mainloop()
    def delete2():
        box20=tk.Tk()

        def studel2():
            box21 = tk.Tk()
            tk.Label(box21, text="Student ID --").grid(row=0, column=0)
            tk.Label(box21, text="Student Name --").grid(row=1, column=0)

            def del3():
                ent16 = stid2.get()
                ent17 = stname2.get()
                dele3 = "DELETE FROM basic_info_intermidiate WHERE sid={}".format(ent16)
                cursor.execute(dele3)
                std.commit()
                tk.Button(box21, text="QUIT", command=box21.quit, bg="red").grid(row=2, column=1)
                tk.Label(box21, text="**Successfully deleted the student information!!**", bg="light blue").grid(row=3,
                                                                                                                 column=1)

            stid2 = tk.Entry(box21, width=60)
            stid2.grid(row=0, column=1)
            stname2 = tk.Entry(box21, width=60)
            stname2.grid(row=1, column=1)
            tk.Button(box21, text="DELETE INFO", command=del3, bg="red").grid(row=2, column=0)
            box21.mainloop()

        def classdel2():
            box22 = tk.Tk()
            tk.Label(box22, text="Class --").grid(row=0, column=0)

            def del4():
                ent18 = cl2.get()
                dele4 = "DELETE FROM class_highschool WHERE class='{}'".format(ent18)
                cursor.execute(dele4)
                std.commit()
                tk.Label(box22, text="**Successfully deleted class information**", bg="light blue").grid(row=2,column=1)
                tk.Button(box22, text="QUIT", command=box22.quit, bg="red").grid(row=1, column=1)

            cl2 = tk.Entry(box22, width=60)
            cl2.grid(row=0, column=1)
            tk.Button(box22, text="DELETE INFO", command=del4, bg="red").grid(row=1, column=0)

        tk.Checkbutton(box20, text="Delete Student Information", bg="yellow", command=studel2).grid(row=0, column=0)
        tk.Checkbutton(box20, text="Delete Class Information", bg="green", command=classdel2).grid(row=1, column=0)
        box20.mainloop()
    tk.Label(box9, text="Add a information",bg="violet").grid(row=0, column=0)
    tk.Checkbutton(box9,text="Highschool",command=add1).grid(row=1,column=0)
    tk.Checkbutton(box9,text="Intermediate",command=add2).grid(row=2,column=0)
    tk.Label(box9,text="Delete Info",bg="violet").grid(row=3,column=0)
    tk.Checkbutton(box9, text="Highschool", command=delete1).grid(row=4, column=0)
    tk.Checkbutton(box9, text="Intermediate", command=delete2).grid(row=5, column=0)
    box9.mainloop()


def edit_staff():
    h=tk.Tk()
    def add2():
        y=tk.Tk()
        def addstaff1():
            g=tk.Tk()
            tk.Label(g,text="Add  data",bg="magenta").grid(row=0)
            tk.Label(g,text="Staff ID -- ").grid(row=2,column=0)
            tk.Label(g,text="Staff Name -- ").grid(row=3,column=0)
            tk.Label(g,text="Paygrade -- ").grid(row=4,column=0)
            tk.Label(g,text="Subject --" ).grid(row=5,column=0)
            tk.Label(g,text="Sex -- ").grid(row=6,column=0)
            tk.Label(g,text="DOB -- ").grid(row=7,column=0)
            def sta1():
                ent1=tid.get()
                ent2=paygrade.get()
                ent3=tsubject.get()
                ent4=sex2.get()
                ent5=tname.get()
                ent6=dob2.get()
                ins3="INSERT INTO teaching_staff(tid,paygrade,subject,sex,tname,dob)" \
                     "VALUES({},'{}','{}','{}','{}','{}')".format(ent1,ent2,ent3,ent4,ent5,ent6)
                cursor.execute(ins3)
                std.commit()
                tk.Label(g, text="**Successfully Edited**", bg="light blue").grid(row=9, column=1)
                tk.Button(g, text="QUIT", bg="red", command=g.quit).grid(row=8, column=1)
            tid=tk.Entry(g,width=60)
            tid.grid(row=2,column=1)
            tname=tk.Entry(g,width=60)
            tname.grid(row=3,column=1)
            paygrade=tk.Entry(g,width=60)
            paygrade.grid(row=4,column=1)
            tsubject=tk.Entry(g,width=60)
            tsubject.grid(row=5,column=1)
            sex2=tk.Entry(g,width=60)
            sex2.grid(row=6,column=1)
            dob2=tk.Entry(g,width=60)
            dob2.grid(row=7,column=1)
            tk.Button(g,text="SUBMIT",command=sta1,bg="blue").grid(row=8,column=0)
            g.mainloop()
        def addstaff2():
            a=tk.Tk()
            tk.Label(a,text="Add  data",bg="magenta").grid(row=0)
            tk.Label(a,text="Staff ID -- ").grid(row=2,column=0)
            tk.Label(a,text="Staff Name -- ").grid(row=3,column=0)
            tk.Label(a,text="Designation -- ").grid(row=4,column=0)
            tk.Label(a,text="Salary --" ).grid(row=5,column=0)
            tk.Label(a,text="Sex -- ").grid(row=6,column=0)
            tk.Label(a,text="DOB -- ").grid(row=7,column=0)
            def sta2():
                ent7=stid.get()
                ent8=stname.get()
                ent9=desig.get()
                ent10=sal.get()
                ent11=sex.get()
                ent12=dob3.get()
                ins4="INSERT INTO nonteaching_staff(stid,stname,designation,salary,sex,dob)" \
                    "VALUES({},'{}','{}',{},'{}','{}')".format(ent7,ent8,ent9,ent10,ent11,ent12)
                cursor.execute(ins4)
                std.commit()
                tk.Label(a,text="**Successfully Edited**",bg="light blue").grid(row=11,column=1)
                tk.Button(a, text="QUIT", bg="red", command=a.quit).grid(row=10, column=1)
            stid=tk.Entry(a,width=60)
            stid.grid(row=2,column=1)
            stname=tk.Entry(a,width=60)
            stname.grid(row=3,column=1)
            desig=tk.Entry(a,width=60)
            desig.grid(row=4,column=1)
            sal=tk.Entry(a,width=60)
            sal.grid(row=5,column=1)
            sex=tk.Entry(a,width=60)
            sex.grid(row=6,column=1)
            dob3=tk.Entry(a,width=60)
            dob3.grid(row=7,column=1)
            tk.Button(a,text="SUBMIT",command=sta2,bg="blue").grid(row=10,column=0)
            a.mainloop()
        tk.Button(y,text="Add teaching staff information",command=addstaff1).grid(row=0,column=0)
        tk.Button(y, text="Add non teaching staff information", command=addstaff2).grid(row=1, column=0)
        y.mainloop()
    def delete3():
        box24=tk.Tk()

        def ten():
            box25 = tk.Tk()
            tk.Label(box25, text="Teacher ID --").grid(row=0, column=0)
            tk.Label(box25, text="Teacher Name --").grid(row=1, column=0)

            def ten1():
                en36 = tid.get()
                en37 = tname.get()
                del5 = "DELETE FROM teaching_staff WHERE tid=%s and tname='%s'" % (en36, en37)
                cursor.execute(del5)
                std.commit()
                tk.Label(box25, text="**Successfully deleted class information**", bg="light blue").grid(row=3,column=1)
                tk.Button(box25, text="QUIT", command=box25.quit, bg="red").grid(row=2, column=1)
            tid = tk.Entry(box25, width=60)
            tid.grid(row=0, column=1)
            tname = tk.Entry(box25, width=60)
            tname.grid(row=1, column=1)
            tk.Button(box25, text="Next", command=ten1).grid(row=2, column=0)

        def nten():
            box26 = tk.Tk()
            tk.Label(box26, text="Staff ID --").grid(row=0, column=0)
            tk.Label(box26, text="Staff Name --").grid(row=1, column=0)

            def nten1():
                en38 = stid.get()
                en39 = stname.get()
                del6 = "DELETE FROM nonteaching_staff WHERE stid=%s and stname='%s'" % (en38, en39)
                cursor.execute(del6)
                std.commit()
                tk.Label(box26, text="**Successfully deleted class information**", bg="light blue").grid(row=3,column=1)
                tk.Button(box26, text="QUIT", command=box26.quit, bg="red").grid(row=2, column=1)
            stid = tk.Entry(box26, width=60)
            stid.grid(row=0, column=1)
            stname = tk.Entry(box26, width=60)
            stname.grid(row=1, column=1)
            tk.Button(box26, text="Next", command=nten1).grid(row=2, column=0)

        tk.Checkbutton(box24, text="Teaching Staff", command=ten).grid(row=0, column=0)
        tk.Checkbutton(box24, text="Non-Teaching Staff", command=nten).grid(row=1, column=0)
    tk.Button(h,text="Add a information",command=add2).grid(row=0,column=0)
    tk.Button(h, text="Delete a information", command=delete3).grid(row=1, column=0)
    h.mainloop()
title=tk.Label(w,text="G.N NATIONAL PUBLIC SCHOOL",bg="skyblue",font="algerian").grid(row=0)
tk.Label(w,text="ENQUIRY",bg="yellow",font="jokerman").grid(row=2,column=0)
tk.Label(w,text="EDIT INFO",bg="yellow",font="jokerman").grid(row=5,column=0)
tk.Button(w,text='Student Enquiry',command=student_enquiry).grid(row=3,column=0)
tk.Button(w,text="Staff Enquiry",command=staff_enquiry).grid(row=4,column=0)
tk.Button(w,text="Edit student data",command=edit_student).grid(row=6,column=0)
tk.Button(w,text="Edit staff data",command=edit_staff).grid(row=7,column=0)
w.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
