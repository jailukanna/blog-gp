---
title: matplotlib example OPLS (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'OPLS'


Modules used in program: 
* `import numpy`

## python OPLS

Python matplotlib example: OPLS

```python
import numpy

class OPLS_EP:
    def __init__(self, X,comp):   
        std=numpy.std(X,axis=0)
        std=std.reshape(len(std),1)
        Xstd=numpy.ones((X.shape[0],1))*std.T
        Xuv=X/Xstd
        self.comp=comp
        Y=numpy.ones((X.shape[0],1))
        w=Xuv.T.dot(Y).dot(numpy.linalg.inv(numpy.sqrt(Y.T.dot(Y))))
        t=Xuv.dot(w).dot(numpy.linalg.inv(w.T.dot(w)))
        self.Wo=[]
        self.To=[]
        self.Po=[]
        for x in range(0, comp-1):
            p=Xuv.T.dot(t).dot(numpy.linalg.inv(t.T.dot(t)))
            wo=p-w
            to=Xuv.dot(wo).dot(numpy.linalg.inv(wo.T.dot(wo)))
            po=Xuv.T.dot(to).dot(numpy.linalg.inv(to.T.dot(to)))
            Xuv=Xuv-to.dot(po.T)
            t=Xuv.dot(w).dot(numpy.linalg.inv(w.T.dot(w)))
            if x == 0:
                To=to
                Wo=wo
                Po=po
            else:
                To=numpy.concatenate((To,to), axis=1)
                Po=numpy.concatenate((Po,po), axis=1)
                Wo=numpy.concatenate((Wo,wo), axis=1)
                
            
        self.t=Xuv.dot(w).dot(numpy.linalg.inv(w.T.dot(w)))
        self.c=Y.T.dot(t).dot(numpy.linalg.inv(t.T.dot(t)))
        self.yhat=t.dot(self.c)
        ydiff=Y-self.yhat
        sstot=len(Y)
        ssres=ydiff.T.dot(ydiff)
        self.r2=1-ssres/sstot
        std=numpy.std(Xuv,axis=0)
        std=std.reshape(len(std),1)
        
        self.wlatent=w/std
        self.w = w
        if comp > 1:
            self.To=To
            self.Po=Po
            self.Wo=Wo

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
