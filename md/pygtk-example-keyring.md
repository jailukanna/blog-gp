---
title: pygtk example keyring (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'keyring'

Functions in program: 
* `def main():`

Modules used in program: 
* `import gnomekeyring as gkey`
* `import gtk # ensure that the application name is correctly set`

## python keyring

Python pygtk example: keyring

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-

# Gnome Keyring in Python
# Shows how to store credentials of type NETWORK_PASSWORD using python.
# Found on <http://www.rittau.org/blog/20070726-01>
# More on the GNOME Keyring: <http://live.gnome.org/GnomeKeyring>

# alternatively have a look at the new universal and actively developed Python Keyring:
# on <http://pypi.python.org/pypi/keyring> and <http://home.python-keyring.org/>

# another small piece of nice python code: gkeyring (Can retreive all the data stored in the keyring...)
# <http://bazaar.launchpad.net/%7Ekamil.paral/gkeyring/trunk/annotate/head%3A/gkeyring.py>

import gtk # ensure that the application name is correctly set
import gnomekeyring as gkey


class Keyring(object):
    def __init__(self, name, server, protocol):
        self._name = name
        self._server = server
        self._protocol = protocol
        self._keyring = gkey.get_default_keyring_sync()

    def has_credentials(self):
        try:
            attrs = {"server": self._server, "protocol": self._protocol}
            items = gkey.find_items_sync(gkey.ITEM_NETWORK_PASSWORD, attrs)
            return len(items) > 0
        except gkey.DeniedError:
            return False
        except gkey.NoMatchError:
            return False

    def get_credentials(self):
        attrs = {"server": self._server, "protocol": self._protocol}
        items = gkey.find_items_sync(gkey.ITEM_NETWORK_PASSWORD, attrs)
        return (items[0].attributes["user"], items[0].secret)

    def set_credentials(self, (user, pw)):
        attrs = {
                "user": user,
                "server": self._server,
                "protocol": self._protocol,
            }
        gkey.item_create_sync(gkey.get_default_keyring_sync(),
                gkey.ITEM_NETWORK_PASSWORD, self._name, attrs, pw, True)

def main():
    kr = Keyring('keyring in python', 'someserver.net', 'someprotocol')
    
    if kr.has_credentials():
        print("We have credentials stored here.")
    else:
        print("We have NO credentials stored here.")
    
    print("Storing credentials.")
    kr.set_credentials(('admin','krylline'))
    
    print(kr.get_credentials())

if __name__ == "__main__":
    main()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
