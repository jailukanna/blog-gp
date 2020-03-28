---
title: tkinter example gui obj (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'gui obj'


Modules used in program: 
* `import random`
* `import Tkinter`

## python gui obj

Python tkinter example: gui obj

```python
import Tkinter
import random


class MyApp:

    def __init__(self):
        self.window = Tkinter.Tk()

        self.result_text_var = Tkinter.DoubleVar(self.window, value='Nisi se ugibal')

        self.secret = random.randint(1, 100)

        self.greeting = Tkinter.Label(self.window, text='Guess secret number!')
        self.greeting.pack()

        self.guess = Tkinter.Entry(self.window)
        self.guess.pack()

        self.submit = Tkinter.Button(self.window, text='Submit', command=self.check_guess)
        self.submit.pack()

        self.result_text_label = Tkinter.Label(self.window, textvariable=self.result_text_var)
        self.result_text_label.pack()

        # potrdi vnos z Enter tipko
        self.window.bind('<Return>', lambda _: self.check_guess())

    def run(self):
        self.window.mainloop()

    def check_guess(self):
        if int(self.guess.get()) == self.secret:
            result_text = 'CORRECT!'
            self.secret = random.randint(1, 100)
        elif int(self.guess.get()) > self.secret:
            result_text = 'WRONG! Too high!'
        else:
            result_text = 'WRONG! Too low!'

        self.result_text_var.set(result_text)


app = MyApp()
app.run()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
