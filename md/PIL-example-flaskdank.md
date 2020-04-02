---
title: PIL example flaskdank (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'flaskdank'

Functions in program: 
* `def creatememe():`

Modules used in program: 
* `import dankmeme`

## python flaskdank

Python PIL example: flaskdank

```python
#!/usr/bin/env python
# coding: utf-8

# In[ ]:


from flask import Flask, request, jsonify, render_template
import dankmeme
app = Flask(__name__)

@app.route('/creatememe')
def creatememe():
    i = request.args.get('i')
    l = request.args.get('l')

    u = request.args.get('u')
    d = request.args.get('d')
    o = request.args.get('o')


    #imgloc, upzero, botzero, outloc
    dankmeme.creatememe(i, l, u, d, o)
    return('ok')


# In[ ]:


if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5556, debug=True) 


# In[1]:


#import dankmeme


# In[ ]:





# In[2]:


#dankmeme.creatememe('/home/wcmckee/meme/meme/galleries/default/61585.jpg', 
###                    "/mnt/c/Users/luke/Downloads/impact.ttf",
#                    'CHECKS OVERSEAS WEATHER OF GIRL HE LIKES',
#                    'GETS THE COUNTRY WRONG', '/mnt/c/Users/luke/Desktop/t2.jpg')


# In[ ]:





# In[ ]:


#imgloc=/home/wcmckee/meme/meme/galleries/default/61585.jpg&upzero=CHECKS OVERSEAS WEATHER OF GIRL HE LIKES&
#          botzero=GETS THE COUNTRY WRONG&outloc=/mnt/c/Users/luke/Desktop/testies.jpg'



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
