---
title: tkinter example chuseok (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'chuseok'


Modules used in program: 
* `import random`
* `import sys`

## python chuseok

Python tkinter example: chuseok

```python
import sys
import random

pyVersion = sys.version_info[0]

if pyVersion == 2:
    import Tkinter as tk
else:
    import tkinter as tk

    xrange = range

    def range(*args):
        return list(xrange(*args))

class Chuseok(tk.Frame, object):
    def __init__(self, master=None):
        super(Chuseok, self).__init__(master)

        codes = "#&$&%&&&'&(&)&*&+&,&-&.&/&0&1&2&3&4&5&6&7&8&9"\
                "&:&;&<&=&>&?&@&A&B&C&D&%*'*)***+*-*.*/*1*2*3*"\
                "5*7*%+'+)+++-+/+1+3+5+7+%,&,',),*,+,-,.,/,1,2"\
                ",3,5,6,7,%-'-)-+---1-6-%.'.).+.-.1.6.(0)0*0,0"\
                ".000204050608090:0<0=0>0@0B0(1,1.101214181<1>"\
                "1@1A1(2,2-2.202224252628292:2<2>2@2(3,3.30323"\
                "6383<3>3@3A3(4)4*4,4.40414244454648494:4<4=4>"\
                "4@4B44819595:5;1<5<4=#A$A%A&A'A(A)A*A+A,A-A.A"\
                "/A0A1A2A3A4A5A6A7A8A9A:A;A<A=A>A?A@AAABACADA"

        markCount = int(len(codes) / 2)

        self.marks = [
                (ord(codes[i * 2]) - ord(' '), ord(codes[i * 2 + 1]) - ord(' '))
                for i in xrange(markCount)
        ]

        random.shuffle(self.marks)

        self.width = 40
        self.height = 40
        self.unit = 10
        self.ending = 0

        self.board = tk.Canvas(
                self,
                width=self.width * self.unit,
                height=self.height * self.unit,
                highlightthickness=0,
                bg='#eeeeee'
        )

        for x in xrange(self.width - 1):
            self.board.create_line((
                self.unit * (x + 1), 0,
                self.unit * (x + 1), self.height * self.unit,
            ), fill='#000000')

        for y in xrange(self.height - 1):
            self.board.create_line((
                0, self.unit * (y + 1),
                self.width * self.unit, self.unit * (y + 1)
            ), fill='#000000')

        self.board.pack()
        self.animate()

    def mark(self, x, y):
        self.board.create_rectangle((
            self.unit * x, self.unit * y,
            self.unit * (x + 1), self.unit * (y + 1)
        ), fill='#000000')

    def animate(self):
        if len(self.marks) > 0:
            x, y = self.marks.pop()
            self.mark(x, y)
            self.after(10, self.animate)
        elif self.ending == 0:
            self.ending = 1
            self.after(500, self.animate)
        elif self.ending == 1:
            self.board.create_rectangle((
                0, 0, self.width * self.unit, self.height * self.unit
            ), fill='#eeeeee', tags='blank')
            self.ending = 2
            self.after(500, self.animate)
        elif self.ending == 2:
            self.board.delete('blank')
            self.ending = 3
            self.after(500, self.animate)
        elif self.ending == 3:
            self.board.create_rectangle((
                0, 0, self.width * self.unit, self.height * self.unit
            ), fill='#eeeeee', tags='blank')
            self.ending = 4
            self.after(500, self.animate)
        elif self.ending == 4:
            self.board.delete('blank')

if __name__ == '__main__':
    root = tk.Tk()
    root.title('Letter')
    root.resizable(0, 0)
    app = Chuseok(root)
    app.pack()
    root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
