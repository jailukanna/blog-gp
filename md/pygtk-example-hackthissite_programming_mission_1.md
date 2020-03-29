---
title: pygtk example hackthissite programming mission 1 (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'hackthissite programming mission 1'


## python hackthissite programming mission 1

Python pygtk example: hackthissite programming mission 1

```python
if __name__ == '__main__':
    #get wordlist
    f = open('wordlist.txt','r')
    wordlist = [line.strip()for line in f]
    f.close()
    #get given words
    f = open('words.txt')
    words = [line[1:].strip()for line in f]
    f.close()
    #result will be stored in this list
    result = []
    
    #loop over given words
    for word in words:
        #put characters of word into a list and sort them
        lword = [c for c in word]
        lword.sort()
        
        #loop over candidatelist
        for candidate in wordlist:
            lcandidate = [c for c in candidate]
            lcandidate.sort()
            #if list of characters in current word is equal to list of
            #characters in candidate add it to result and stop candidate loop
            if lword == lcandidate:
                result.append(candidate)
                break
    # for w in result:
    #     print(w)
    print(','.join(result))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
