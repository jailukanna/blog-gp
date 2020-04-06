---
title: mysql example imdb to database (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'imdb to database'

Functions in program: 
* `def load_imdb_to_database():`
* `def prepare_tables():`
* `def get_db_connect(database="imdb"):`

Modules used in program: 
* `import mysql.connector`
* `import numpy as np`
* `import tensorflow as tf`

## python imdb to database

Python mysql example: imdb to database

```python
import tensorflow as tf
from tensorflow import keras
import numpy as np
import mysql.connector

db_conn = None

def get_db_connect(database="imdb"):
    global db_conn
    if not db_conn:
        db_conn = mysql.connector.connect(
            host="127.0.0.1",
            user="root",
            passwd="root",
            database="imdb",
            use_unicode=True,
            charset="utf8",
        )
    mycursor = db_conn.cursor()
    return db_conn, mycursor

def prepare_tables():
    mydb, mycursor = get_db_connect()
    mycursor.execute("""CREATE TABLE imdb.train (
content TEXT NOT NULL,
class int(2) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci""")
    mycursor.execute("""CREATE TABLE imdb.test (
content TEXT NOT NULL,
class int(2) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci""")
    mydb.commit()
    mycursor.close()

def load_imdb_to_database():
    imdb = keras.datasets.imdb
    (train_data, train_labels), (test_data, test_labels) = imdb.load_data(num_words=10000)
    # A dictionary mapping words to an integer index
    word_index = imdb.get_word_index()

    # The first indices are reserved
    word_index = {k:(v+3) for k,v in word_index.items()}
    word_index["<PAD>"] = 0
    word_index["<START>"] = 1
    word_index["<UNK>"] = 2  # unknown
    word_index["<UNUSED>"] = 3
    train_data = keras.preprocessing.sequence.pad_sequences(train_data,
                                                            value=word_index["<PAD>"],
                                                            padding='post',
                                                            maxlen=256)
    test_data = keras.preprocessing.sequence.pad_sequences(test_data,
                                                        value=word_index["<PAD>"],
                                                        padding='post',
                                                        maxlen=256)
    conn, cursor = get_db_connect()
    idx = 0
    for sample in train_data:
        label = train_labels[idx]
        cursor.execute("INSERT INTO train (content, class) VALUES ('%s', %d)" % (",".join([str(x) for x in sample]), label))
        idx += 1
    # TODO(typhoonzero): insert test data also adding a column `sqlflow_is_train` to
    # indicate whether is train data or test data.
    idx = 0
    for sample in test_data:
        label = test_labels[idx]
        cursor.execute("INSERT INTO test (content, class) VALUES ('%s', %d)" % (",".join([str(x) for x in sample]), label))
        idx += 1

    conn.commit()
    cursor.close()

if __name__ == "__main__":    
    prepare_tables()  # should run only once
    load_imdb_to_database()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
