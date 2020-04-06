---
title: mysql example scorpio config env db (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio config env db'


## python scorpio config env db

Python mysql example: scorpio config env db

```python
LOCAL = "local"
ROOT = "root"

MKX_55_IP = "192.168.130.55"
KMX_55_PAS = "kmx_55_pas"
KMX_55_AUTH = "kmx_55_auth"
PAS_DB = "pasdb"

MKX_83_IP = "192.168.130.83"
KMX_83_PAS = "kmx_83_pas"
KMX_83_AUTH = "kmx_83_auth"
CAS_DB = "cas_db"

KMX_PWD = "passw0rd"
UTF_8 = "utf8"

MYSQL_CONFIG = {
    LOCAL: {
        "host": "127.0.0.1",
        "port": 3306,
        "user": ROOT,
        "passwd": "aa123456",
        "db": "mydb",
        "charset": UTF_8,
    },
    KMX_55_PAS: {
        "host": MKX_55_IP,
        "port": 3309,
        "user": ROOT,
        "passwd": KMX_PWD,
        "db": "pasdb",
        "charset": UTF_8,
    },
    KMX_55_AUTH: {
        "host": MKX_55_IP,
        "port": 23307,
        "user": ROOT,
        "passwd": KMX_PWD,
        "db": CAS_DB,
        "charset": UTF_8,
    },
    KMX_83_PAS: {
        "host": MKX_83_IP,
        "port": 3309,
        "user": ROOT,
        "passwd": KMX_PWD,
        "db": PAS_DB,
        "charset": UTF_8,
    },
    KMX_83_AUTH: {
        "host": MKX_83_IP,
        "port": 23307,
        "user": ROOT,
        "passwd": KMX_PWD,
        "db": "cas_db",
        "charset": UTF_8,
    }
}

# ======================= 当前环境 =======================
CURRENT_ENV = KMX_55_AUTH


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
