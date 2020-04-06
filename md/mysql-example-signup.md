---
title: mysql example signup (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'signup'

Functions in program: 
* `def submit():`
* `def next():`
* `def cancel():`
* `def signup():`

Modules used in program: 
* `import random`
* `import mysql.connector`

## python signup

Python mysql example: signup

```python
from tkinter import *
from tkinter import messagebox
from datetime import date
import mysql.connector
import random



def signup():
    global win, name, fname, dob, emailid, add, city, pincode, state, var1, var2

    win = Tk()
    win.config(bg="orange")
    win.title("Signup")
    win.geometry("500x700")
    win.resizable(width=False, height=False)

    var1 = StringVar()
    var2 = StringVar()
    var1.set("male")
    var2.set("unmarried")

    l1=Label(win,text="APPLICATION FORM",font=("georgia",30),bg="orange").grid(padx=30,pady=10)
    l2=Label(win,text="Page 1: Personal Details",font=("georgia",20),bg="orange").grid(padx=10,pady=10)

    f4=Frame(win,bg="orange")

    l3=Label(f4,text="Name :",font="15",bg="orange").grid(row=3,column=3,padx=10,pady=10)
    name=Entry(f4,font="15")
    name.grid(row=3,column=4,padx=10,pady=10)

    l4=Label(f4,text="Father's Name :",font="15",bg="orange").grid(row=4,column=3,padx=10,pady=10)
    fname=Entry(f4,font="15")
    fname.grid(row=4,column=4,padx=10,pady=10)

    l5=Label(f4,text="Date of Birth :",font="15",bg="orange").grid(row=5,column=3,padx=10,pady=10)
    dob=Entry(f4,font="15")
    dob.grid(row=5,column=4,padx=10,pady=10)


    l6 = Label(f4, text="Gender :",font="15",bg="orange").grid(row=6,column=3,padx=10,pady=10)
    m=Radiobutton(f4,text="Male",bg="orange",variable=var1,value="male").grid(row=6,column=4,padx=0,pady=10)
    f=Radiobutton(f4, text="Female",bg="orange",variable=var1,value="female").grid(row=6,column=5,padx=0,pady=10)

    l7=Label(f4,text="Email id:",font="15",bg="orange").grid(row=7,column=3,padx=10,pady=10)
    emailid = Entry(f4,font="15")
    emailid.grid(row=7,column=4,padx=10,pady=10)

    l8=Label(f4,text="Martial Status :",font="15",bg="orange").grid(row=8,column=3,padx=10,pady=10)
    mary = Radiobutton(f4, text="married", bg="orange",variable=var2,value="married").grid(row=8, column=4, padx=0, pady=10)
    unmary= Radiobutton(f4, text="unmarried", bg="orange",variable=var2,value="unmarried").grid(row=8, column=5, padx=0, pady=10)

    l9=Label(f4,text="Address :",font="15",bg="orange").grid(row=9,column=3,padx=10,pady=10)
    add= Entry(f4,font="15")
    add.grid(row=9,column=4,padx=10,pady=10)

    l10=Label(f4,text="City :",font="15",bg="orange").grid(row=10,column=3,padx=10,pady=10)
    city = Entry(f4,font="15")
    city.grid(row=10,column=4,padx=10,pady=10)

    l11 = Label(f4, text="Pin Code :",font="15",bg="orange").grid(row=11,column=3,padx=10,pady=10)
    pincode = Entry(f4,font="15")
    pincode.grid(row=11,column=4,padx=10,pady=10)

    l12 = Label(f4, text="State :",font="15",bg="orange").grid(row=12,column=3,padx=10,pady=10)
    state = Entry(f4,font="15")
    state.grid(row=12,column=4,padx=10,pady=10)
    f4.grid()

    f5 = Frame(win, bg="orange")
    b=Button(f5,text="Next",bg="blue",command=next).grid(row=1,columnspan=12,ipadx=10,padx=10,pady=10)
    b1 = Button(f5, text="Cancel", bg="blue", command=cancel).grid(row=1, column=12,padx=10,ipadx=10,pady=10)
    f5.grid()


    win.mainloop()



def cancel():
    win.destroy()

def next():
    global win3, religion, category, income, qualify, panno, aadhar, occupation, var3, var4
    win3 = Tk()
    win3.config(bg="orange")
    win3.geometry("500x600")
    win3.title("Page 2")
    win3.resizable(width=False, height=False)
    var3=StringVar()
    var4=StringVar()
    var3.set("not senior ciizen")
    var4.set("unexisting")

    l=Label(win3,text="Page 2: Additional Details",font=("georgia",30),bg="orange").grid(padx=10,pady=10)

    f=Frame(win3,bg="orange")

    l1=Label(f,text="Religion :",bg="orange",font="15").grid(row=3,column=3,padx=10,pady=10)
    religion=Spinbox(f,font="15",values=("HINDU","MUSLIM","SIKH","CHRISTIAN"))
    religion.grid(row=3,column=4,padx=10,pady=10)

    l2= Label(f, text="Category :", bg="orange", font="15").grid(row=4,column=3,padx=10,pady=10)
    category = Spinbox(f, font="15",values=("SC","ST","OBC","GENERAL"))
    category.grid(row=4,column=4,padx=10,pady=10)

    l3= Label(f, text="Income :", bg="orange", font="15").grid(row=5,column=3,padx=10,pady=10)
    income=Spinbox(f,font="15",values=("BELOW 100000","100000","ABOVE 100000"))
    income.grid(row=5,column=4,padx=10,pady=10)

    l4 = Label(f, text="Educational Qualification :", bg="orange", font="15").grid(row=6,column=3,padx=10,pady=10)
    qualify = Spinbox(f, font="15",values=("10th","12th","GRADUATE"))
    qualify.grid(row=6,column=4,padx=10,pady=10)

    l5 = Label(f, text="Occupation :", bg="orange", font="15").grid(row=7,column=3,padx=10,pady=10)
    occupation = Entry(f, font="15")
    occupation.grid(row=7,column=4,padx=10,pady=10)

    l6 = Label(f, text="PAN Number :", bg="orange", font="15").grid(row=8,column=3,padx=10,pady=10)
    panno = Entry(f, font="15")
    panno.grid(row=8,column=4,padx=10,pady=10)

    l7 = Label(f, text="Aadhar Number :", bg="orange", font="15").grid(row=9,column=3,padx=10,pady=10)
    aadhar = Entry(f, font="15")
    aadhar.grid(row=9,column=4,padx=10,pady=10)

    l8 = Label(f, text="Senior Citizen :", bg="orange", font="15").grid(row=10,column=3,padx=10,pady=10)
    c1=Radiobutton(f, text="yes", font="15", bg="orange",variable=var3,value="senior citizen").grid(row=10, column=4, padx=10, pady=10)
    c2 = Radiobutton(f, text="no", font="15", bg="orange",variable=var3,value="not senior ciizen").grid(row=10, column=5, padx=10, pady=10)

    l9 = Label(f, text="Existing Account :", bg="orange", font="15").grid(row=11,column=3,padx=10,pady=10)
    y = Radiobutton(f, text="yes", font="15", bg="orange",variable=var4,value="existing").grid(row=11, column=4, padx=10, pady=10)
    n = Radiobutton(f, text="no", font="15", bg="orange",variable=var4,value="unexisting").grid(row=11, column=5, padx=10, pady=10)

    b=Button(f, text="Submit ", bg="blue", font="15",command=submit).grid(row=12,column=4,padx=10,pady=10)
    f.grid()

    win3.mainloop()


def submit():
    global card_no
    namee = name.get()
    fnamee = fname.get()
    emailidd = emailid.get()
    addd = add.get()
    cityy = city.get()
    pincodee = pincode.get()
    statee = state.get()
    religionn = religion.get()
    categoryy = category.get()
    incomee = income.get()
    qualifyy = qualify.get()
    pannoo = panno.get()
    aadharr = aadhar.get()
    occupationn = occupation.get()
    acno = random.randint(1000000000,9999999999)
    card_no = acno
    pin = random.randint(1000,9999)
    balance = 1000

    if (namee == "" or fnamee == "" or emailidd == "" or addd == "" or cityy == "" or pincodee == "" or statee == "" or religionn == "" or categoryy == "" or incomee == "" or qualifyy == "" or pannoo == "" or aadharr == "" or occupationn == ""):
        messagebox.showinfo("ERROR", "PLEASE FILL ALL THE FIELDS.")
        win.destroy()
        win3.destroy()
    else:
        conn = mysql.connector.connect(user="root", password="ashish", host="localhost", database="atm")
        mycursor = conn.cursor()
        #mycursor.execute("DROP TABLE IF EXISTS ATM")
        sql = """CREATE TABLE ATM(
            Name VARCHAR(20) NOT NULL, 
            Fathers_name VARCHAR(20) NOT NULL,
            D_o_b DATE NOT NULL,
            Gender VARCHAR(20) NOT NULL,
            Email_id VARCHAR(20) NOT NULL,
            Marital_status VARCHAR(20) NOT NULL,
            Address VARCHAR(20) NOT NULL,
            City VARCHAR(20) NOT NULL,
            Pincode INT NOT NULL, 
            State VARCHAR(20) NOT NULL,
            Religion VARCHAR(20) NOT NULL,
            Category VARCHAR(20) NOT NULL,
            Income VARCHAR(20) NOT NULL,
            Education VARCHAR(20) NOT NULL,
            Occupation VARCHAR(20) NOT NULL,
            PAN_no INT NOT NULL,
            Aadhar INT NOT NULL,
            Senior_citizen VARCHAR(20) NOT NULL,
            Existing_account VARCHAR(20) NOT NULL,
            Account_no VARCHAR(20) NOT NULL,
            card_no VARCHAR(20) NOT NULL,
            Pin INT NOT NULL,
            balance varchar(20) NOT NULL,
            PRIMARY KEY(Account_no)
            )"""
        #mycursor.execute(sql)

        abc = "insert into atm values('%s','%s',%s,'%s','%s','%s','%s','%s',%s,'%s','%s','%s','%s','%s','%s',%s,%s,'%s','%s',%s,%s,%s,%s)" %(namee, fnamee,dob.get(),var1.get(), emailidd,var2.get(), addd, cityy, pincodee, statee, religionn, categoryy, incomee,qualifyy, occupationn, pannoo, aadharr,var3.get(),var4.get(),acno,card_no,pin,balance)
        mycursor.execute(abc)
        conn.commit()
        messagebox.showinfo("SUBMITTED", "YOUR FORM HAS BEEN SUBMITTED.")
        win3.destroy()
        win.destroy()





```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
