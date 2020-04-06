---
title: mysql example preprocess imdb (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'preprocess imdb'

Functions in program: 
* `def write_prepared_table(max_seq_length):`
* `def load_vocab(vocabfile="aclImdb/imdb.vocab"):`
* `def inject_imdb_to_db():`
* `def escape_single_quote(str):`
* `def prepare_tables():`

Modules used in program: 
* `import mysql.connector`
* `import codecs`
* `import sys, os`

## python preprocess imdb

Python mysql example: preprocess imdb

```python
# -*- coding: utf-8 -*-
# encoding=utf-8

# REQUIREMENTS:
# pip install mysql-connector
import sys, os
import codecs
import mysql.connector

def prepare_tables():
    mydb = mysql.connector.connect(
        host="127.0.0.1",
        user="root",
        passwd="root",
        database="imdb",
        use_unicode=True,
        charset="utf8",
    )
    mycursor = mydb.cursor()
    mycursor.execute("""CREATE TABLE imdb.train (
content TEXT NOT NULL,
class int(2) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci""")
    mycursor.execute("""CREATE TABLE imdb.train_processed (
content TEXT NOT NULL,
class int(2) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci""")
    mydb.commit()

def escape_single_quote(str):
    return str.replace("'", "\\\'")

def inject_imdb_to_db():
    mydb = mysql.connector.connect(
        host="127.0.0.1",
        user="root",
        passwd="root",
        database="imdb",
        use_unicode=True,
        charset="utf8",
    )
    mycursor = mydb.cursor()

    train_max_seq_length = 0
    for filename in os.listdir("aclImdb/train/pos"):
        with open(os.path.join("aclImdb/train/pos", filename)) as fn:
            content = escape_single_quote(fn.read())
            curr_len = len(content.split(" "))
            if curr_len > train_max_seq_length:
                train_max_seq_length = curr_len
            insert = "insert into imdb.train (content, class) values ('%s', %d)" % (content, 0)
            mycursor.execute(insert)
    for filename in os.listdir("aclImdb/train/neg"):
        with open(os.path.join("aclImdb/train/neg", filename)) as fn:
            content = escape_single_quote(fn.read())
            insert = "insert into imdb.train (content, class) values ('%s', %d)" % (content, 1)
            mycursor.execute(insert)
    mydb.commit()
    mycursor.close()
    print("train max seq len: %d" % train_max_seq_length)

def load_vocab(vocabfile="aclImdb/imdb.vocab"):
    word_idx = 1
    vocab = dict()
    vocab["<unk>"] = 0
    with open(vocabfile) as fn:
        while True:
            line = fn.readline()
            if not line:
                break
            word = line[:-1]
            vocab[word] = word_idx
            word_idx += 1
    return vocab

def write_prepared_table(max_seq_length):
    # write to table train_processed, string fields should be encoded like:
    # "9,100,33,21,0,0,0,0" padding to max seq length with 0
    vocab = load_vocab("aclImdb/imdb.vocab")
    conn_write = mysql.connector.connect(
        host="127.0.0.1",
        user="root",
        passwd="root",
        database="imdb",
        use_unicode=True,
        charset="utf8",
    )
    conn_read = mysql.connector.connect(
        host="127.0.0.1",
        user="root",
        passwd="root",
        database="imdb",
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
        content_tensor = []
        content_seg = x[0].split(" ")
        for w in content_seg:
            if w in vocab:
                content_tensor.append(str(vocab[w]))
            else:
                content_tensor.append("0")
        # padding
        while len(content_tensor) < max_seq_length:
            content_tensor.append("0")
        
        # NOTE: update class_id = class_id - 100 to let it start from 0
        sql = """INSERT INTO train_processed (content, class) 
VALUES ("%s", %d)""" % (",".join(content_tensor), x[1])
        write_cursor.execute(sql)

    conn_write.commit()

    mycursor.reset()
    conn_read.close()

if __name__ == "__main__":
    prepare_tables()
    inject_imdb_to_db()
    # NOTE: 2470 is max_seq_length, can run the above line to get this number.
    write_prepared_table(2470)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
