---
title: mysql example customer management (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'customer management'

Functions in program: 
* `def selection():`
* `def manager_page():`
* `def see_salesmen():`
* `def simple_analytics():`
* `def duplicates(lst, item):`
* `def plots():`
* `def convert_leads_to_orders():`
* `def fire_salesman():`
* `def update_salesman():`
* `def view_sales_manager():`
* `def add_salesman():`
* `def view_all_leads():`
* `def view_leads():`
* `def delete_leads():`
* `def add_leads():`
* `def selection_leads():`
* `def login_front():`
* `def welcome_page():`
* `def login():`
* `def customer_all():`
* `def view_sales():`
* `def my_cust():`
* `def view_all():`
* `def view():`
* `def delete():`
* `def update():`
* `def add():`
* `def delete_check():`
* `def import_salesids():`
* `def import_userdict():`
* `def createsalesidfile():`
* `def createuserfile():`
* `def phone_nocheck(phone_no):`
* `def date_validation(day, month, year):`
* `def get_key(val):`

Modules used in program: 
* `import datetime`
* `import time`
* `import random`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import pickle as p`
* `import mysql.connector as mysql`

## python customer management

Python mysql example: customer management

```python
from tkinter import *
from tkinter import messagebox
from tkinter.ttk import Combobox as Combo
import mysql.connector as mysql
import pickle as p
import matplotlib.pyplot as plt
import numpy as np
import random
import time
import datetime

# from IPython import get_ipython

mysql_password = '1'
hostname = 'localhost'
cust_ids_customer = []
cust_ids_leads = []
total_sales_list_final = []
id_list_final = []


# use as per requirement
# get_ipython().run_line_magic('matplotlib', 'auto')
def get_key(val):
    print(sales_ids_dict)
    for key, value in sales_ids_dict.items():
        if val == value:
            return key

    return "key doesn't exist"


def date_validation(day, month, year):
    isValidDate = True
    try:
        datetime.datetime(int(year),
                          int(month), int(day))
    except ValueError:
        isValidDate = False
    if (isValidDate):
        return True
    else:
        return False


def phone_nocheck(phone_no):
    phno = str(phone_no)
    if len(phno) != 10:
        print('incorrect no of digits')
        return False

    for i in phno:
        if i not in '1234567890':
            print('please enter only numbers')
            return False
    if phno.isdigit():
        return True


def createuserfile():
    userdict = {'admin': '1234', 'avig': '1234', 'rohan': '1234', 'rishabh': '1234'}
    f = open('users', 'wb+')
    p.dump(userdict, f)
    f.flush()
    f.close()


def createsalesidfile():
    sales_ids = {'admin': 'Null', 'avig': '2', 'rohan': '1', 'rishabh': '3'}
    f = open('salesids1', 'wb+')
    p.dump(sales_ids, f)
    f.flush()
    f.close()


def import_userdict():
    global userdict
    f = open('users', 'rb+')
    userdict = p.load(f)
    f.close()


def import_salesids():
    global sales_ids_dict
    f = open('salesids1', 'rb+')
    sales_ids_dict = p.load(f)
    f.close()


sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                               port=3306, auth_plugin='mysql_native_password')

cursor_main = sql_connection.cursor()

cursor_main.execute('create database if not exists project')

cursor_main.execute('use project;')

cursor_main.execute(
    'create table if not exists customer (sales_id varchar(10),cust_id varchar(10) unique,name varchar(100) '
    ',phone_no varchar(10),email varchar(100),gender varchar(6),address varchar(500),mode varchar('
    '10),value varchar(10));')

cursor_main.execute(
    'create table if not exists leads (cust_id varchar(30) unique,name varchar(50),cust_type varchar(50),'
    'phone_no varchar(10) , '
    'next_meeting varchar(30));')

cursor_main.execute('select cust_id from customer;')
rows = cursor_main.fetchall()
for i in rows:
    cust_ids_customer.append(i[0])

cursor_main.execute('select cust_id from leads;')
rows = cursor_main.fetchall()
for i in rows:
    cust_ids_leads.append(i[0])


def delete_check():
    return messagebox.askyesno('DELETE', 'DO YOU REALLY WANT TO DELETE')


