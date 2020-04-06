---
title: mysql example lern simple webapp (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'lern simple webapp'

Functions in program: 
* `def check_status() -> str:`
* `def do_logout() -> str:`
* `def do_login() -> str:`
* `def page3() -> str:`
* `def page2() -> str:`
* `def page1() -> str:`
* `def hello() -> str:`

## python lern simple webapp

Python mysql example: lern simple webapp

```python
from flask import Flask,session
from checker import check_logged_in


app = Flask(__name__)
app.secret_key = 'Nastursuk'
@app.route('/')
def hello() -> str:
    return 'Hello  from the simple webapp.'

@app.route('/page1')
@check_logged_in
def page1() -> str:
    return 'This is page 1'

@app.route('/page2')
@check_logged_in
def page2() -> str:
    return 'This is page 2'

@app.route('/page3')
@check_logged_in
def page3() -> str:
    return 'This is page 3'

@app.route('/login')
def do_login() -> str:
    session['logged_in'] = True
    return 'You are now logged in'

@app.route('/logout')
def do_logout() -> str:
    session.pop('logged_in')
    return 'You are now logged out'

@app.route('/status')
def check_status() -> str:
    if 'logged_in' in session:
        return 'You are now logged in'
    return 'You are now logged out'

if __name__ == '__main__':
    app.run(debug=True)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
