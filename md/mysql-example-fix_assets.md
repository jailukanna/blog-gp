---
title: mysql example fix assets (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'fix assets'


Modules used in program: 
* `import mysql.connector`
* `import sys`
* `import getpass`
* `import argparse`

## python fix assets

Python mysql example: fix assets

```python
import argparse
import getpass
import sys

import mysql.connector


class AssetsFixer:

    cols = ["id", "parent_id", "level", "lft", "rgt"]

    def __init__(self, user, password, db, prefix, host="localhost"):
        self.db = mysql.connector.connect(user=user, password=password, db=db, host=host)
        self.prefix = prefix

    def get_all(self, query, *params):
        c = self.db.cursor()
        c.execute(query, params)
        return c.fetchall()

    def collect_info(self):
        by_id = self.by_id = {}
        by_parent_id = self.by_parent_id = {}
        query = "select %s from %sassets" % (", ".join(self.cols), self.prefix)
        for values in self.get_all(query):
            row = dict(zip(self.cols, values))
            by_id[row["id"]] = row
            siblings = by_parent_id.setdefault(row["parent_id"], [])
            siblings.append(row["id"])

    def fix_data(self, parent_id, level, lft):
        """depth first descent
        """
        child_lft = lft
        children = self.by_parent_id.get(parent_id, [])
        # sort on actual lft
        children.sort(key=lambda i: self.by_id[i]['lft'])
        for child_id in children:
            child_lft = self.fix_data(child_id, level + 1, child_lft + 1)
        # fix row
        data = self.by_id[parent_id]
        data["level"] = level
        data["lft"] = lft
        data["rgt"] = child_lft + 1
        return data["rgt"]

    def update_db(self):
        c = self.db.cursor()
        update_cols = list(self.cols)
        update_cols.remove("id")
        update_expr = ", ".join("{col}=%({col})s".format(col=col) for col in update_cols)
        update_query = "UPDATE {prefix}assets SET {update_expr} WHERE id=%(id)s".format(
            prefix=self.prefix, update_expr=update_expr)
        c.executemany(update_query, list(self.by_id.values()))
        self.db.commit()

    def __call__(self):
        print("Collecting info")
        self.collect_info()
        print("Fixing")
        roots = self.by_parent_id[0]
        assert len(roots) == 1, "More than one root !"
        root = roots[0]
        self.fix_data(root, level=0, lft=1)
        print("Updating db")
        self.update_db()

if __name__ == "__main__":
    parser = argparse.ArgumentParser(
        description=("Utility to fixe a not so broken joomla assets table. " +
                     "Provided parent is correct"))
    parser.add_argument("-u", "--username", required=True)
    parser.add_argument("-l", "--hostname", default="localhost")
    parser.add_argument("database", nargs=1)
    parser.add_argument("prefix", nargs=1, help="tables prefix")
    opts = parser.parse_args()
    answer = input(
        "Note that this is a potentially dangerous operation, " +
        "did you backup you database first ? (yes/no):")
    if answer == "yes":
        password = getpass.getpass()
        af = AssetsFixer(
            db=opts.database[0], prefix=opts.prefix[0], user=opts.username, password=password)
        af()
    else:
        print("So backup first")
        sys.exit(1)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