def add():
    global rando_id, cust_ids_customer
    window = Toplevel()
    window.geometry("330x300")
    window.configure(bg='pink')

    lblTitle = Label(window, text="ADD CUSTOMER", font="Arial,12", bg="Yellow")
    lblTitle.pack()

    lblsales_id = Label(window, text='Unique salesman id')
    lblsales_id.place(x=10, y=20)

    lblsales_id = Label(window, text=sales_id_user)
    lblsales_id.place(x=160, y=20)

    lblcust_id = Label(window, text="Customer Id")
    lblcust_id.place(x=10, y=50)

    rando_id = str(random.randint(0, 99999))

    lblcust_id = Label(window, text=rando_id)
    lblcust_id.place(x=160, y=50)

    lblname = Label(window, text="Enter Customer Name")
    lblname.place(x=10, y=80)

    entryname = Entry(window)
    entryname.place(x=160, y=80)

    lblph_no = Label(window, text="Enter Customer Phone")
    lblph_no.place(x=10, y=110)

    entryph_no = Entry(window)
    entryph_no.place(x=160, y=110)

    lblemail = Label(window, text="Enter Customer Email")
    lblemail.place(x=10, y=140)

    entryemail = Entry(window)
    entryemail.place(x=160, y=140)

    lblgender = Label(window, text="Select Customer Gender")
    lblgender.place(x=10, y=170)

    combogender = Combo(window, value=['Male', 'Female'], width=10, height=10)
    combogender.set('Genders')
    combogender.place(x=160, y=170)

    lbladdress = Label(window, text="Enter Customer Address")
    lbladdress.place(x=10, y=200)

    entryaddress = Entry(window)
    entryaddress.place(x=160, y=200)

    lblpayment_mode = Label(window, text="Select Payment mode")
    lblpayment_mode.place(x=10, y=230)

    entrypayment_mode = Combo(window, value=['Cash', 'Card', 'Others'], width=10, height=10)
    entrypayment_mode.place(x=160, y=230)

    lblpayment = Label(window, text='Enter value')
    lblpayment.place(x=10, y=260)

    entrypayment = Entry(window)
    entrypayment.place(x=160, y=260)

    def Save():  # for orders
        sales_id = sales_id_user
        cust_id = rando_id
        name = entryname.get()
        ph_no = entryph_no.get()
        email = entryemail.get()
        gender = combogender.get()
        address = entryaddress.get()
        mode = entrypayment_mode.get()
        value = entrypayment.get()
        check = 0

        for i in value:
            if i in '1234567890':
                check = True
            else:
                check = False
                break

        if check == True:

            if int(value) > 0 and entrypayment_mode != '' and gender != 'Genders':

                print(sales_id, cust_id, name, ph_no, email, gender, address, mode, value)

                sql = "insert into customer values('{}', '{}', '{}', '{}', '{}', '{}', '{}', '{}', '{}')".format(
                    sales_id,
                    cust_id,
                    name,
                    ph_no,
                    email,
                    gender,
                    address,
                    mode,
                    value)

                sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                               database='project', port=3306, auth_plugin='mysql_native_password')

                cursor = sql_connection.cursor()

                cursor.execute(sql)

                sql_connection.commit()

                print(name, " Saved in DataBase")

                cust_ids_customer.append(rando_id)

            else:

                print("Sales cannot be negative. We don't give out loans")

        else:

            print('Sales can only be integers')

    Button(window, text="Save", command=Save).pack(side=BOTTOM)

    window.mainloop()


def update():
    win1 = Toplevel()
    win1.geometry("330x300")
    win1.configure(bg='bisque')

    def update_name():
        window1 = Toplevel()
        window1.geometry("200x200")
        window1.configure(bg='powderblue')

        lblTitle = Label(window1, text="Update Name", font="Arial,12", bg="Yellow")
        lblTitle.pack()

        lblname = Label(window1, text="Enter Customer New Name")
        lblname.pack()

        entryname = Entry(window1)
        entryname.pack()

        lblid = Label(window1, text="Enter Customer ID")
        lblid.pack()

        entryid = Entry(window1)
        entryid.pack()

        def update1():
            name = entryname.get()
            email = entryid.get()

            sql = "update customer set name='{}' where cust_id='{}'".format(name, email)

            sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                           database='project', port=3306, auth_plugin='mysql_native_password')

            cursor = sql_connection.cursor()

            cursor.execute(sql)

            sql_connection.commit()

            print(name, " Updated in DataBase")

        Button(window1, text="Update Name", command=update1).pack(side=BOTTOM)

    def update_phone():
        window2 = Toplevel()
        window2.geometry("200x200")
        window2.configure(bg='powderblue')

        lblTitle = Label(window2, text="Update Phone", font="Arial,12", bg="Yellow")
        lblTitle.pack()

        lblph_no = Label(window2, text="Enter Customer New Phone")
        lblph_no.pack()

        entryph_no = Entry(window2)
        entryph_no.pack()

        lblid = Label(window2, text="Enter Customer ID")
        lblid.pack()

        entryid = Entry(window2)
        entryid.pack()

        def update2():
            ph_no = entryph_no.get()
            name = entryid.get()

            sql = "update customer set ph_no='{}' where cust_id='{}'".format(ph_no, name)

            sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                           database='project', port=3306, auth_plugin='mysql_native_password')

            cursor = sql_connection.cursor()

            cursor.execute(sql)

            sql_connection.commit()

            print(ph_no, " Updated in DataBase")

        Button(window2, text="Update Phone", command=update2).pack(side=BOTTOM)

    def update_email():
        window3 = Toplevel()
        window3.geometry("200x200")
        window3.configure(bg='powderblue')

        lblTitle = Label(window3, text="Update Email", font="Arial,12", bg="Yellow")
        lblTitle.pack()

        lblemail = Label(window3, text="Enter Customer New Email")
        lblemail.pack()

        entryemail = Entry(window3)
        entryemail.pack()

        id = Label(window3, text="Enter Customer ID")
        id.pack()

        entryid = Entry(window3)
        entryid.pack()

        def update3():
            email = entryemail.get()
            name = entryid.get()

            sql = "update customer set email='{}' where cust_id='{}'".format(email, name)

            sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                           database='project', port=3306, auth_plugin='mysql_native_password')

            cursor = sql_connection.cursor()

            cursor.execute(sql)

            sql_connection.commit()

            print(email, " Updated in DataBase")

        Button(window3, text="Update Email", command=update3).pack(side=BOTTOM)

    def update_address():
        window4 = Toplevel()
        window4.geometry("200x200")
        window4.configure(bg='powderblue')

        lblTitle = Label(window4, text="Update Address", font="Arial,12", bg="Yellow")
        lblTitle.pack()

        lbladdress = Label(window4, text="Enter Customer New Address")
        lbladdress.pack()

        entryaddress = Entry(window4)
        entryaddress.pack()

        lblid = Label(window4, text="Enter Customer ID")
        lblid.pack()

        entryid = Entry(window4)
        entryid.pack()

        def update4():
            address = entryaddress.get()
            name = entryid.get()

            sql = "update customer set address='{}' where cust_id='{}'".format(address, name)

            sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                           database='project', port=3306, auth_plugin='mysql_native_password')

            cursor = sql_connection.cursor()

            cursor.execute(sql)

            sql_connection.commit()

            print(address, " Updated in DataBase")

        Button(window4, text="Update Address", command=update4).pack(side=BOTTOM)

    b1 = Button(win1, text="Update Customer Name", command=update_name)
    b2 = Button(win1, text="Update Customer Phone", command=update_phone)
    b3 = Button(win1, text="Update Customer Email", command=update_email)
    b4 = Button(win1, text="Update Customer Address", command=update_address)

    b1.pack()
    b2.pack()
    b3.pack()
    b4.pack()


