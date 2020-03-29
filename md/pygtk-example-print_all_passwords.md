---
title: pygtk example print all passwords (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'print all passwords'

Functions in program: 
* `def print_all():`

Modules used in program: 
* `import gnomekeyring`
* `import gtk # sets app name`
* `import pygtk`

## python print all passwords

Python pygtk example: print all passwords

```python
#!/usr/bin/env python

# found on <http://michael.susens-schurter.com/blog/2008/10/30/listing-all-passwords-stored-in-gnome-keyring/> 

import pygtk
pygtk.require('2.0')
import gtk # sets app name
import gnomekeyring
 
def print_all():
    for keyring in gnomekeyring.list_keyring_names_sync():
        try:
            for id in gnomekeyring.list_item_ids_sync(keyring):
                item = gnomekeyring.item_get_info_sync(keyring, id)
                print('[%s] %s = %s' % (keyring, item.get_display_name(), item.get_secret()))
            else:
                if len(gnomekeyring.list_item_ids_sync(keyring)) == 0:
                    print('[%s] --empty--' % keyring)
        except gnomekeyring.DeniedError:
            pass
 
if __name__ == '__main__':
    print_all()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
