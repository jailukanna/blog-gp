---
title: mysql example register of mergers (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'register of mergers'

Functions in program: 
* `def main():`
* `def extract_mergers( book ):`
* `def extract_charity_number( str ):`
* `def get_date( value, mode ):`

Modules used in program: 
* `import urllib2`
* `import mysql_config as msc`
* `import MySQLdb as mdb`
* `import argparse`
* `import re`
* `import datetime, time`
* `import xlrd`

## python register of mergers

Python mysql example: register of mergers

```python
import xlrd
import datetime, time
import re
import argparse
import MySQLdb as mdb
import mysql_config as msc
import urllib2

def get_date( value, mode ):
    try:
        return datetime.date(*xlrd.xldate_as_tuple( value, mode)[:3]).isoformat()
    except ValueError:
        return ''

def extract_charity_number( str ):
    ccnum_matchs = re.finditer('\(([0-9]{6,7})([\-\/]([0-9]+))?\)', str)
    if ccnum_matchs:
        ccnum_returns = []
        for ccnum_match in ccnum_matchs:
            ccnum_match = ccnum_match.groups()
            ccnum_return = {"regno":ccnum_match[0], "subno":"0"}
            if ccnum_match[2]:
                ccnum_return["subno"] = ccnum_match[2]
            ccnum_returns.append(ccnum_return)
        return ccnum_returns
        
def extract_mergers( book ):
    rom = book.sheet_by_index(0)
    mergers = []

    for row_index in range(rom.nrows):
    #for row_index in range(3):
        if row_index > 0:
            transferors = rom.cell(row_index,0).value
            transferors = transferors.replace('   ', '\n').replace('; ', '\n').splitlines()
            for transferor in transferors:
                transferor = transferor.strip()
                if(len(transferor)>0):
                    merger = {
                        "transferor": transferor,
                        "transferee": rom.cell(row_index,1).value,
                        "vesting-declaration-date": get_date(rom.cell(row_index,3).value, book.datemode),
                        "property-transferred-date": get_date(rom.cell(row_index,4).value, book.datemode),
                        "merger-registered-date": get_date(rom.cell(row_index,3).value, book.datemode)
                    }
                    merger["transferor_regnos"] = extract_charity_number( merger["transferor"] )
                    merger["transferee_regno"] = extract_charity_number( merger["transferee"] )
                    if(len(merger["transferee_regno"])==1):
                        merger["transferee_regno"] = merger["transferee_regno"][0]
                    else:
                        merger["transferee_regno"] = {"regno":"", "subno":""}
                        print("No transferee regno found", merger["transferee"])
                    mergers.append(merger)
    return mergers

def main():

    parser = argparse.ArgumentParser(description='Import the Charity Commission register of mergers')
    parser.add_argument("-f", "--file", default="https://www.gov.uk/government/uploads/system/uploads/attachment_data/file/369817/Rom.xls", help='The register of mergers file')
    parser.add_argument("--test", dest='test', action='store_true', help='Whether to run in test mode (only processes first 10 rows, no rows uploaded to database)')
    parser.set_defaults(feature=True)
    args = parser.parse_args()
    
    con = mdb.connect( msc.host, msc.user, msc.password, 'almanac')
    cur = con.cursor(mdb.cursors.DictCursor)
    
    if(args.file[0:4]=="http"):
        response = urllib2.urlopen( args.file )
        file_contents = response.read()
        book = xlrd.open_workbook( file_contents=file_contents )
    else:
        book = xlrd.open_workbook( args.file )
    mergers = extract_mergers( book )
    

    # The SQL query that fetches the rows we're looking for from the database.
    # excludes any rows that have already been checked
    add_sql = """REPLACE INTO `cc_register_of_mergers` (`date_scraped`, `transferee_no`, `transferee_regno`, `transferee_subno`, `vesting_date`, `transferor_no`, `transferor_regno`, `transferor_subno`, `merger_date`, `transferee`, `transferor`, `transfer_date`) VALUES ( %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s )"""
    add_sql_array = []
    
    if(args.test):
        del mergers[10:] # remove all but first 10 mergers
    
    for merger in mergers:
    
        if(len(merger["transferor_regnos"])==0):
            merger["transferor_regnos"] = [{"regno":"", "subno":""}]
        
        for regno in merger["transferor_regnos"]:
            if(merger["transferee_regno"]["subno"]=="0"):
                transferee_no = merger["transferee_regno"]["regno"]
            else:
                transferee_no = merger["transferee_regno"]["regno"] + "-" + merger["transferee_regno"]["subno"]
            if(regno["subno"]=="0"):
                transferor_no = regno["regno"]
            else:
                transferor_no = regno["regno"] + "-" + regno["subno"]
            this_merger = ( time.strftime('%Y-%m-%d %H:%M:%S'), transferee_no, merger["transferee_regno"]["regno"], merger["transferee_regno"]["subno"], merger["vesting-declaration-date"], transferor_no, regno["regno"], regno["subno"], merger["merger-registered-date"], merger["transferee"], merger["transferor"], merger["property-transferred-date"] )
            if(args.test):
                print(this_merger)
            add_sql_array.append( this_merger )
    
    cur.execute( "TRUNCATE `cc_register_of_mergers`")
    cur.executemany( add_sql, add_sql_array )
    print(len(add_sql_array), " mergers added to database")


if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