def delete():
    root = Toplevel()

    lblname = Label(root, text="Enter Customer  ID")
    lblname.pack()

    entryname = Entry(root)
    entryname.pack()

    def DeleteIt():
        name = entryname.get()

        sql = "delete from customer where cust_id = '{}'".format(name)

        sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                       database='project', port=3306, auth_plugin='mysql_native_password')

        cursor = sql_connection.cursor()

        if delete_check():

            cursor.execute(sql)

            sql_connection.commit()

            print(name, " Deleted from DataBase")

            cust_ids_customer.remove(name)

        else:

            print(name, 'Record not deleted')

    Button(root, text="Delete", command=DeleteIt).pack(side=BOTTOM)


def view():
    root2 = Toplevel()

    lblname = Label(root2, text="Select Customer ID")
    lblname.pack()

    entryname = Combo(root2, value=cust_ids_customer, width=10, height=10)
    entryname.set('Choose Id')
    entryname.pack()

    def ShowIt():
        name = entryname.get()

        sql = "select * from customer where cust_id = '{}'".format(name)

        sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                       database='project', port=3306, auth_plugin='mysql_native_password')

        cursor = sql_connection.cursor()

        cursor.execute(sql)

        rows1 = cursor.fetchall()
        for row in rows1:
            print("Sales ID:", row[0], "Customer ID:", row[1], "Customer Name:", row[2], "Phone Number:", row[3]
                  , "Email:", row[4],
                  "Gender:", row[5], "Address:", row[6], "Payment Mode:", row[7], "Sale:", row[8])

    Button(root2, text="Show", command=ShowIt).pack(side=BOTTOM)


def view_all():
    sql = "select * from customer"

    sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                   database='project', port=3306, auth_plugin='mysql_native_password')

    cursor = sql_connection.cursor()

    cursor.execute(sql)

    rows2 = cursor.fetchall()
    for row in rows2:
        print("Sales ID:", row[0], "Customer ID:", row[1], "Customer Name:", row[2], "Phone Number:", row[3]
              , "Email:", row[4],
              "Gender:", row[5], "Address:", row[6], "Payment Mode:", row[7], "Sale:", row[8])


def my_cust():
    sql = "select * from customer where sales_id='{}'".format(sales_id_user)

    sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                   database='project', port=3306, auth_plugin='mysql_native_password')

    cursor = sql_connection.cursor()

    cursor.execute(sql)

    rows3 = cursor.fetchall()

    if rows3 == []:

        print('Sorry! You have no customers yet')

    else:

        for row in rows3:
            print("Sales ID:", row[0], "Customer ID:", row[1], "Customer Name:", row[2], "Phone Number:", row[3]
                  , "Email:", row[4],
                  "Gender:", row[5], "Address:", row[6], "Payment Mode:", row[7], "Sale:", row[8])


def view_sales():
    root2 = Toplevel()

    lblsales_id = Label(root2, text="Your Sales ID")
    lblsales_id.pack()

    entrysales_id = Label(root2, text=sales_id_user)
    entrysales_id.pack()

    def ShowIt():
        print(sales_id_user, ':checking sales id')

        sql = "select value from customer where sales_id = '{}'".format(sales_id_user)

        sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                       database='project', port=3306, auth_plugin='mysql_native_password')

        cursor = sql_connection.cursor()

        cursor.execute(sql)

        rows4 = cursor.fetchall()
        sales_list = list(map(int, [x[0] for x in rows4]))
        print(sales_list)

        total = sum(sales_list)
        print('Gross Sales:', total)

    Button(root2, text="Show", command=ShowIt).pack(side=BOTTOM)


