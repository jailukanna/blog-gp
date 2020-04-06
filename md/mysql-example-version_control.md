---
title: mysql example version control (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'version control'


## python version control

Python mysql example: version control

```python
from question import Question

# Add version control questions
version_control_questions = []

question = "In Git, how can I find out all the commits done on HEAD but not pushed to origin ?"
answer = "git log origin/master..HEAD"
version_control_questions.append(Question(question, answer))

question = "In Git, how can I find out what is available on origin/master for me to fetch ?"
answer = "git fetch origin/master"
version_control_questions.append(Question(question, answer))

question = "In Git, how can I view all the files which were part of a commit ?"
answer = "git log format=short --name-only \n See http://stackoverflow.com/questions/424071/how-do-i-list-all-the-files-for-a-commit-in-git"
version_control_questions.append(Question(question, answer))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
