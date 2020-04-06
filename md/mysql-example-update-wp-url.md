---
title: mysql example update-wp-url (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'update-wp-url'


Modules used in program: 
* `import mysql.connector`
* `import sys`

## python update-wp-url

Python mysql example: update-wp-url

```python
#!/usr/bin/python3

import sys
import mysql.connector


if __name__ == '__main__':
  if len(sys.argv) < 5:
    print('Wrong number of arguments!\nUse update-wp-url.py <database> <user> <pass> <url> [table prefix]')
    sys.exit()
  
  # read user input
  db_name = sys.argv[1]
  db_user = sys.argv[2]
  db_pass = sys.argv[3]
  new_url = sys.argv[4]
  prefixed = sys.argv[5] + "options" if len(sys.argv) == 6 else "wp_options"
  
  try:
    # connect
    conn = mysql.connector.connect(user=db_user,
                                   password=db_pass,
                                   host='127.0.0.1',
                                   database=db_name)
    cursor = conn.cursor()

    # update values
    update_query = "UPDATE " + prefixed + " SET option_value = %s WHERE option_name = 'siteurl' OR option_name = 'home'"
    cursor.execute(update_query, (new_url,))
    
    # show confirmation
    print('URL Changed')

    conn.commit()
    cursor.close()
    conn.close()
  except mysql.connector.Error as err:
    print('Error:', err)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
