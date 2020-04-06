---
title: mysql example chart1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'chart1'

Functions in program: 
* `def plot(d0: np.ndarray, d1: np.ndarray):`
* `def min_max_mean(races: List[Tuple[int, List[int]]]) -> np.ndarray:`
* `def group_by_year(d: List[Tuple[int, int]]) -> List[Tuple[int, List[int]]]:`
* `def race_year_and_ages_podium_only(db: mysql.connector.MySQLConnection) -> List[Tuple[int, int]]:`
* `def race_year_and_ages(db: mysql.connector.MySQLConnection) -> List[Tuple[int, int]]:`
* `def main():`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import mysql.connector`

## python chart1

Python mysql example: chart1

```python
# dataset: http://ergast.com/mrd/db/
from typing import List, Tuple
import mysql.connector
import matplotlib.pyplot as plt
import numpy as np


def main():
    try:
        db = mysql.connector.connect(user='root', password='root', database='f1')
        plot(
            min_max_mean(group_by_year(race_year_and_ages(db))),
            min_max_mean(group_by_year(race_year_and_ages_podium_only(db))),
        )
    finally:
        db.close()


# Returns list of results [[year, age]]
def race_year_and_ages(db: mysql.connector.MySQLConnection) -> List[Tuple[int, int]]:
    cursor = db.cursor()

    query = """
        SELECT races.year, races.date, drivers.dob
        FROM results
        INNER JOIN drivers ON drivers.driverId = results.driverId
        INNER JOIN races ON races.raceId = results.raceId
    """
    cursor.execute(query)

    result: List[Tuple[int, int]] = []
    for (year, raceDate, dob) in cursor:
        age = raceDate.year - dob.year - ((raceDate.month, raceDate.day) < (dob.month, dob.day))
        result.append((year, age))

    cursor.close()
    return result


def race_year_and_ages_podium_only(db: mysql.connector.MySQLConnection) -> List[Tuple[int, int]]:
    cursor = db.cursor()

    query = """
        SELECT races.year, races.date, drivers.dob
        FROM results
        INNER JOIN drivers ON drivers.driverId = results.driverId
        INNER JOIN races ON races.raceId = results.raceId
        WHERE position <= 3
    """
    cursor.execute(query)

    result: List[Tuple[int, int]] = []
    for (year, raceDate, dob) in cursor:
        age = raceDate.year - dob.year - ((raceDate.month, raceDate.day) < (dob.month, dob.day))
        result.append((year, age))

    cursor.close()
    return result


# Groups ages by year
def group_by_year(d: List[Tuple[int, int]]) -> List[Tuple[int, List[int]]]:
    races = {}
    for row in d:
        if row[0] not in races:
            races[row[0]] = []

        races[row[0]].append(row[1])

    result = [(year, ages) for year, ages in races.items()]
    return sorted(result, key=lambda x: x[0])


def min_max_mean(races: List[Tuple[int, List[int]]]) -> np.ndarray:
    result = np.ndarray([len(races), 3])
    for i, row in enumerate(races):
        result[i] = [min(row[1]), max(row[1]), np.mean(row[1])]
    return result


def plot(d0: np.ndarray, d1: np.ndarray):
    # input: [[min, max, avg]]
    plt.rc('font', size=30)
    fig, axs = plt.subplots(1, 2, figsize=(30, 15))

    axs[0].set_title('Year vs driver age')
    axs[0].set_ylim([15, 60])
    axs[0].plot(np.arange(len(d0)) + 1950, d0.T[1], label='max')
    axs[0].plot(np.arange(len(d0)) + 1950, d0.T[2], label='mean')
    axs[0].plot(np.arange(len(d0)) + 1950, d0.T[0], label='min')
    axs[0].legend(loc="upper right")

    axs[1].set_title('Year vs podium age')
    axs[1].set_ylim([15, 60])
    # axs[1].plot(np.arange(len(d1)) + 1950, d1.T[1], label='max')
    axs[1].plot(np.arange(len(d1)) + 1950, d1.T[2], label='mean', color='tab:orange')
    # axs[1].plot(np.arange(len(d1)) + 1950, d1.T[0], label='min')
    axs[1].legend(loc="upper right")
    plt.show()


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
