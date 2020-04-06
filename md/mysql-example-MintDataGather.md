---
title: mysql example MintDataGather (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'MintDataGather'

Functions in program: 
* `def Gather_Mint_Information():`
* `def RunStuff():`

Modules used in program: 
* `import MysqlCommands`
* `import mintapi`

## python MintDataGather

Python mysql example: MintDataGather

```python
import mintapi
from builtins import enumerate
import MysqlCommands



def RunStuff():
    Gather_Mint_Information()



def Gather_Mint_Information():
    # email and password
    email = "mintusername"
    password = "mintpassword"

    # gather instantiate the mint api
    mint = mintapi.Mint(email, password)

    # Get basic account information
    #mint.get_accounts()
    JSONInfo = mint.get_accounts()

    # Print out the arrays for each accont for debugging
    for (i, item) in enumerate(JSONInfo):
        print(i, item)
        MysqlCommands.InsertMintData(item)

RunStuff()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
