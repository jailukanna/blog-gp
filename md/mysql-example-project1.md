---
title: mysql example project1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'project1'

Functions in program: 
* `def view_all():`
* `def view():`
* `def delete():`
* `def update():`
* `def add():`

Modules used in program: 
* `import mysql.connector as mariadb`

## python project1

Python mysql example: project1

```python
from tkinter import *

import mysql.connector as mariadb

def add():
    window = Tk()
    window.geometry("300x300")
    window.configure(bg='pink')
    lblTitle = Label(window, text="ADD CUSTOMER",font = "Arial,12",bg = "Yellow")
    lblTitle.pack()

    lblcust_id = Label(window, text="Enter Customer Id")
    lblcust_id.place(x = 30,y = 50)

    entrycust_id = Entry(window)
    entrycust_id.place(x = 160, y = 50)
    lblname = Label(window, text="Enter Customer Name")
    lblname.place(x = 30,y = 80)

    entryname = Entry(window)
    entryname.place(x = 160,y = 80)

    lblph_no = Label(window, text="Enter Customer Phone")
    lblph_no.place(x = 30,y = 110)

    entryph_no = Entry(window)
    entryph_no.place(x = 160,y = 110)

    lblemail = Label(window, text="Enter Customer Email")
    lblemail.place(x = 30,y = 140)

    entryemail = Entry(window)
    entryemail.place(x = 160,y = 140)

    lblgender = Label(window, text="Enter Customer Gender")
    lblgender.place(x = 30,y = 170)

    entrygender = Entry(window)
    entrygender.place(x = 160,y = 170)

    lbladdress = Label(window, text="Enter Customer Address")
    lbladdress.place(x = 30,y = 200)

    entryaddress = Entry(window)
    entryaddress.place(x = 160,y = 200)

    lblpayment_mode = Label(window, text="Enter Payment Mode")
    lblpayment_mode.place(x = 30,y = 230)

    entrypayment_mode = Entry(window)
    entrypayment_mode.place(x = 160,y = 230)

    def Save():
        cust_id = entrycust_id.get()
        name = entryname.get()
        ph_no = entryph_no.get()
        email = entryemail.get()
        gender = entrygender.get()
        address = entryaddress.get()
        payment_mode = entrypayment_mode.get()

        print(cust_id, name, ph_no, email, gender, address, payment_mode)
        sql = "insert into customer values('{}', '{}', '{}', '{}', '{}', '{}', '{}')".format(cust_id, name, ph_no,
                                                                                             email, gender, address,
                                                                                             payment_mode)

        mariadb_connection = mariadb.connect(user="root", password="", host="localhost", database="project")

        cursor = mariadb_connection.cursor()
        cursor.execute(sql)

        mariadb_connection.commit()

        print(name, " Saved in DataBase")


    Button(window, text="Save", command=Save).pack(side=BOTTOM)
    window.mainloop()

def update():

    def update_name():
        window1 = Tk()
        window1.geometry("200x200")
        window1.configure(bg='powderblue')
        lblTitle = Label(window1, text="Update Name",font = "Arial,12",bg = "Yellow")
        lblTitle.pack()

        lblname = Label(window1, text="Enter Customer New Name")
        lblname.pack()
        entryname = Entry(window1)
        entryname.pack()

        lblemail = Label(window1, text="Enter Customer Email")
        lblemail.pack()
        entryemail = Entry(window1)
        entryemail.pack()
        def update1():
         name = entryname.get()
         email = entrynmail.get()

         print(name, email)
         sql = "update customer set name='{}' where email='{}'".format(name, email)

         mariadb_connection = mariadb.connect(user="root", password="", host="localhost", database="project")

         cursor = mariadb_connection.cursor()
         cursor.execute(sql)

         mariadb_connection.commit()

         print(name, " Update in DataBase")

        Button(window1, text="Update Name", command=update1).pack(side=BOTTOM)
        window1.mainloop()

    def update_phone():
        window2 = Tk()
        window2.geometry("200x200")
        window2.configure(bg='powderblue')
        lblTitle = Label(window2, text="Update Phone",font = "Arial,12",bg = "Yellow")
        lblTitle.pack()

        lblph_no = Label(window2, text="Enter Customer New Phone")
        lblph_no.pack()
        entryph_no = Entry(window2)
        entryph_no.pack()

        lblname = Label(window2, text="Enter Customer Name")
        lblname.pack()
        entryname = Entry(window2)
        entryname.pack()
        def update2():
         ph_no = entryph_no.get()
         name = entryname.get()

         print(ph_no, name)
         sql = "update customer set ph_no='{}' where name='{}'".format(ph_no, name)

         mariadb_connection = mariadb.connect(user="root", password="", host="localhost", database="project")

         cursor = mariadb_connection.cursor()
         cursor.execute(sql)

         mariadb_connection.commit()

         print(ph_no, " Update in DataBase")

        Button(window2, text="Update Phone", command=update2).pack(side=BOTTOM)
        window2.mainloop()

    def update_email():
        window3 = Tk()
        window3.geometry("200x200")
        window3.configure(bg='powderblue')
        lblTitle = Label(window3, text="Update Email",font = "Arial,12",bg = "Yellow")
        lblTitle.pack()

        lblemail = Label(window3, text="Enter Customer New Email")
        lblemail.pack()
        entryemail = Entry(window3)
        entryemail.pack()

        lblname = Label(window3, text="Enter Customer Name")
        lblname.pack()
        entryname = Entry(window3)
        entryname.pack()

        def update3():
            email = entryemail.get()
            name = entryname.get()

            print(email, name)
            sql = "update customer set email='{}' where name='{}'".format(email, name)

            mariadb_connection = mariadb.connect(user="root", password="", host="localhost", database="project")

            cursor = mariadb_connection.cursor()
            cursor.execute(sql)

            mariadb_connection.commit()

            print(ph_no, " Update in DataBase")

        Button(window3, text="Update Email", command=update3).pack(side=BOTTOM)
        window3.mainloop()

    def update_address():
        window4 = Tk()
        window4.geometry("200x200")
        window4.configure(bg='powderblue')
        lblTitle = Label(window4, text="Update Address",font = "Arial,12",bg = "Yellow")
        lblTitle.pack()

        lbladdress = Label(window4, text="Enter Customer New Address")
        lbladdress.pack()
        entryaddress = Entry(window4)
        entryaddress.pack()

        lblname = Label(window4, text="Enter Customer Name")
        lblname.pack()
        entryname = Entry(window4)
        entryname.pack()

        def update4():
            address = entryaddress.get()
            name = entryname.get()

            print(address, name)
            sql = "update customer set address='{}' where name='{}'".format(address, name)

            mariadb_connection = mariadb.connect(user="root", password="", host="localhost", database="project")

            cursor = mariadb_connection.cursor()
            cursor.execute(sql)

            mariadb_connection.commit()

            print(ph_no, " Update in DataBase")

        Button(window4, text="Update Address", command=update4).pack(side=BOTTOM)
        window4.mainloop()

    def update_paymentmode():
        window5 = Tk()
        window5.geometry("200x200")
        window5.configure(bg='powderblue')
        lblTitle = Label(window5, text="Update Payment Mode",font = "Arial,12",bg = "Yellow")
        lblTitle.pack()

        lblpayment_mode = Label(window5, text="Enter Updated Payment Mode")
        lblpayment_mode.pack()
        entrypayment_mode = Entry(window5)
        entrypayment_mode.pack()

        lblname = Label(window5, text="Enter Customer Name")
        lblname.pack()
        entryname = Entry(window5)
        entryname.pack()

        def update5():
            payment_mode = entrypayment_mode.get()
            name = entryname.get()

            print(payment_mode, name)
            sql = "update customer set payment_mode='{}' where name='{}'".format(payment_mode, name)

            mariadb_connection = mariadb.connect(user="root", password="", host="localhost", database="project")

            cursor = mariadb_connection.cursor()
            cursor.execute(sql)

            mariadb_connection.commit()

            print(payment_mode, " Update in DataBase")

        Button(window5, text="Update Payment Mode", command=update5).pack(side=BOTTOM)
        window5.mainloop()

    def update__():
      win1 = Tk()
      frame1 = Frame(win1)
      frame1.pack()
      b1 = Button(frame1, text="Update Customer Name", command=update_name)
      b2 = Button(frame1, text="Update Customer Phone", command=update_phone)
      b3 = Button(frame1, text="Update Customer Email", command=update_email)
      b4 = Button(frame1, text="Update Customer Address", command=update_address)
      b5 = Button(frame1, text="Update Payment Mode", command=update_paymentmode)

      b1.pack()
      b2.pack()
      b3.pack()
      b4.pack()
      b5.pack()

    win1 = update__()
    win1.mainloop()

def delete():
    root = Tk()

    lblTitle = Label(root, text="Enter Name",font = "Arial,16",bg = "Yellow")
    lblTitle.pack()

    lblname = Label(root, text="Enter Customer Name")
    lblname.pack()

    entryname = Entry(root)
    entryname.pack()

    def DeleteIt():
      name = entryname.get()
      print(name)

      sql = "delete from customer where name = '{}'".format(name)
      mariadb_connection = mariadb.connect(user="root", password="", host="localhost", database="project")

      cursor = mariadb_connection.cursor()
      cursor.execute(sql)
      mariadb_connection.commit()
      print(name, " Delete from DataBase")

    Button(root, text="Delete", command=DeleteIt).pack(side=BOTTOM)
    root.mainloop()

def view():

    root2 = Tk()

    lblname = Label(root2, text="Enter Customer Name")
    lblname.pack()

    entryname = Entry(root2)
    entryname.pack()

    def ShowIt():
        name = entryname.get()
        print(name)

        sql = "select * from customer where name = '{}'".format(name)
        mariadb_connection = mariadb.connect(user="root", password="", host="localhost", database="project")

        cursor = mariadb_connection.cursor()
        cursor.execute(sql)
        rows = cursor.fetchall()
        for row in rows:
            print(row[0], row[1], row[2], row[3], row[4], row[5], row[6])

    Button(root2, text="Show", command=ShowIt).pack(side=BOTTOM)
    root2.mainloop()

def view_all():
    sql = "select * from customer"

    mariadb_connection = mariadb.connect(user="root", password="", host="localhost", database="project")

    cursor = mariadb_connection.cursor()
    cursor.execute(sql)
    rows = cursor.fetchall()
    for row in rows:
        print(row[0], row[1], row[2], row[3], row[4], row[5], row[6])


win = Tk()
win.geometry("300x170")
win.configure(bg='bisque')
lblTitle = Label(win, text="SELECT YOUR CHOICE!!!!",font = "Arial,16",bg = "Yellow")
lblTitle.pack()
b1 = Button(win, text=" Add Customer", command=add,fg = "red")
b2 = Button(win, text="Update Customer", command=update,fg = "purple")
b3 = Button(win, text="Delete Customer", command=delete,fg = "orange")
b4 = Button(win, text="View Customer", command=view,fg = "green")
b5 = Button(win, text="View All Customers", command=view_all,fg = "blue")
b1.pack()
b2.pack()
b3.pack()
b4.pack()
b5.pack()

win.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
