---
title: mysql example homepage (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'homepage'

Functions in program: 
* `def clear():`
* `def exit():`
* `def homepage():`
* `def checklogin():`

Modules used in program: 
* `import mysql.connector`
* `import time`
* `import signup`
* `import login`

## python homepage

Python mysql example: homepage

```python
import login
import signup
from tkinter import *
import time
from tkinter import messagebox
import mysql.connector



def checklogin():
    #global cno
    cno = int(c.get())
    pno=int(c1.get())
    conn = mysql.connector.connect(user="root", password="ashish", host="localhost", database="atm")
    mycursor = conn.cursor()

    a="SELECT * FROM ATM where card_no=%s "
    mycursor.execute(a,(cno,))
    total=mycursor.fetchall()

    for i in total:
        card_check=i[20]
        pin_check=i[21]

    if(pin_check==pno):
        messagebox.showinfo("WELCOME","YOU ARE IN.")
        login.Login(cno,win1)
    else:
        messagebox.showerror("ERROR", "WRONG PIN")

    conn.close()
    return cno,win1

def homepage():
    global win1,c,c1
    win1=Tk()
    win1.title("ATM HOMEPAGE")
    win1.geometry("1000x500")
    win1.config(bg="yellow")
    win1.resizable(width=False, height=False)
    localtime = time.asctime(time.localtime(time.time()))

    f=LabelFrame(win1).grid()
    l=Label(f,text="  WELCOME  TO  ATM ",font=("georgia",61),bg="yellow")
    l.grid(row=1,column=6,padx=50,pady=10)
    l1 = Label(f, text=localtime, font=("arial", 30, "bold"), bg="yellow", fg="blue").grid(row=2,column=6,padx=50,pady=10)

    f1=Frame(win1,bg="yellow")
    l1=Label(f1,text="CARD NO.",font=("georgia",25),bg="yellow").grid(row=3,column=5,padx=10,pady=10)
    c=Entry(f1,font="30")
    c.grid(row=3,column=6,padx=10,pady=0)

    l2=Label(f1,text="PIN NO.",font=("georgia",25),bg="yellow").grid(row=4,column=5,padx=10,pady=10)
    c1=Entry(f1,font="30",show="*")
    c1.grid(row=4,column=6,padx=10,pady=0)
    
    b1=Button(f1,text="Login",bg="blue",command=checklogin).grid(row=5,columnspan=10,padx=10,pady=10,ipadx=10)
    b2=Button(f1,text="Sign Up",bg="blue",command=signup.signup).grid(row=5,column=6,padx=4,pady=10)
    b3 = Button(f1, text="Clear",bg="blue",command=clear).grid(row=6,columnspan=11,padx=0,ipadx=10)
    b4 = Button(f1, text="Exit", bg="blue",command=exit).grid(row=6, column=6, padx=0,ipadx=10)
    f1.grid(row=3,column=6)

    win1.mainloop()


def exit():
    win1.destroy()

def clear():
    exit()
    homepage()


homepage()


    












```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
