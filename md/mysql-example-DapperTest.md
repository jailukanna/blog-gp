---
title: mysql example DapperTest (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'DapperTest'


Modules used in program: 
* `import App.Core.Models as Models              #get our "dumb" model objects from the projects assembly.  `
* `import MySql.Data`
* `import Dapper`
* `import os`
* `import sys`
* `import clr`
* `import System`

## python DapperTest

Python mysql example: DapperTest

```python
#DapperTest.py - Quick And dirty example of IronPython using Dapper.NET (w/MySql backend and stored procedures)
import System

import clr
#clr.AddReferenceByPartialName("System.Core")
clr.AddReferenceByPartialName("System.Data")

from System.Collections import IDictionary
from System.Collections.Generic import IDictionary, IList
from System.Data import IDbConnection, IDbCommand, CommandType

import sys
import os
sys.path.append(os.getcwd() + '\\lib')       #external dependencies are located in a 'lib' folder in same path as this script

clr.AddReferenceToFile('Dapper.dll')
import Dapper
clr.ImportExtensions(Dapper)
from Dapper import DynamicParameters, SqlMapper

clr.AddReferenceToFile('MySql.Data.dll')
import MySql.Data

clr.AddReferenceToFile('App.dll')             #custom .NET assembly used for when IronPython doesn't fit the need.
import App.Core.Models as Models              #get our "dumb" model objects from the projects assembly.  
    
cn = MySql.Data.MySqlClient.MySqlConnection("Server=localhost;Database=northwinddb;Uid=springqa;Pwd=springqa;" \
                                              "Convert Zero Datetime=true;Allow Zero Datetime=false")
cn.Open()

params = DynamicParameters()
params.Add('@in_CustomerId', 'ALFKI')
procName = 'GetCustomerById'
customer = cn.Query[Models.Customer](procName, params,commandType=CommandType.StoredProcedure)[0]
cn.Close()

print(customer.CustomerId)
print(customer.ContactName)
print(customer.ContactTitle)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
