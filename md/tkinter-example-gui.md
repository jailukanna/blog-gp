---
title: tkinter example gui (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'gui'

Functions in program: 
* `def check_guess():`

Modules used in program: 
* `import tkMessageBox`
* `import random`
* `import Tkinter`

## python gui

Python tkinter example: gui

```python
import Tkinter
import random
import tkMessageBox


def check_guess():
    if int(guess.get()) == secret:
        result_text = 'CORRECT!'
    elif int(guess.get()) > secret:
        result_text = 'WRONG! Too high!'
    else:
        result_text = 'WRONG! Too low!'

    result_text_var.set(result_text)
    tkMessageBox.showinfo('Result', result_text)


window = Tkinter.Tk()

result_text_var = Tkinter.DoubleVar(window, value='Nisi se ugibal')

secret = random.randint(1, 100)

greeting = Tkinter.Label(window, text='Guess secret number!')
greeting.pack()

guess = Tkinter.Entry(window)
guess.pack()

submit = Tkinter.Button(window, text='Submit', command=check_guess)
submit.pack()

result_text_label = Tkinter.Label(window, textvariable=result_text_var)
result_text_label.pack()

# potrdi vnos z Enter tipko
window.bind('<Return>', lambda _: check_guess())

window.mainloop()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
