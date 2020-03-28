---
title: tkinter example 100 GAME SAFI (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example '100 GAME SAFI'

Functions in program: 
* `def fan():`
* `def san():`

Modules used in program: 
* `import tkinter.simpledialog`
* `import tkinter.messagebox`

## python 100 GAME SAFI

Python tkinter example: 100 GAME SAFI

```python
from tkinter import *
import tkinter.messagebox
import tkinter.simpledialog
window = Tk()

window.title("100Game")
window.geometry("300x300")


my_text = Label (window, text='Hello Gamers!', bg="#AAAAFF", font="Arial 16 bold", width=25, height=3)
my_text.pack()

menu_bar = Menu(window)
file_menu = Menu(menu_bar, tearoff=0)
file_menu.add_command(label="new")
file_menu.add_command(label="open")
file_menu.add_command(label="exit!")
menu_bar.add_cascade(label="File", menu=file_menu)
window.config(menu=menu_bar)



def san():
    R='x'
    while R=='x':
        PlayerOne = tkinter.simpledialog.askstring("Player One Name", "Enter Your Name")
        PlayerTwo = tkinter.simpledialog.askstring("Player Two Name", "Enter Your Name")
        tkinter.messagebox.showinfo("100Game", "Welcome "+PlayerOne+" And "+PlayerTwo)
        tkinter.messagebox.showinfo("100Game", "Game Rule: Each Player Have To Choose A Number Between 1 And 10,Who Reach 100 Is The Winner,Let's Play")
        total = 0
        while True:
            while total != 100:
                pOneNum = tkinter.simpledialog.askfloat((PlayerOne+" Turn"), "Enter A Num Between 1 And 10")
                if (pOneNum > 9) or (pOneNum < 0) :
                    tkinter.messagebox.showinfo("100Game", "Sorry,you lost your turn" )
                    break
                total += pOneNum
                tkinter.messagebox.showinfo("100Game", (PlayerOne , " Now You Reach :  ",total) )
                if (total == 100 ):
                    tkinter.messagebox.showinfo("100Game", (PlayerOne+" Finaly You Have Got The 100,Good Job,You're The Winner!,Good Luck "+PlayerTwo) )
                if (total > 100):
                    tkinter.messagebox.showinfo("100Game", "Sorry,you lost your turn" )
                    total -=pOneNum
                break
            if (total==100):
                answer = tkinter.messagebox.askquestion("Q1", "Play Again?")
                if answer == ("yes"):
                    break
            while total != 100:
                pTwoNum = tkinter.simpledialog.askfloat((PlayerTwo+" Turn"), "Enter A Num Between 1 And 10")
                if (pTwoNum > 9) or (pTwoNum < 0) :
                    tkinter.messagebox.showinfo("100Game", "Sorry,you lost your turn" )
                    break
                total += pTwoNum
                tkinter.messagebox.showinfo("100Game", (PlayerTwo ," Now You Reach :  ",total) )
                if (total == 100 ):
                    tkinter.messagebox.showinfo("100Game", (PlayerTwo+" Finaly You Have Got The 100,Good Job,You're The Winner!,Good Luck "+PlayerOne))
                if (total > 100):
                    tkinter.messagebox.showinfo("100Game", "Sorry,you lost your turn")
                    total -= pTwoNum
                break
            if (total==100):
                answer = tkinter.messagebox.askquestion("Q1", "Play Again?")
                if answer == "yes":
                    break




b = Button(command=san, text="Play With Friend!")
b.pack(fill=X, expand=0)

def fan():
    R='x'
    while R=='x':
        PlayerOne = tkinter.simpledialog.askstring("Player One Name", "Enter Your Name")
        tkinter.messagebox.showinfo("100Game", ("Welcome "+PlayerOne))
        tkinter.messagebox.showinfo("100Game", "Game Rule: Each Player Have To Choose A Number Between 1 And 10,Who Reach 100 Is The Winner,Let's Play")
        total = 0
        while True:
            while total != 100:
                pOneNum = tkinter.simpledialog.askfloat((PlayerOne+" Turn"), "Enter A Num Between 1 And 10")
                if (pOneNum > 9) or (pOneNum < 0) :
                    tkinter.messagebox.showinfo("100Game", "Sorry,you lost your turn" )
                    break
                total += pOneNum
                tkinter.messagebox.showinfo("100Game", (PlayerOne , " Now You Reach :  ",total) )
                if (total == 100 ):
                    tkinter.messagebox.showinfo("100Game", (PlayerOne+" Finaly You Have Got The 100,Good Job,You're The Winner!,Good Luck Computer") )
                break
                if (total > 100):
                    tkinter.messagebox.showinfo("100Game", "Sorry,you lost your turn" )
                    total -=pOneNum
                break
                answer = tkinter.messagebox.askquestion("Q1", "Play Again?")
                if answer == "yes":
                    break
            while total != 100:
                import random,time
                computer = random.randint(1,10)
                total += computer
                tkinter.messagebox.showinfo("100Game", ("the Total Now Is : ",total) )
                if (total == 100 ):
                    tkinter.messagebox.showinfo("100Game", ( "Computer You Have Got The 100,Good Job,You're The Winner!,Good Luck ",PlayerOne) )
                break
                if (total > 100):
                    tkinter.messagebox.showinfo("100Game", "Sorry,you lost your turn" )
                    total -=computer
                break 

            if (total==100):
                answer = tkinter.messagebox.askquestion("Q1", "Play Again?")
                if answer == "yes":
                    break




b = Button(command=fan, text="Play With Computer!")
b.pack( expand=0)





window.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