def customer_all():
    win = Toplevel()
    win.geometry("400x400")
    win.configure(bg='bisque')

    lblTitle = Label(win, text="SELECT YOUR CHOICE!!!!", font="Arial,16", bg="Yellow")
    lblTitle.pack()

    b1 = Button(win, text=" Add Customer", command=add, fg="red")
    b2 = Button(win, text="Update Customer", command=update, fg="purple")
    b3 = Button(win, text="Delete Customer", command=delete, fg="orange")
    b4 = Button(win, text="View Customer", command=view, fg="green")
    b5 = Button(win, text="View All Customers", command=view_all, fg="blue")
    b6 = Button(win, text="View sales", command=view_sales, fg='IndianRed4')
    b7 = Button(win, text='View Your Customer', command=my_cust, fg='Powder Blue')

    b1.pack()
    b2.pack()
    b3.pack()
    b4.pack()
    b7.pack()
    b5.pack()
    b6.pack()


def login():
    global loginwindow, usernameentry, passwordentry, sales_id_user

    user = usernameentry.get().replace(' ', '')
    password = passwordentry.get().replace(' ', '')
    userlist = list(userdict.keys())

    if user == userlist[0]:

        if userdict[user] == password:

            messagebox.showinfo("SUCCESS", "WELCOME BACK BOSS")

            loginwindow.destroy()

            manager_page()

        else:

            messagebox.showinfo("login failed", "incorrect password")

    elif user in userlist:

        if userdict[user] == password:

            messagebox.showinfo("SUCCESS", "WELCOME BACK " + user)

            sales_id_user = sales_ids_dict[user]

            loginwindow.destroy()

            selection()
        else:

            messagebox.showinfo("login failed", "incorrect password")
    else:

        messagebox.showinfo("login failed", "incorrect username")


def welcome_page():
    global welcome

    text1 = '''Welcome to customer relations management

    please enter your designated username and password

    if you don\' t have the details then please contact the administration

    for a salesman: enter potential customer details as leads

    add new customers as orders

    for a manager: add,update and fire salesmen also analyse data '''

    welcome = Tk(className='Welcome To CRM')

    lbl = Label(welcome, text=text1)
    lbl.pack()

    okbutton = Button(welcome, text='Close', command=login_front)
    okbutton.pack()

    welcome.mainloop()


def login_front():
    welcome.destroy()

    global loginwindow, usernameentry, passwordentry

    loginwindow = Tk(className='LOGIN')

    loginwindow.geometry("300x170")

    loginwindow.configure(bg='bisque')

    lbluser = Label(loginwindow, text="ENTER USERNAME", font="Arial,12", bg="Yellow")

    lbluser.pack()

    usernameentry = Entry(loginwindow)

    usernameentry.pack()

    Label(loginwindow, text="ENTER PASSWORD", font="Arial,12", bg="Yellow").pack()

    passwordentry = Entry(loginwindow)
    passwordentry.config(show='*')
    passwordentry.pack()

    loginbutton = Button(loginwindow, text="LOGIN", command=login, fg="blue")

    loginbutton.pack()


def selection_leads():
    win = Toplevel()
    win.geometry("300x170")
    win.configure(bg='bisque')

    lblTitle = Label(win, text="SELECT YOUR CHOICE!!!!", font="Arial,16", bg="Yellow")
    lblTitle.pack()

    b1 = Button(win, text="Add Leads", command=add_leads, fg="red")
    b3 = Button(win, text="Delete Leads", command=delete_leads, fg="orange")
    b4 = Button(win, text="View Lead", command=view_leads, fg="green")
    b5 = Button(win, text="View All Leads", command=view_all_leads, fg="blue")
    b2 = Button(win, text="convert to order", command=convert_leads_to_orders, fg="blue")

    b1.pack()
    b3.pack()
    b4.pack()
    b5.pack()
    b2.pack()


def add_leads():
    global cust_ids_leads, cust_id_lead

    window = Toplevel()
    window.geometry('300x330')
    window.configure(bg='pink')

    lb_title = Label(window, text='ADD LEADS', font="Arial,10", bg="Yellow")
    lb_title.pack()

    cust_id_lead = str(random.randint(0, 99999))

    lblcustidname = Label(window, text='Customer ID')
    lblcustidname.place(x=10, y=20)

    lblcustid = Label(window, text=cust_id_lead)
    lblcustid.place(x=160, y=20)

    lbname = Label(window, text='Name')
    lbname.place(x=10, y=50)

    entry_lbname = Entry(window)
    entry_lbname.place(x=160, y=50)

    lblcategory = Label(window, text="Customer Type")
    lblcategory.place(x=10, y=80)

    combo_custtype = Combo(window, values=['New', 'Repeating'], width=10, height=10)
    combo_custtype.set('Select')
    combo_custtype.place(x=160, y=80)

    lblph_no = Label(window, text="Phone number")
    lblph_no.place(x=10, y=110)

    entry_lblph_no = Entry(window)
    entry_lblph_no.place(x=160, y=110)

    lbl_meet = Label(window, text="Next meeting ('ddmmyyyy')")  # put date picker here
    lbl_meet.place(x=10, y=130)

    entry_lbl_meet = Entry(window)
    entry_lbl_meet.place(x=160, y=130)

    def Save():  # for orders
        cust_id = cust_id_lead
        name = entry_lbname.get()
        category = combo_custtype.get()
        phone = entry_lblph_no.get()
        meet = entry_lbl_meet.get()

        if date_validation(int(meet[0:2]), int(meet[2:4]), int(meet[4:])) and category != 'Select':

            if time.strptime(meet, "%d%m%Y") > time.strptime(time.strftime("%d%m%Y"), "%d%m%Y"):

                sql = "insert into leads values('{}','{}', '{}', '{}', '{}')".format(cust_id, name, category, phone,
                                                                                     meet)

                sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                               database='project', port=3306, auth_plugin='mysql_native_password')

                cursor = sql_connection.cursor()

                cursor.execute(sql)

                sql_connection.commit()

                print(name, " Saved in DataBase")

                cust_ids_leads.append(cust_id)

            else:

                print('Please enter a future date')

        else:

            print('Please enter Valid date')

    Button(window, text="Save", command=Save).pack(side=BOTTOM)


