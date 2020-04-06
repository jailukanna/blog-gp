---
title: mysql example secure-mysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'secure-mysql'

Functions in program: 
* `def main():`
* `def sql_anweisung(cursor, module, action, description):`

Modules used in program: 
* `import os`
* `import datetime`

## python secure-mysql

Python mysql example: secure-mysql

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

import datetime
import os
from ansible.module_utils.basic import AnsibleModule
from ansible.module_utils.mysql import mysql_driver, mysql_driver_fail_msg

def sql_anweisung(cursor, module, action, description):
    try:
        cursor.execute(action)
    except Exception as e:
        failmsg = 'Fehler beim ' + description + ': ' + e.args[1]
        module.fail_json(msg=failmsg)
    return True

def main():
    args = {'rootpw': {'required': True, 'type': 'str'}}
    module = AnsibleModule(argument_spec=args, supports_check_mode=True)

    if mysql_driver is None:
        module.fail_json(msg=mysql_driver_fail_msg)

    config_file = '/root/.my.cnf'
    try:
        if os.path.exists(config_file):
            mydb = mysql_driver.connect(read_default_file=config_file)
        else:
            mydb = mysql_driver.connect()
    except Exception:
        module.fail_json(msg='Verbindung zum Datenbank-Server nicht möglich!')

    cursor = mydb.cursor()

    if module.check_mode:
        module.exit_json(changed=True)

    startd = datetime.datetime.now()

    action = "UPDATE mysql.user SET Password=PASSWORD('%s') WHERE User='root';" % module.params['rootpw']
    sql_anweisung(cursor, module, action, 'Setzen des Root-Kennworts')

    action = "DELETE FROM mysql.user WHERE User='root' AND Host NOT IN ('localhost', '127.0.0.1', '::1');"
    sql_anweisung(cursor, module, action, 'Ändern der Zugänge für Root')

    action = "DELETE FROM mysql.user WHERE User='';"    
    sql_anweisung(cursor, module, action, 'Löschen des anonymen Users')

    action = 'DROP DATABASE IF EXISTS test;'
    sql_anweisung(cursor, module, action, 'Löschen der Test-Datenbank')

    action = 'FLUSH PRIVILEGES;'
    changed = sql_anweisung(cursor, module, action, 'Modulabschluss')

    endd = datetime.datetime.now()
    delta = endd - startd

    result = {
        'changed': changed,
        'start': str(startd),
        'end': str(endd),
        'delta': str(delta),
        'rootpw': module.params['rootpw'],
        'message': 'Have a nice day!'
    }

    module.exit_json(**result)

if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
