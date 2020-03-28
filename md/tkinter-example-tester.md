---
title: tkinter example tester (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'tester'

Functions in program: 
* `def makeTester():`

Modules used in program: 
* `import tkinter.filedialog`
* `import tkinter`
* `import random`
* `import pickle`
* `import sys`
* `import os`

## python tester

Python tkinter example: tester

```python
#!/usr/bin/env python3

import os
import sys
import pickle
import random
import tkinter
import tkinter.filedialog


class bg:
    HEADER = '\033[95m'
    OKBLUE = '\033[94m'
    OKGREEN = '\033[92m'
    WARNING = '\033[93m'
    FAIL = '\033[91m'
    ENDC = '\033[0m'
    BOLD = '\033[1m'
    UNDERLINE = '\033[4m'


class Answer:

    def __init__(self, line):
        self.text = line[1:].strip()
        self.correct = line.startswith('+')

    def __str__(self):
        return self.text


class Question:

    def __init__(self, text):
        self.text = text
        self.answers = []

    def addAnswer(self, answer):
        self.answers.append(answer)
        random.shuffle(self.answers)

    def test(self, answerStr):
        for i, answer in enumerate(self.answers):
            if answer.correct and str(i + 1) not in answerStr:
                return False
            elif not answer.correct and str(i + 1) in answerStr:
                return False

        return True

    def getCorrectAnswers(self):
        str = ''

        str += bg.OKGREEN + 'Spravne odpovedi:\n' + bg.ENDC
        for i, answer in enumerate(self.answers):
            if answer.correct:
                str += '{}{}){} {}\n'.format(bg.OKGREEN, i + 1, bg.ENDC, answer)

        str += bg.FAIL + 'Chybne odpovedi:\n' + bg.ENDC
        for i, answer in enumerate(self.answers):
            if not answer.correct:
                str += '{}{}){} {}\n'.format(bg.FAIL, i + 1, bg.ENDC, answer)

        return str

    def __str__(self):
        return self.text


class Tester:

    def __init__(self, fileName):
        self.fileName = fileName
        self.questions = []
        self.cntCorrect = 0
        self.cntIncorrect = 0

        self.load()
        self.shuffle()

    def load(self):
        with open(self.fileName, encoding='utf8') as file:
            question = None

            for line in file:
                line = line.strip()

                if line == '':
                    pass
                elif line.startswith('+') or line.startswith('-'):
                    question.addAnswer(Answer(line))
                else:
                    question = Question(line)
                    self.questions.append(question)

    def shuffle(self):
        random.shuffle(self.questions)

    def run(self):

        cntQuestions = 0

        for question in self.questions:

            if cntQuestions < self.cntCorrect + self.cntIncorrect:
                cntQuestions += 1
                continue

            self.save()

            print('{}/{} :: {}'.format(self.cntCorrect + self.cntIncorrect, len(self.questions), question))

            for i, answer in enumerate(question.answers):
                print('{}) {}'.format(i + 1, answer))

            answerStr = input('Zadejte odpoved: ')
            cntQuestions += 1

            if question.test(answerStr):
                self.cntCorrect += 1
                print()
                print(bg.OKGREEN + 'Spravne' + bg.ENDC + ' - ' + self.getStats())
                print()
            else:
                self.cntIncorrect += 1
                print()
                print(bg.FAIL + 'Spatne' + bg.ENDC + ' - ' + self.getStats())
                print()
                print(question.getCorrectAnswers())

        os.remove('__tester.pickle')

    def getStats(self):
        total = self.cntCorrect + self.cntIncorrect
        percentage = int((self.cntCorrect / total) * 100)

        return '{} ✔, {} ✗ - {}%\n'.format(self.cntCorrect, self.cntIncorrect, percentage)

    def save(self):
        with open('__tester.pickle', 'wb') as file:
            pickle.dump(self, file)


def makeTester():
    fileName = None

    if len(sys.argv) > 1:
        fileName = sys.argv[1]
    else:
        root = tkinter.Tk()
        root.withdraw()
        fileName = tkinter.filedialog.askopenfilename()
        root.destroy()

    if fileName == None or fileName == '' or fileName == ():
        print('Zadejte spravne soubor s otazkami')
        sys.exit(1)

    if os.path.isfile('__tester.pickle'):
        ans = input('Chcete pokracovat tam, kde jste naposledy skoncili? a/n: ')

        if ans == 'a':
            with open('__tester.pickle', 'rb') as file:
                return pickle.load(file)
        else:
            return Tester(fileName)
    else:
        return Tester(fileName)


if __name__ == '__main__':
    try:
        tester = makeTester()
        tester.run()
    except EOFError:
        pass
    except KeyboardInterrupt:
        pass


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
