---
title: mysql example mysql isolation test (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql isolation test'

Functions in program: 
* `def print_count(cur):`
* `def print_users(cur):`
* `def run_transactions(*queries):`
* `def set_isolation_level(level):`
* `def setup():`
* `def get_connection():`

Modules used in program: 
* `import mysql.connector`

## python mysql isolation test

Python mysql example: mysql isolation test

```python
import mysql.connector


def get_connection():
    return mysql.connector.connect(user='USERNAME', password='PASSWORD', database='DATABASE', host='HOST')

def setup():
    con = get_connection()
    cursor = con.cursor()
    cursor.execute("DROP TABLE IF EXISTS users")
    cursor.execute(
        "CREATE TABLE IF NOT EXISTS users( \
        id INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY, \
        name VARCHAR(200) NOT NULL, \
        age INT(11) NOT NULL \
        ) ENGINE=INNODB"
    )
    cursor.execute("SET GLOBAL AUTOCOMMIT=0")
    for val in cursor:
        print(val)
    cursor.close()
    con.close()

def set_isolation_level(level):
    con = get_connection()
    cursor = con.cursor()
    cursor.execute("SET GLOBAL TRANSACTION ISOLATION LEVEL " + level)
    cursor.close()
    con.close()

def run_transactions(*queries):
    transactions = {}

    setup()

    for transaction_num, query, action in queries:
        if not transaction_num in transactions:
            con = get_connection()
            cur = con.cursor()
            transactions[transaction_num] = (con, cur)

    for transaction_num, query, action in queries:
        cursor = transactions[transaction_num][1]

        print((' ' * 4) + '[TRANSACTION ' + str(transaction_num) + ']: ' + query)

        try:
            cursor.execute(query)

            if action:
                action(cursor)
        except:
            print((' ' * 4) + '[ERROR] concurrent transaction could not be executed...')

    for key in transactions:
        con, cur = transactions[key]
        cur.close()
        con.close()

def print_users(cur):
    for (id, name, age) in cur:
        print((' ' * 8) + '(%s, %s, %s)' % (id, name, age))

def print_count(cur):
    for count in cur:
        print((' ' * 8) + 'Count: %s' % count)

for level in ("READ UNCOMMITTED", "READ COMMITTED", "REPEATABLE READ", "SERIALIZABLE"):
    print('\n\nIsolation level: %s' % level)
    set_isolation_level(level)

    print('dirty read')
    run_transactions((1, "INSERT INTO users(name, age) VALUES('franz', 20)", None),
                     (1, "START TRANSACTION", None),
                     (1, "SELECT * FROM users WHERE name = 'franz'", print_users),
                     (2, "START TRANSACTION", None),
                     (2, "UPDATE users SET age=30 WHERE name='franz'", None),
                     (1, "SELECT * FROM users WHERE name = 'franz'", print_users),
                     (2, "ROLLBACK", None),
                     (2, "SELECT * FROM users WHERE name = 'franz'", print_users),)

    print('\nnon-repeatable read')
    run_transactions((1, "INSERT INTO users(name, age) VALUES('franz', 20)", None),
                      (1, "START TRANSACTION", None),
                      (1, "SELECT * FROM users WHERE name = 'franz'", print_users),
                      (2, "START TRANSACTION", None),
                      (2, "UPDATE users SET age=30 WHERE name='franz'", None),
                      (2, "COMMIT", None),
                      (1, "SELECT * FROM users WHERE name = 'franz'", print_users),)

    print('\nphantom read')
    run_transactions((1, "INSERT INTO users(name, age) VALUES('franz', 20)", None),
                     (1, "START TRANSACTION", None),
                     (1, "SELECT COUNT(*) FROM users", print_count),
                     (2, "START TRANSACTION", None),
                     (2, "INSERT INTO users(name, age) VALUES('fritz', 20)", None),
                     (2, "COMMIT", None),
                     (1, "SELECT COUNT(*) FROM users", print_count),)

    print('\nlost update')
    run_transactions((1, "INSERT INTO users(name, age) VALUES('franz', 20)", None),
                     (1, "START TRANSACTION", None),
                     (1, "SELECT * FROM users WHERE name = 'franz'", print_users),
                     (2, "START TRANSACTION", None),
                     (2, "UPDATE users SET age=30 WHERE name='franz'", None),
                     (2, "COMMIT", None),
                     (1, "UPDATE users SET age=21 WHERE name='franz'", None),
                     (1, "COMMIT", None),
                     (2, "SELECT * FROM users WHERE name = 'franz'", print_users),)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