def delete_leads():
    root = Toplevel()

    lblTitle = Label(root, text="Enter Cust_ID", font="Arial,16", bg="Yellow")
    lblTitle.pack()

    entryname = Entry(root)
    entryname.pack()

    def DeleteIt_leads():
        name = entryname.get()

        sql = "delete from leads where cust_id = '{}'".format(name)

        sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                       database='project', port=3306, auth_plugin='mysql_native_password')
        cursor = sql_connection.cursor()

        if delete_check():

            cursor.execute(sql)

            sql_connection.commit()

            print(name, 'Deleted from database')

            cust_ids_leads.remove(name)

        else:

            print(name, 'Record not deleted')

    Button(root, text="Delete", command=DeleteIt_leads).pack(side=BOTTOM)


def view_leads():
    root2 = Toplevel()

    combo = Combo(root2, values=cust_ids_leads, width=15, height=20)
    combo.set('Select customer ID')
    combo.pack()

    def ShowIt():
        name = combo.get()

        sql = "select * from leads where cust_id = '{}'".format(name)

        sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                       database='project', port=3306, auth_plugin='mysql_native_password')

        cursor = sql_connection.cursor()
        cursor.execute(sql)

        rows5 = cursor.fetchall()
        for row in rows5:
            print("Customer ID:", row[0], "Customer Name:", row[1], "Customer Type:", row[2], "Phone number:", row[3]
                  , "Next Meeting:", row[4])

    Button(root2, text="Show", command=ShowIt).pack(side=BOTTOM)


def view_all_leads():
    sql = "select * from leads"

    sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                   database='project', port=3306, auth_plugin='mysql_native_password')

    cursor = sql_connection.cursor()
    cursor.execute(sql)

    rows6 = cursor.fetchall()
    for row in rows6:
        print("Customer ID:", row[0], "Customer Name:", row[1], "Customer Type:", row[2], "Phone number:", row[3]
              , "Next Meeting:", row[4])


def add_salesman():
    win = Toplevel()
    win.geometry("400x400")
    win.configure(bg='bisque')

    Label(win, text="Enter Employee Name", bg='Yellow', fg='red').pack()
    entrykey = Entry(win)
    entrykey.pack()

    Label(win, text='Enter password', bg='yellow', fg='red').pack()
    entrypassword = Entry(win)
    entrypassword.pack()

    def process():
        key = entrykey.get()
        password = entrypassword.get()

        f2 = open('salesids1', 'rb+')
        f = open('users', 'rb+')
        sales_ids_dict = p.load(f2)
        userdict = p.load(f)
        sales_id_count = list(sales_ids_dict.values())[-1]
        f3 = open('sales_id_latest', 'w+')

        if key in userdict.keys():

            print('Employee already exists')

        else:
            userdict[key] = password
            f.seek(0)
            p.dump(userdict, f)
            f.flush()
            f.seek(0)
            userdict = p.load(f)

            sales_id_count = str(int(sales_id_count) + 1)
            sales_ids_dict[key] = sales_id_count
            f2.seek(0)
            p.dump(sales_ids_dict, f2)
            f2.flush()
            f2.seek(0)
            f3.write(sales_id_count)

            print('EMPLOYEE ADDED:', userdict, sales_ids_dict)

        f3.flush()
        f3.close()

        f2.flush()
        f2.close()

        f.flush()
        f.close()

    Button(win, text='add', command=process).pack()


def view_sales_manager():
    win = Toplevel()
    win.geometry("400x400")
    win.configure(bg='bisque')

    Label(win, text="Enter Employee ID", bg='Yellow', fg='red').pack()
    entryid = Entry(win)
    entryid.pack()

    def ShowIt():
        print(entryid.get(), ':checking sales id')

        sql = "select value from customer where sales_id = '{}'".format(entryid.get())

        sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                       database='project', port=3306, auth_plugin='mysql_native_password')

        cursor = sql_connection.cursor()
        cursor.execute(sql)

        rows7 = cursor.fetchall()
        total = sum(list(map(int, [x[0] for x in rows7])))
        print('Gross Sales:', total)

    Button(win, text="Show", command=ShowIt).pack(side=BOTTOM)


