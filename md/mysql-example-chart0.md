---
title: mysql example chart0 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'chart0'

Functions in program: 
* `def plot(d: List[Tuple[datetime.timedelta, int]]):`
* `def data() -> List[Tuple[datetime.timedelta, int]]:`
* `def main():`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import datetime`
* `import mysql.connector`

## python chart0

Python mysql example: chart0

```python
# dataset: https://ergast.com/mrd/db/

from typing import List, Tuple
import mysql.connector
import datetime
import matplotlib.pyplot as plt
import numpy as np
from collections import Counter


def main():
    plot(data())


def data() -> List[Tuple[datetime.timedelta, int]]:
    try:
        db = mysql.connector.connect(user='root', password='root', database='f1')
        cursor = db.cursor()

        query = """
                SELECT drivers.dob, races.date, results.points
                FROM results
                INNER JOIN drivers ON drivers.driverId = results.driverId
                INNER JOIN races ON races.raceId = results.raceId
            """
        cursor.execute(query)

        result: List[Tuple[datetime.timedelta, int]] = []
        for (dob, raceDate, points) in cursor:
            age = raceDate.year - dob.year - ((raceDate.month, raceDate.day) < (dob.month, dob.day))
            result.append((age, points))

        cursor.close()
        return result

    finally:
        db.close()


def plot(d: List[Tuple[datetime.timedelta, int]]):
    plt.figure(figsize=(15, 15))
    plt.rc('font', **{'size': 30})
    plt.title('Age of driver vs points scored')
    plt.xlabel('Age')
    plt.ylabel('Points scored')

    xy = np.array(d)
    c = Counter(d)
    weights = np.ndarray(xy.shape[0])

    for i in range(0, xy.shape[0]):
        weights[i] = c[tuple(xy[i])]

    plt.scatter(xy.T[0], xy.T[1], s=weights ** 0.9 + 20)
    plt.show()


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
