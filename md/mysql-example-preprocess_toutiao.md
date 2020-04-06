---
title: mysql example preprocess toutiao (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'preprocess toutiao'

Functions in program: 
* `def write_prepared_table(max_seq_length):`
* `def load_vocab(file_path):`
* `def prepare_vocab():`

Modules used in program: 
* `import jieba`
* `import mysql.connector`
* `import codecs`
* `import sys`

## python preprocess toutiao

Python mysql example: preprocess toutiao

```python
# -*- coding: utf-8 -*-
# encoding=utf-8

# REQUIREMENTS:
# pip install jieba mysql-connector

# Download the dataset from https://github.com/fate233/toutiao-text-classfication-dataset

import sys
import codecs
import mysql.connector
import jieba

stdout = codecs.getwriter('utf-8')(sys.stdout)
def prepare_vocab():
    mydb = mysql.connector.connect(
    host="127.0.0.1",
    user="root",
    passwd="root",
    database="toutiao",
    use_unicode=True,
    charset="utf8",
    )

    mycursor = mydb.cursor()

    mycursor.execute("SELECT * FROM train")

    voc = dict()
    voc["<unk>"] = 0
    wordid = 0
    max_seq_length = 0

    while True:
        x = mycursor.fetchone()
        if x is None:
            break

        seg_list = jieba.cut(x[3])
        seg_len = 0
        
        for w in seg_list:
            if not w in voc:
                wordid += 1
                voc[w] = wordid
            
            seg_len += 1
        if seg_len > max_seq_length:
            max_seq_length = seg_len
    mycursor.reset()

    print("total vocab size: ", len(voc), " max seq length: ", max_seq_length)
    with open("vocab.txt", "w", encoding="utf-8") as fn:
        for w in voc:
            fn.write("%s\t%d\n" % (w, voc[w]))


def load_vocab(file_path):
    voc = dict()
    with open(file_path, "r") as fn:
        for l in fn:
            l_strip = l[0:-1]
            try:
                w, idx = l_strip.split("\t")
                voc[w] = idx
            except:
                print("skip vocab: ", l)
    return voc
            

def write_prepared_table(max_seq_length):
    # write to table train_processed, string fields should be encoded like:
    # "9,100,33,21,0,0,0,0" padding to max seq length with 0
    voc = load_vocab("vocab.txt")

    conn_write = mysql.connector.connect(
    host="127.0.0.1",
    user="root",
    passwd="root",
    database="toutiao",
    use_unicode=True,
    charset="utf8",
    )
    conn_read = mysql.connector.connect(
    host="127.0.0.1",
    user="root",
    passwd="root",
    database="toutiao",
    use_unicode=True,
    charset="utf8",
    )

    mycursor = conn_read.cursor()
    mycursor.execute("SELECT * FROM train")
    write_cursor = conn_write.cursor()
    while True:
        x = mycursor.fetchone()
        if x is None:
            break
        title_tensor = []
        title_seg = jieba.cut(x[3])
        for w in title_seg:
            if w in voc:
                title_tensor.append(str(voc[w]))
            else:
                title_tensor.append("0")
        # padding
        while len(title_tensor) < max_seq_length:
            title_tensor.append("0")
        
        # NOTE: update class_id = class_id - 100 to let it start from 0
        sql = """INSERT INTO train_processed (id, class_id, class_name, news_title, news_keywords) 
VALUES (%d, %d, "%s", "%s", "%s")""" % (x[0], x[1] - 100, x[2], ",".join(title_tensor), x[4])
        write_cursor.execute(sql)

    conn_write.commit()

    mycursor.reset()
    conn_read.close()

if __name__ == "__main__":
    prepare_vocab()
    # NOTE: 92 is max_seq_length, can run the above line to get this number.
    write_prepared_table(92)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
