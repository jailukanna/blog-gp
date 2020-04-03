---
title: matplotlib example loading MNIST (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'loading MNIST'

Functions in program: 
* `def load_data():`

## python loading MNIST

Python matplotlib example: loading MNIST

```python
def load_data():
    (x_train, y_train), (x_test, y_test) = mnist.load_data()
    x_train = (x_train.astype(np.float32) - 127.5)/127.5
    
    # convert shape of x_train from (60000, 28, 28) to (60000, 784) 
    # 784 columns per row
    x_train = x_train.reshape(60000, 784)
    return (x_train, y_train, x_test, y_test)
(X_train, y_train,X_test, y_test)=load_data()
print(X_train.shape)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
