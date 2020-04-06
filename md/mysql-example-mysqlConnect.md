---
title: mysql example mysqlConnect (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysqlConnect'


Modules used in program: 
* `import mysql.connector`

## python mysqlConnect

Python mysql example: mysqlConnect

```python
from __future__ import print_function
from datetime import datetime, timedelta
import mysql.connector
conn = mysql.connector.connect(user='root', password='123456', host='10.21.0.183', port=3306, buffered=True)

cursor1 = conn.cursor(buffered=True)
query1 = ("SELECT count(*) from finance.f_account a where LOCKED='0' and a.ACCT_ID NOT IN (SELECT DISTINCT b.ACCT_ID from finance.f_acct_book_exp b)")
cursor1.execute(query1)
for count1 in cursor1:
    print(count1[0])
    while count1[0] > 0:
        expId = ("SELECT max(exp_id) from finance.f_acct_book_exp")
        acctId = ("SELECT ACCT_ID from finance.f_account a where LOCKED='0' and a.ACCT_ID NOT IN (SELECT DISTINCT b.ACCT_ID from finance.f_acct_book_exp b)")
        logId = ("SELECT max(id) from finance.f_b_balance_log")
        cursor2 = conn.cursor(buffered=True)
        cursor2.execute(expId)
        cursor3 = conn.cursor(buffered=True)
        cursor3.execute(acctId)
        cursor4 = conn.cursor(buffered=True)
        cursor4.execute(logId)
        beginTime = datetime.now().date()
        endTime = datetime.now().date() + timedelta(days=4)
        updateTime = datetime.now()
        for expId1 in cursor2:
            for acctId1 in cursor3:
                for logId1 in cursor4:
                    print(expId1[0])

                    addAcctBook = ("INSERT INTO finance.f_acct_book_exp VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s)")
                    data_accBookExp = (expId1[0]+1, acctId1[0], beginTime, endTime, 1000000, updateTime, '1', '1', '1')
                    cursor3.execute(addAcctBook, data_accBookExp)

                    print("===============")

                    addBalanceLog = ("INSERT INTO finance.f_b_balance_log VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s)")
                    data_balanceLog = (logId1[0]+1, acctId1[0], None, None, 1000000, 0, updateTime, '6', '0', '0', None, '转入-体验金发放')
                    cursor4.execute(addBalanceLog, data_balanceLog)

                    print("+++++++++++++++")
                    conn.commit()
            cursor4.close()
            cursor3.close()
            cursor2.close()
cursor1.close()
conn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