def update_salesman():
    update = Toplevel()
    update.geometry("400x400")
    update.configure(bg='bisque')

    Label(update, text='UPDATE MENU', bg='Yellow', fg='red').pack()

    lblold = Label(update, text='enter old username which has to be changed')
    lblold.pack()

    entryold = Entry(update)
    entryold.pack()

    f = open('users', 'rb+')
    userdict = p.load(f)
    f.close()

    f2 = open('salesids1', 'rb+')
    sales_ids_dict = p.load(f2)
    f2.close()

    def update_user():
        global process

        update1 = Toplevel()
        update1.geometry("400x400")
        update1.configure(bg='bisque')

        old = entryold.get()

        if old in list(userdict.keys()):
            lblold = Label(update1, text='enter new username which has to be added')
            lblold.pack()

            entrynew = Entry(update1)
            entrynew.pack()

            def process():
                key_list = []
                value_list = []

                if entrynew.get() in list(userdict.keys()):

                    print('This user already exists')

                else:

                    if entryold.get() == list(userdict.keys())[0]:

                        print('changing admin')

                        id_sales = sales_ids_dict[old]
                        password = userdict[old]

                        del userdict[old]
                        del sales_ids_dict[old]

                        userdict[entrynew.get()] = password
                        for key, value in userdict.items():
                            key_list.append(key)
                            value_list.append(value)
                        userdict.clear()
                        dict3 = dict(zip(key_list[::-1], value_list[::-1]))

                        sales_ids_dict[entrynew.get()] = id_sales
                        key_list.clear()
                        value_list.clear()
                        for key, value in sales_ids_dict.items():
                            key_list.append(key)
                            value_list.append(value)
                        sales_ids_dict.clear()
                        dict4 = dict(zip(key_list[::-1], value_list[::-1]))

                        f = open('users', 'wb+')
                        f.truncate()
                        f.seek(0)
                        p.dump(dict3, f)
                        f.close()

                        f2 = open('salesids1', 'wb+')
                        f2.truncate()
                        f2.seek(0)
                        p.dump(dict4, f2)
                        f2.close()

                        print(dict4)
                        print(dict3)

                    else:

                        id_sales = sales_ids_dict[old]
                        password = userdict[old]

                        del userdict[old]
                        del sales_ids_dict[old]

                        userdict[entrynew.get()] = password
                        sales_ids_dict[entrynew.get()] = id_sales

                        f = open('users', 'wb+')
                        f.truncate()
                        f.seek(0)
                        p.dump(userdict, f)
                        f.close()

                        f2 = open('salesids1', 'wb+')
                        f2.truncate()
                        f2.seek(0)
                        p.dump(sales_ids_dict, f2)
                        f2.close()

                        print(userdict)
                        print(sales_ids_dict)

        else:

            print('username does not exist')

        Button(update1, text='done', command=process).pack()

    def update_password():
        global process2

        update2 = Toplevel()
        update2.geometry("400x400")
        update2.configure(bg='bisque')

        old = entryold.get()

        if old in list(userdict.keys()):
            lblpassnew = Label(update2, text='enter new password which has to be changed')
            lblpassnew.pack()

            entrynew = Entry(update2)
            entrynew.pack()

            def process2():
                f = open('users', 'rb+')
                userdict = p.load(f)
                userdict[old] = entrynew.get()

                f.seek(0)
                f.truncate()
                f.seek(0)
                p.dump(userdict, f)
                f.flush()
                f.close()

                print('success')
                print(userdict)

        else:

            print('username does not exist')

        Button(update2, text='done', command=process2).pack()

    Button(update, text='update user', command=update_user).pack()
    Button(update, text='update password', command=update_password).pack()


def fire_salesman():
    update3 = Toplevel()
    update3.geometry("400x400")
    update3.configure(bg='bisque')

    Label(update3, text='FIRE!!!', bg='Yellow', fg='red').pack()

    lblold = Label(update3, text='Enter Employee who has to be fired')
    lblold.pack()

    entryold = Entry(update3)
    entryold.pack()

    f = open('users', 'rb')
    userdict = p.load(f)
    f.close()

    def process():

        if entryold.get() in list(userdict.keys()):

            if entryold.get() == 'admin':

                print('Sorry! You cannot resign.')

            else:

                del userdict[entryold.get()]

                print('Employee fired successfully!. Nice job!')
                print(userdict)

                f = open('users', 'wb+')
                p.dump(userdict, f)
                f.flush()
                f.close()

                f1 = open('salesids1', 'rb+')
                sales_ids_dict = p.load(f1)
                del sales_ids_dict[entryold.get()]
                f1.seek(0)
                p.dump(sales_ids_dict, f1)
                f1.close()

        else:

            print('Employee doesnt exist')

    Button(update3, text='Fire!!!', command=process, bg='black', fg='red').pack()


