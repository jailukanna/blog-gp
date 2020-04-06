---
title: mysql example wp hops (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'wp hops'

Functions in program: 
* `def wp_hops(c, w_from, w_to):`
* `def get_linkfrom(c, w):`
* `def get_title(c, pid):`
* `def get_pageid(c, w):`

Modules used in program: 
* `import mysql.connector`
* `import getpass`
* `import sys`

## python wp hops

Python mysql example: wp hops

```python
import sys
import getpass
import mysql.connector

class WordNotFoundError(Exception):
    def __init__(self, word):
        self.word = word
    def __str__(self):
        return self.word + " was not found."


class PageIdNotFoundError(Exception):
    def __init__(self, pid):
        self.pid = pid
    def __str(self):
        return self.pid + " was not found."


class LinkNotFoundError(Exception):
    def __init__(self, msg):
        self.msg = msg
    def __str__(self):
        return self.msg


def get_pageid(c, w):
    c.execute("SELECT page_id FROM page WHERE page_namespace=0 AND page_title=%s", (w,))
    result = c.fetchone()
    if result is None:
        raise WordNotFoundError(w)
    return result[0]


def get_title(c, pid):
    c.execute("SELECT page_title FROM page WHERE page_id=%s", (pid,))
    result = c.fetchone()
    if result is None:
        raise PageIdNotFoundError(pid)
    return result[0].decode("utf-8")


def get_linkfrom(c, w):
    c.execute("SELECT pl_from FROM pagelinks WHERE pl_from_namespace=0 AND pl_namespace=0 AND pl_title=%s", (w,))
    result = c.fetchall()
    if result:
        return [t[0] for t in result]
    else:
        return []


def wp_hops(c, w_from, w_to):
    # Raise WordNotFoundError if input w_from, w_to are not in Wikipedia.
    get_pageid(c, w_to)
    get_pageid(c, w_from)

    title_list = [w_to]
    links = {}
    n_link = 0  # for debug purpose.

    while 1:
        next_title_list = []
        for title in title_list:
            print(n_link, title)
            c.execute("""SELECT page_title FROM page JOIN pagelinks ON page.page_id=pagelinks.pl_from
                             WHERE pagelinks.pl_from_namespace=0 AND
                                   pagelinks.pl_namespace=0 AND
                                   pagelinks.pl_title=%s""", (title,))
            from_titles = [a[0].decode("utf-8") for a in c.fetchall()]

            for ft in from_titles:
                if ft == w_from:
                    result = [w_from, title]
                    t = title
                    while t != w_to:
                        t = links[t]
                        result.append(t)
                    return result
                if ft not in links:
                    links[ft] = title
                    next_title_list.append(ft)
        title_list = next_title_list
        n_link += 1


if __name__ == "__main__":
    if len(sys.argv) == 4:
        user = sys.argv[1]
        pw = getpass.getpass()
        w_from = sys.argv[2]
        w_to = sys.argv[3]
    elif len(sys.argv) == 5:
        user = sys.argv[1]
        pw = sys.argv[2]
        w_from = sys.argv[3]
        w_to = sys.argv[4]
    conn = mysql.connector.Connect(user=user, password=pw, db="jawiki", charset="utf8")
    c = conn.cursor()
    print(wp_hops(c, w_from, w_to))
    c.close()
    conn.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
