---
title: socket example hello (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'hello'

Functions in program: 
* `def echo_test():`
* `def hello():`
* `def echo_socket(ws):`

## python hello

Python socket example: hello

```python
# Copy of http://stackoverflow.com/a/20104705
from flask import Flask, render_template
from flask_sockets import Sockets

app = Flask(__name__)
app.debug = True

sockets = Sockets(app)

@sockets.route('/echo')
def echo_socket(ws):
    while True:
        message = ws.receive()
        ws.send(message[::-1])

@app.route('/')
def hello():
    return 'Hello World!'

@app.route('/echo_test', methods=['GET'])
def echo_test():
    return render_template('echo_test.html')

if __name__ == '__main__':
    app.run()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