def convert_leads_to_orders():
    root2 = Toplevel()

    combo = Combo(root2, values=cust_ids_leads, width=15, height=20)
    combo.set('Select customer ID')
    combo.pack()

    def conversion():
        global phone_no, cust_id_lead
        name = combo.get()
        print(name)

        sql = "select * from leads where cust_id = '{}'".format(name)

        sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,

                                       database='project', port=3306, auth_plugin='mysql_native_password')

        cursor = sql_connection.cursor()

        cursor.execute(sql)

        rows8 = cursor.fetchall()
        for row in rows8:
            cust_id_lead = row[0]
            name = row[1]
            phone_no = row[3]

            print("Customer ID:", row[0], "Customer Name:", row[1], "Customer Type:", row[2], "Phone number:", row[3]
                  , "Next Meeting:", row[4])

        window = Toplevel()
        window.geometry('300x330')
        window.configure(bg='pink')

        lb_title = Label(window, text='Covert Leads To Orders', font="Arial,10", bg="Yellow")
        lb_title.pack()

        lblsales_id = Label(window, text='Unique salesman id')
        lblsales_id.place(x=10, y=20)

        lblsales_id = Label(window, text=sales_id_user)
        lblsales_id.place(x=160, y=20)

        lblcustidname = Label(window, text='Customer ID')
        lblcustidname.place(x=10, y=50)

        lblcustid = Label(window, text=cust_id_lead)
        lblcustid.place(x=160, y=50)

        lbname = Label(window, text='Name')
        lbname.place(x=10, y=80)

        entry_lbname = Label(window, text=name)
        entry_lbname.place(x=160, y=80)

        lblph_no = Label(window, text="Phone number")
        lblph_no.place(x=10, y=110)

        entry_lblph_no = Label(window, text=phone_no)
        entry_lblph_no.place(x=160, y=110)

        lblemail = Label(window, text="Enter Customer Email")
        lblemail.place(x=10, y=140)

        entryemail = Entry(window)
        entryemail.place(x=160, y=140)

        lblgender = Label(window, text="Select Customer Gender")
        lblgender.place(x=10, y=170)

        combogender = Combo(window, value=['Male', 'Female'], width=10, height=10)
        combogender.set('Genders')
        combogender.place(x=160, y=170)

        lbladdress = Label(window, text="Enter Customer Address")
        lbladdress.place(x=10, y=200)

        entryaddress = Entry(window)
        entryaddress.place(x=160, y=200)

        lblpayment_mode = Label(window, text="Select Payment mode")
        lblpayment_mode.place(x=10, y=230)

        entrypayment_mode = Combo(window, value=['Cash', 'Card', 'Others'], width=10, height=10)
        entrypayment_mode.place(x=160, y=230)

        lblpayment = Label(window, text='Enter value')
        lblpayment.place(x=10, y=260)

        entrypayment = Entry(window)
        entrypayment.place(x=160, y=260)

        def Save():  # for orders

            sales_id = sales_id_user

            cust_id = cust_id_lead

            name1 = name

            ph_no = phone_no

            email = entryemail.get()

            gender = combogender.get()

            address = entryaddress.get()

            mode = entrypayment_mode.get()

            value = entrypayment.get()

            check = 0

            for i in value:

                if i in '1234567890':

                    check = True

                else:

                    check = False

                    break

            if check == True:

                if int(value) > 0 and entrypayment_mode != '' and gender != 'Genders':
                    print(sales_id, cust_id, name1, ph_no, email, gender, address, mode, value)

                    sql = "insert into customer values('{}', '{}', '{}', '{}', '{}', '{}', '{}', '{}', '{}')".format(
                        sales_id,
                        cust_id,
                        name1,
                        ph_no,
                        email,
                        gender,
                        address,
                        mode,
                        value)

                    sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                                   database='project', port=3306, auth_plugin='mysql_native_password')

                    cursor = sql_connection.cursor()

                    cursor.execute(sql)

                    sql_connection.commit()

                    sql1 = "delete from leads where cust_id = '{}'".format(cust_id)

                    cursor = sql_connection.cursor()

                    cursor.execute(sql1)

                    sql_connection.commit()

                    print(name, " Saved in DataBase", 'deleted from leads')

                    cust_ids_customer.append(cust_id)

                else:

                    print("Sales cannot be negative. We don't give out loans")

            else:

                print('Sales can only be integers')

        Button(window, text="Save", command=Save).pack(side=BOTTOM)

        window.mainloop()

    Button(root2, text="Convert", command=conversion).pack(side=BOTTOM)


def plots():
    plotswin = Toplevel()
    plotswin.geometry('300x300')
    plotswin.configure(bg='burlywood1')

    total_sales_list = []
    total_sales_list_final = []
    id_list_final = []

    sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                   database='project', port=3306, auth_plugin='mysql_native_password')

    cursor = sql_connection.cursor()

    cursor.execute('select sales_id from customer')

    id_list = [x[0] for x in cursor.fetchall()]
    for i in id_list:
        if i not in id_list_final:
            id_list_final.append(i)
    id_list_final = list(map(int, id_list_final))

    for i in id_list:
        cursor.execute("select value from customer where sales_id='{}'".format(i))
        rows = cursor.fetchall()
        sales_list = [x[0] for x in rows]
        sales_list = list(map(int, sales_list))
        total_sales_list.append(sum(sales_list))

    for i in total_sales_list:
        if i not in total_sales_list_final:
            total_sales_list_final.append(i)

    def total_sales_vs_salesman():

        plt.figure()
        plt.bar(np.arange(len(id_list_final)), total_sales_list_final, align='center', alpha=0.5)
        plt.xticks(np.arange(len(id_list_final)), id_list_final)
        plt.ylabel('Total Sale in Rupees')
        plt.xlabel('Sales ID of salesman')
        plt.title('Total sales of each Salesman')
        plt.show()

    def pie_totalsales_vs_salesman():
        labels = id_list_final
        sizes = total_sales_list_final
        colors = ['red', 'blue', 'green', 'yellow', 'orange', 'white']
        explode = []
        for i in total_sales_list_final:
            if i > 100000:
                explode.append(0.2)
            else:
                explode.append(0)
        plt.pie(sizes, explode=explode, labels=labels, colors=colors,
                autopct='%03.1f%%', shadow=True, startangle=140)
        plt.axis('equal')
        plt.show()

    def payment_method_pie():
        cash = 0
        card = 0
        other = 0
        colors = ['red', 'blue', 'green', 'yellow', 'orange', 'white']
        explode = []
        labels = ['cash', 'card', 'other']
        sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                       database='project', port=3306, auth_plugin='mysql_native_password')
        cursor = sql_connection.cursor()

        cursor.execute('select mode from customer')
        mode_list = [x[0] for x in cursor.fetchall()]
        for i in mode_list:
            if i == 'Cash':
                cash += 1
            elif i == 'Card':
                card += 1
            else:
                other += 1

        sizes = [cash, card, other]
        for i in range(0, len(sizes) - 1):
            if sizes[i] == 0:
                sizes.pop(i)
                labels.pop(i)

        for i in sizes:
            explode.append(0.0)
        plt.pie(sizes, explode=explode, labels=labels, colors=colors,
                autopct='%03.1f%%', shadow=True, startangle=140)
        plt.show()

    Button(plotswin, text='View Salesman vs Total Sales Graph', command=total_sales_vs_salesman).pack()
    Button(plotswin, text='View Salesman vs Total Sales Graph in Pie', command=pie_totalsales_vs_salesman).pack()
    Button(plotswin, text='Pie Payment Mode', command=payment_method_pie).pack()


