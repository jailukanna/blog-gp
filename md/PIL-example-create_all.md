---
title: PIL example create all (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'create all'

Functions in program: 
* `def main():`

Modules used in program: 
* `import putquote`

## python create all

Python PIL example: create all

```python
# Create meme of all names in a given list `names_path`

names_path = 'pratenici.txt' # path of list of names to process (za/protiv/neglasal)
img_path = 'source_plain_imgs'
target_path = 'target_meme_imgs'
import putquote
 
def main():
    all_p = []
    with open(names_path, 'r') as f:
        for line in f:
            all_p.append(line.decode('utf-8').strip())
            
    for p in all_p:
        try:
            f = open(img_path + '/' + p + '.jpg')
        except Exception, exc:
            print(p)
            #print(exc)

        putquote.create(img_path, target_path, p)
            
    
if __name__=='__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
