---
title: mysql example Connecting-to-RDS-using-SSL-in-Python (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Connecting-to-RDS-using-SSL-in-Python'


Modules used in program: 
* `import mysql.connector`

## python Connecting-to-RDS-using-SSL-in-Python

Python mysql example: Connecting-to-RDS-using-SSL-in-Python

```python

import mysql.connector
#CA Bundle file is provided on AWS documentation for RDS: http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html
#Link: https://s3.amazonaws.com/rds-downloads/rds-combined-ca-bundle.pem
SSL_CA='rds-combined-ca-bundle.pem'
cnx = mysql.connector.connect(user='sequoia_usr_prod', password='iz8sVGk0TwYxKfJo',
                             host='sequoia-db-prod.cxtfi76qjanv.us-west-2.rds.amazonaws.com',
                             database='sequoia_db_prod', ssl_ca=SSL_CA)
cnx.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
