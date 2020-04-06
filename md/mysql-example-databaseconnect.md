---
title: mysql example databaseconnect (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'databaseconnect'

Functions in program: 
* `def describe_table(cur, table_name):`
* `def clear_table(table_name):`
* `def learn_question_response(H):`
* `def add_learnt_statement_to_database(subject, root, verb):`
* `def get_question_response(subject, root, verb):`
* `def get_chat_response():`
* `def add_to_database(classification, subject, root, verb, H):`
* `def setup_database():`
* `def connection_to_database():`

Modules used in program: 
* `import logger_config`
* `import logging`

## python databaseconnect

Python mysql example: databaseconnect

```python
import logging
import logger_config

log = logging.getLogger(__name__)
log.info('Entered module: %s' % __name__)


@logger_config.logger
def connection_to_database():
    import config
    import mysql.connector
    from time import sleep

    max_tries = 10
    tries = 1
    conn = None
    while tries <= max_tries:
        try:
            conn = mysql.connector.connect(
                user=config.user,
                password=config.password,
                host=config.host,
                port=config.port,
                database=config.database,
            )
            if conn.is_connected():
                # logging.debug("Connected")
                logging.debug('MySQL connected')
                break

        except mysql.connector.Error as e:
            tries += 1
            logging.debug(e, "...Retrying")
            sleep(20)
    try:
        if conn.is_connected():
            # logging.debug("Connected")
            logging.debug('MySQL connected')
            return conn
    except:
        raise Exception("DATABASE NOT CONNECTED")


@logger_config.logger
# setup database
def setup_database():
    db = connection_to_database()
    cur = db.cursor()
    cur.execute("CREATE TABLE IF NOT EXISTS chat_table(id INTEGER PRIMARY KEY AUTO_INCREMENT, root_word VARCHAR(40), subject VARCHAR(40), verb VARCHAR(40), sentence VARCHAR(200))")
    cur.execute("CREATE TABLE IF NOT EXISTS statement_table(id INTEGER PRIMARY KEY AUTO_INCREMENT, root_word VARCHAR(40), subject VARCHAR(40), verb VARCHAR(40), sentence VARCHAR(200))")
    cur.execute("CREATE TABLE IF NOT EXISTS question_table(id INTEGER PRIMARY KEY AUTO_INCREMENT, root_word VARCHAR(40), subject VARCHAR(40), verb VARCHAR(40), sentence VARCHAR(200))")
    cur.execute("CREATE TABLE IF NOT EXISTS directions_table(id INTEGER PRIMARY KEY AUTO_INCREMENT, origin_location VARCHAR(100), destination_location VARCHAR(100))")


@logger_config.logger
# add classified sentences to database
def add_to_database(classification, subject, root, verb, H):
    db = connection_to_database()
    cur = db.cursor()
    cur = db.cursor(buffered=True)
    if classification == 'C':
        cur.execute("INSERT INTO chat_table(root_word,verb,sentence) VALUES (?,?,?)", (str(root),str(verb),str(H)))
        db.commit()
    elif classification == 'Q':
        cur.execute("SELECT sentence FROM question_table")
        res = cur.fetchall()
        exist = 0
        for r in res:
            if r[-1] == H:
                exist = 1
                break
        if exist == 0:
            # do not add if question already exists
            cur.execute("INSERT INTO question_table(subject,root_word,verb,sentence) VALUES (?,?,?,?)", (str(subject),str(root),str(verb),str(H)))
            db.commit()
    else:
        cur.execute("SELECT sentence FROM statement_table")
        res = cur.fetchall()
        exist = 0
        for r in res:
            if r[-1] == H:
                exist = 1
                break
        if exist == 0:    # do not add if question already exists
            cur.execute("INSERT INTO statement_table(subject,root_word,verb,sentence)  VALUES (?,?,?,?)", (str(subject),str(root),str(verb),H))
            db.commit()


@logger_config.logger
# get a random chat response
def get_chat_response():
    db = connection_to_database()
    cur = db.cursor()
    cur = db.cursor(prepared=True)
    cur.execute("SELECT COUNT(*) FROM chat_table")
    res = cur.fetchone()
    total_chat_records = res[0]
    import random
    chat_id = random.randint(1, total_chat_records+1)
    cur.execute('SELECT sentence FROM chat_table WHERE id=?', str(chat_id))
    res = cur.fetchone()
    B = res[0]
    return B


@logger_config.logger
def get_question_response(subject, root, verb):
    db = connection_to_database()
    cur = db.cursor(buffered=True)
    if str(subject) == '[]':
        cur.execute("SELECT verb FROM statement_table")
        res = cur.fetchall()
        found = 0
        for r in res:
            if r[-1] == str(verb):
                found = 1
                break
        if found == 1:
            cur.execute('SELECT sentence FROM statement_table WHERE verb=?', str(verb))
            res = cur.fetchone()
            B = res[0]
            return B, 0
        else:
            B = "Sorry I don't know the response to this. Please train me."
            return B, 1
    else:
        cur.execute("SELECT subject FROM statement_table")
        res = cur.fetchall()
        found = 0
        for r in res:
            if r[-1] == str(subject[0]):
                found = 1
                break
        if found == 1:
            cur.execute('SELECT verb FROM statement_table WHERE subject= ?', (subject[0]))
            res = cur.fetchone()
            checkVerb = res[0]   # checkVerb is a string while verb is a list. checkVerb ['verb']
            if checkVerb == '[]':
                cur.execute('SELECT sentence FROM statement_table WHERE subject= ?', (subject[0]))
                res = cur.fetchone()
                B = res[0]
                return B, 0
            else:
                if checkVerb[2:-2] == verb[0]:
                    cur.execute('SELECT sentence FROM statement_table WHERE subject=?', (subject[0]))
                    res = cur.fetchone()
                    B = res[0]
                    return B, 0
                else:
                    B = "Sorry I don't know the response to this. Please train me."
                    return B, 1
        else:
            B = "Sorry I don't know the response to this. Please train me."
            return B, 1


@logger_config.logger
def add_learnt_statement_to_database(subject, root, verb):
    db = connection_to_database()
    cur = db.cursor()
    cur.execute('INSERT INTO statement_table(subject,root_word,verb) VALUES (?,?,?)', (str(subject),str(root),str(verb)))
    db.commit()


@logger_config.logger
def learn_question_response(H):
    db = connection_to_database()
    cur = db.cursor(buffered=True)
    cur.execute("SELECT id FROM statement_table ORDER BY id DESC")
    res = cur.fetchone()
    last_id = res[0]
    cur.execute("UPDATE statement_table SET sentence = '{}' WHERE id = '{}'".format(H, last_id))
    db.commit()
    B = "Thank you! I have learnt this."
    return B, 0

@logger_config.logger
def clear_table(table_name):
    db = connection_to_database()
    cur = db.cursor()

    if table_name in ("question_table", "statement_table"):
        tables_to_be_cleaned = ("question_table", "statement_table")
        logging.debug("The following tables will be cleaned:\n")
        for table in tables_to_be_cleaned:
            describe_table(cur, table)

        if input("Enter 'Y' to confirm cleaning of BOTH tables: ") in ("Y", "y"):
            for table in tables_to_be_cleaned:
                cur.execute('DELETE FROM ?',(table))
            db.commit()
            logging.debug("Tables cleaned successfully")
        else:
            logging.debug("Table cleaning skipped.")

    else:
        logging.debug("The following table will be cleaned:\n")
        describe_table(cur, table_name)

        if input("Enter 'Y' to confirm: ") in ("Y", "y"):
            logging.debug("Table cleaned successfully")
            cur.execute('DELETE FROM ?',(table_name))
            db.commit()
        else:
            logging.debug("Table cleaning skipped.")

@logger_config.logger
def describe_table(cur, table_name):
    cur.execute('DESC ?',(table_name))
    res = cur.fetchall()
    column_names = [col[0] for col in res]

    cur.execute('SELECT COUNT(*) FROM ?',(table_name))
    res = cur.fetchall()
    records_no = res[0][0]

    logging.debug("Table Name:", table_name)
    logging.debug("Columns:", column_names)
    logging.debug("Number of existing records:", records_no)
    logging.debug()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
