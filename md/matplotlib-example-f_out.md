---
title: matplotlib example f out (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'f out'

Functions in program: 
* `def f(Comp,Output):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import numpy`
* `import OPLS`

## python f out

Python matplotlib example: f out

```python
import OPLS
import numpy
import matplotlib
import matplotlib.pyplot as plt


def f(Comp,Output):
  
    comp=int(Comp)
    font = {'family' : 'Verdana',
        'size'   : 22}

    matplotlib.rc('font', **font)
    model=OPLS.OPLS_EP(X,comp)
    if Output == "UMEA":
        N = len(model.w)
        x = range(N)
        fig=plt.figure(figsize=(18, 16), dpi= 80, facecolor='w', edgecolor='k')
        plt.scatter(model.w , model.wlatent,c="w")

        plt.title('UMEA-PLOT')
        plt.xlabel('Wdirect')
        plt.ylabel('Wlatent')

        [xmin, xmax] = plt.xlim()
        [ymin, ymax] = plt.ylim() 

        plt.fill(numpy.array([-2, 2, 2, -2, -2]), numpy.array([-2, -2, 2, 2, -2]), "r",alpha=0.50,label="NS")
        plt.fill(numpy.array([-2, 0, 0, -2, -2]), numpy.array([ymin, ymin, -2, -2, ymin]), "c",alpha=0.50,label="LS")
        plt.fill(numpy.array([2, 0, 0, 2, 2]), numpy.array([ymax, ymax, 2, 2, ymax]), "c",alpha=0.50)
        plt.fill(numpy.array([xmin, -2, -2, xmin, xmin]), numpy.array([ymin, ymin, -2, -2, ymin]), "g",alpha=0.50)
        plt.fill(numpy.array([xmax, 2, 2, xmax, xmax]), numpy.array([ymax, ymax, 2, 2, ymax]), "g",alpha=0.50,label="DS")
        plt.scatter(model.w , model.wlatent,c="k",s=150,label="VAR")
        plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
        plt.xlim(xmin,xmax)
        plt.ylim(ymin,ymax)
        plt.grid(color='k', linestyle='--', linewidth=1)
        plt.show()

        [a,b]=numpy.nonzero(numpy.absolute(model.w)>2)
        [a2,b]=numpy.nonzero(numpy.absolute(model.wlatent)>2)
        print('Number of significant Variables')
        print('Direct: ' +str(len(a)))
        print('Latent: ' +str(len(a2)))

    if Output == "Yhat":
        R2=model.r2
        R2=float(R2)
        R2=numpy.round(R2,3)
        N = len(model.yhat)
        x = range(N)
        width = 1/1.5
        fig=plt.figure(figsize=(18, 16), dpi= 80, facecolor='w', edgecolor='k')

        plt.bar(x, model.yhat,width,label="Diet 2",color='r')
        plt.bar(x[0:31], model.yhat[0:31],width,label="Diet 1",color='k')
        plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
        plt.xlabel('Obervation Number')
        plt.ylabel('Yhat')
        plt.title('R2Y =' +str(R2))
        plt.show()
        
    if Output == "Score":
        R2=model.r2
        R2=float(R2)
        R2=numpy.round(R2,3)
        N = len(model.yhat)
        x = range(N)
        width = 1/1.5
        if model.comp==1:
            fig=plt.figure(figsize=(18, 16), dpi= 80, facecolor='w', edgecolor='k')
            plt.bar(x, model.t,width,label="Diet 2",color='r')
            plt.bar(x[0:31], model.t[0:31],width,label="Diet 1",color='k')
            plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
            plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
            plt.xlabel('Obervation Number')
            plt.ylabel('t[1]')
            plt.title('R2Y =' +str(R2))
            plt.show()
        else:
            fig=plt.figure(figsize=(18, 16), dpi= 80, facecolor='w', edgecolor='k')
            plt.scatter(model.t , model.To[:,0],c="k",s=150,label="Diet 2")
            plt.scatter(model.t[0:31] , model.To[0:31,0],c="r",s=150,label="Diet 1")
            plt.legend(bbox_to_anchor=(1.05, 1), loc=2, borderaxespad=0.)
            plt.xlabel('t[1]')
            plt.ylabel('To[1]')
            plt.title('R2Y =' +str(R2))
            plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
