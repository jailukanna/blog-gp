---
title: mysql example login (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'login'

Functions in program: 
* `def bal_enquiry():`
* `def wexit():`
* `def cashwithdraww():`
* `def cashwithdraw():`
* `def cexit():`
* `def change_pin():`
* `def changepin():`
* `def dexit():`
* `def ddeposit():`
* `def deposit():`
* `def exit():`
* `def Login(cno,win1):`

Modules used in program: 
* `import mysql.connector`

## python login

Python mysql example: login

```python
from tkinter import *
import mysql.connector
#from homepage import checklogin
from tkinter import messagebox


def Login(cno,win1):
    global win2,no
    no = cno
    win1.destroy()
    win2=Tk()
    win2.title("Transaction")
    win2.geometry("700x500")
    win2.config(bg="purple")
    win2.resizable(width=False, height=False)

    l1=Label(win2,text="Please Select Your Transaction",font=("georgia",30,"bold"),bg="purple").grid(padx=20,pady=20)

    f=Frame(win2,bg="purple")
    b1=Button(f,text="DEPOSIT",font="15",bg="blue",command=deposit).grid(row=2,columnspan=4,padx=10,pady=20,ipadx=40)
    b2 = Button(f, text="PIN CHANGE",font="15",bg="blue",command=changepin).grid(row=12,columnspan=4,padx=10,pady=20,ipadx=25)
    f.grid()

    f1=Frame(win2,bg="purple")
    b4 = Button(f1, text="CASH WITHDRAWL", font="15",bg="blue",command=cashwithdraw).grid(row=2, column=6, padx=20, pady=20)
    b5 = Button(f1, text="BALANCE ENQUIRY", font="15",bg="blue",command=bal_enquiry).grid(row=12, column=6, padx=30, pady=20)
    b3 = Button(f1, text="EXIT", font="15", bg="blue", command=exit).grid(row=20, column=6, padx=10, pady=10, ipadx=55)
    f1.grid()
    win2.mainloop()

def exit():
    messagebox.showinfo("GOOD BYE","THANK YOU FOR USING OUR ATM SERVICE.")
    win2.destroy()

def deposit():
    global win4,e
    win4=Tk()
    win4.config(bg="orange")
    win4.title("MONEY DEPOSITION")
    win4.resizable(width=False, height=False)
    l=Label(win4,text="ENTER AMOUNT YOU WANT TO DEPOSIT",font=("georgia",30),bg="orange").grid(padx=10,pady=10)

    e=Entry(win4,font="15")
    e.grid(padx=10,pady=10)
    b=Button(win4,text="DEPOSIT",font="15",bg="blue",command=ddeposit).grid(padx=10,pady=10)
    b1 = Button(win4, text="EXIT", font="15", bg="blue",command=dexit).grid(padx=10,pady=10)
    win4.mainloop()

def ddeposit():
    global bal
    conn = mysql.connector.connect(user="root", password="ashish", host="localhost", database="atm")
    mycursor = conn.cursor()
    a = "SELECT * FROM ATM where card_no=%s "
    mycursor.execute(a, (no,))
    total = mycursor.fetchall()

    for i in total:
        bal = i[22]
    #print(bal,type(bal))
    sql_update_query = """Update atm set balance = %s where card_no = %s"""
    balance =int(bal)+int(e.get())
    mycursor.execute(sql_update_query,(balance,no))
    conn.commit()
    messagebox.showinfo("GREAT","YOUR BALANCE IS "+str(balance)+".")
    win4.destroy()

def dexit():
    win4.destroy()



def changepin():
    global win6,e,e1
    win6 = Tk()
    win6.config(bg="orange")
    win6.title("CHANGE PIN")
    win6.resizable(width=False, height=False)
    l = Label(win6, text="CHANGE YOUR PIN", font=("georgia", 30), bg="orange").grid(padx=10, pady=10)
    l1 = Label(win6, text="ENTER YOUR NEW PIN", font=("georgia", 15), bg="orange").grid(padx=10, pady=10)
    e = Entry(win6, font="15",show="*")
    e.grid(padx=10, pady=10)
    l2 = Label(win6, text="RE-ENTER YOUR NEW PIN", font=("georgia", 15), bg="orange").grid(padx=10, pady=10)
    e1 = Entry(win6, font="15",show="*")
    e1.grid(padx=10, pady=10)

    b = Button(win6, text="CHANGE PIN", font="15", bg="blue",command=change_pin).grid(padx=10, pady=10)
    b1 = Button(win6, text="EXIT", font="15", bg="blue", command=cexit).grid(padx=10, pady=10)
    win6.mainloop()

def change_pin():
    if (e.get()==e1.get()):
        pin=int(e.get())
        conn = mysql.connector.connect(user="root", password="ashish", host="localhost", database="atm")
        mycursor = conn.cursor()
        sql_update = """Update atm set Pin = %s where card_no = %s"""
        mycursor.execute(sql_update, (pin, no))
        conn.commit()
        messagebox.showinfo("GREAT", "YOUR NEW PIN IS " + str(pin) + ".")
        win6.destroy()

    else:
        messagebox.showerror("ERROR","PLEASE ENTER SAME PIN")

def cexit():
    win6.destroy()

def cashwithdraw():
    global win5,ee
    win5 = Tk()
    win5.config(bg="orange")
    win5.title("MONEY WITHDRAW")
    win5.resizable(width=False, height=False)
    l = Label(win5, text="ENTER AMOUNT YOU WANT TO WITHDRAW", font=("georgia", 30), bg="orange").grid(padx=10, pady=10)

    ee = Entry(win5, font="15")
    ee.grid(padx=10, pady=10)
    b = Button(win5, text="WITHDRAW", font="15", bg="blue",command=cashwithdraww).grid(padx=10, pady=10)
    b1 = Button(win5, text="EXIT", font="15", bg="blue", command=wexit).grid(padx=10, pady=10)
    win5.mainloop()

def cashwithdraww():
    global bal
    conn = mysql.connector.connect(user="root", password="ashish", host="localhost", database="atm")
    mycursor = conn.cursor()
    a = "SELECT * FROM ATM where card_no=%s "
    mycursor.execute(a, (no,))
    total = mycursor.fetchall()

    for i in total:
        bal = i[22]
    print(bal)
    if(ee.get()<bal):
        sql_update_query = """Update atm set balance = %s where card_no = %s"""
        balance = int(bal) - int(ee.get())
        mycursor.execute(sql_update_query, (balance, no))
        conn.commit()
        messagebox.showinfo("GREAT", "YOUR BALANCE IS " + str(balance) + ".")
        win5.destroy()
    else:
        messagebox.showerror("ERROR","ENTER A VALID AMOUNT.")

def wexit():
    win5.destroy()


def bal_enquiry():
    global bal
    conn = mysql.connector.connect(user="root", password="ashish", host="localhost", database="atm")
    mycursor = conn.cursor()
    a = "SELECT * FROM ATM where card_no=%s "
    mycursor.execute(a, (no,))
    total = mycursor.fetchall()

    for i in total:
        bal = i[22]

    messagebox.showinfo("BALANCE ENQUIRY","YOUR BALANCE IS "+bal+".")



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
