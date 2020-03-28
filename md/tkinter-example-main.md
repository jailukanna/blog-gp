---
title: tkinter example main (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'main'


Modules used in program: 
* `import random`

## python main

Python tkinter example: main

```python
#!/usr/bin/python3

import random

try:
    import tkinter as Tkin
except:
    import Tkinter as Tkin

class numgame:

    def __init__(self):
        self.root = Tkin.Tk()
        self.vcmd = (self.root.register(self.on_validate), "%P")
        self.root.title('Number Game')
        self.root.resizable(width=Tkin.FALSE, height=Tkin.FALSE)
        self.num = random.randint(1, 100)
        self.big = int(100)
        self.small = int(1)
        self.done = 'No'
        self.count = 0

    def finish(self):

        def yes():
            self.num = random.randint(1, 100)
            self.big = int(100)
            self.small = int(1)
            self.done = 'No'
            hint = 'No hint yet, start the game(The number is between 1 and 100)'
            self.status.configure(text='Status:\nDone: ' + self.done + '\nMost: ' + str(self.big) + '\nLeast: ' + str(self.small))
            self.hint.configure(text=hint)
            master.destroy()
            return self

        master = Tkin.Tk()
        master.title('Continue?')
        master.resizable(width=Tkin.FALSE, height=Tkin.FALSE)

        top = Tkin.Frame(master)
        top.pack(expand=True, fill=Tkin.BOTH, side=Tkin.TOP)

        bottom = Tkin.Frame(master)
        bottom.pack(expand=True, fill=Tkin.BOTH, side=Tkin.TOP)

        label = Tkin.Label(top, text='Would you like to continue?')
        label.pack()

        yes = Tkin.Button(bottom, text='Yes', command=yes)
        yes.pack(side=Tkin.LEFT, fill=Tkin.BOTH, expand=True)

        no = Tkin.Button(bottom, text='No', command=exit)
        no.pack(side=Tkin.RIGHT, fill=Tkin.BOTH, expand=True)

        self.center(master)

        master.mainloop()
        return self

    def on_validate(self, new_value):
        try:
            if new_value.strip() == "": return True
            value = int(new_value)
            if value < -1 or value > 100:
#                self.disallow()
                return False
        except ValueError:
#            self.disallow()
            return False

        return True

    def center(self, root):
        root.update_idletasks()
        width = root.winfo_width()
        height = root.winfo_height()
        x = (root.winfo_screenwidth() // 2) - (width // 2)
        y = (root.winfo_screenheight() // 2) - (height // 2)
        root.geometry('{}x{}+{}+{}'.format(width, height, x, y))

    def keyPress(self, event):
        if event.char in ('1', '2', '3', '4', '5', '6', '7', '8', '9', '0'):
            return None
        elif event.keysym not in ('BackSpace', 'Left', 'Right', 'Return', 'Delete'):
            return 'break'

    def status(self):
        done = False
        number = self.number.get()
        if int(number) == self.num:
            self.done = 'Yes'
            hint = 'You did it! The number was ' + str(self.num) + '.'
            done = True
        elif int(number) >= self.num:
            hint = 'The number is smaller than ' + number + '.'
            if int(number) <= self.big:
                self.big = int(number)
            self.count += 1
        elif int(number) <= self.num:
            hint = 'The number is bigger than ' + number + '.'
            if int(number) >= self.small:
                self.small = int(number)
            self.count += 1
        self.status.configure(text='Status:\nDone: ' + self.done + '\nMost: ' + str(self.big) + '\nLeast: ' +
                                   str(self.small) + '\nTries: ' + str(self.count))
        self.hint.configure(text=hint)
        self.root.geometry('{}x{}'.format('389', '167'))
        if done:
            self.finish()
        self.number.delete(number)
#        print(str(numgme.root.winfo_width()) + ' ' + str(numgme.root.winfo_height()))

    def startgame(self):
        top = Tkin.Frame(self.root)
        top.pack(expand=True, fill=Tkin.BOTH, side=Tkin.TOP)

        bottom = Tkin.Frame(self.root)
        bottom.pack(expand=True, fill=Tkin.BOTH, side=Tkin.TOP)

        labelnum = Tkin.Label(top, text='Enter a number between 1 and 100: ')
        labelnum.pack(fill=Tkin.NONE, side=Tkin.TOP)

        self.number = Tkin.Entry(top, justify=Tkin.CENTER)
        self.number.bind('<KeyPress>', self.keyPress)
        self.number.configure(validate="key", validatecommand=self.vcmd)

        submit = Tkin.Button(top, text='Submit', command=self.status)
        self.hint = Tkin.Label(bottom, text='No hint yet, start the game(The number is between 1 and 100)')
        self.status = Tkin.Label(bottom, text='Status:\nDone: No\nMost: 100\nLeast: 0')
        self.number.pack(fill=Tkin.NONE, side=Tkin.TOP)
        submit.pack(fill=Tkin.NONE, side=Tkin.TOP)

        self.status.pack()
        self.hint.pack()

        self.center(self.root)

        self.root.mainloop()
#        self.root.geometry('{}x{}'.format('389', '167'))  ##as a reference

if __name__ == '__main__':
    numgme = numgame()
    numgme.startgame()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