def duplicates(lst, item):
    """

    :param lst:
    :param item:
    :return:
    """
    return [i for i, x in enumerate(lst) if x == item]


def simple_analytics():
    root = Tk()
    sql_connection = mysql.connect(user="root", password=mysql_password, host=hostname,
                                   database='project', port=3306, auth_plugin='mysql_native_password')
    cursor = sql_connection.cursor()

    cursor.execute('select sales_id,value from customer')

    rows = cursor.fetchall()
    sales_ids_1 = []
    sales = []
    elements = []
    dictx = {}
    for i in rows:
        sales_ids_1.append(i[0])
        sales.append(i[1])
    sales_ids_1 = list(map(int, sales_ids_1))
    sales = list(map(int, sales))
    '''final_dict = dict((x, duplicates(sales_ids_1, x)) for x in set(sales_ids_1) if sales_ids_1.count(x) > 1)
    print(final_dict)'''
    for elem in sales_ids_1:
        counter = 0
        elem_pos = []
        for i in sales_ids_1:
            if i == elem:
                elem_pos.append(counter)
            counter = counter + 1
        elements.append([elem, elem_pos])
    res = []
    for i in elements:
        if i not in res:
            res.append(i)
    for i in res:
        t = i[0]
        value_final = 0
        for j in i[1]:
            value_final += sales[j]
        dictx[t] = value_final
    print(dictx)
    salesman_id = max(dictx, key=dictx.get)
    print(salesman_id)
    string2 = get_key(str(salesman_id))
    message = "The salesman with maximum sales is:" + string2
    print(message)
    Label(root, text=message).pack()


def see_salesmen():
    f = open('users', 'rb+')
    f.seek(0)
    userdict = p.load(f)
    f.flush()
    f.close()
    print(userdict)


def manager_page():
    win = Tk(className='Manager Window')
    win.geometry("400x400")
    win.configure(bg='bisque')

    lblTitle = Label(win, text="SELECT YOUR CHOICE!!!!", font="Arial,16", bg="Yellow")
    lblTitle.pack()

    b1 = Button(win, text=" Add Salesman", command=add_salesman, fg="red")
    b2 = Button(win, text="Update Salesman", command=update_salesman, fg="purple")
    b3 = Button(win, text="Fire Salesman", command=fire_salesman, fg="orange")
    b4 = Button(win, text="View Customer", command=view, fg="green")
    b5 = Button(win, text="View All Customers", command=view_all, fg="blue")
    b6 = Button(win, text="View sales", command=view_sales_manager, fg='IndianRed4')
    b7 = Button(win, text='Map Graphs', command=plots, fg='Powder Blue')
    b8 = Button(win, text='View Salesmen', command=see_salesmen, fg='Cyan')
    b9 = Button(win, text='View Salesman with most sales', command=simple_analytics, fg='cornsilk3')

    b1.pack()
    b2.pack()
    b3.pack()
    b4.pack()
    b5.pack()
    b6.pack()
    b7.pack()
    b8.pack()
    b9.pack()


# both are same as customer only small changes
def selection():
    win = Tk(className='Selection')
    win.geometry("300x170")
    win.configure(bg='bisque')

    lblTitle = Label(win, text="SELECT YOUR CHOICE!!!!", font="Arial,16", bg="Yellow")
    lblTitle.pack()

    b1 = Button(win, text=" Leads", command=selection_leads, fg="red")
    b2 = Button(win, text="Orders", command=customer_all, fg="purple")

    b1.pack()
    b2.pack()


try:
    F = open('check.txt')
    F.close()
except Exception as e:
    createuserfile()
    createsalesidfile()

    F = open('check.txt', 'w')
    F.write('1')
    F.close()

import_salesids()
import_userdict()
welcome_page()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
