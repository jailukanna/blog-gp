---
title: mysql example languageDistribution (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'languageDistribution'

Functions in program: 
* `def totalNotebookCount(cursor):`
* `def topLanguages(cursor, n):`

Modules used in program: 
* `import sys`
* `import mysql.connector`
* `import matplotlib.pyplot as plt`

## python languageDistribution

Python mysql example: languageDistribution

```python
''' Displays the distribution of the N most common languages of the corpus as specified by a command line argument.'''


import matplotlib.pyplot as plt
import mysql.connector
import sys

if len(sys.argv)<2:
    print("Specify number of languages!")
    exit()

''' Returns a list of tuples containing the names and corresponding notebook counts for the 'n' most common languages'''
def topLanguages(cursor, n):
    results = []

    # Number of Python notebooks
    cursor.execute('''SELECT Name, COUNT(*)\
                      FROM Notebooks INNER JOIN Languages ON Language=LanguageId\
                      GROUP BY Language\
                      HAVING Name LIKE "%python%"
                      ORDER BY COUNT(*) DESC;''')
    (python, python_count) = cursor.fetchone()
    results.append((python, python_count))

    # All other notebooks
    cursor.execute('''SELECT Name, COUNT(*)\
                      FROM Notebooks INNER JOIN Languages ON Language=LanguageId\
                      GROUP BY Language\
                      HAVING Name NOT LIKE "%python%"
                      ORDER BY COUNT(*) DESC;''')

    for i in range(n-1):
        (lang, count) = cursor.fetchone()
        results.append((lang, count))

    return results


''' Returns the total number of notebooks in the database'''
def totalNotebookCount(cursor):
    cursor.execute('''SELECT COUNT(*) FROM Notebooks;''')
    (count,) = cursor.fetchone()
    return count

n = int(sys.argv[1])

#MySQL config
config = {
  'user': 'root',
  'password': 'password',
  'host': 'localhost',
  'database': 'jupyter',
  'raise_on_warnings': True
}

conn = mysql.connector.connect(**config)
c = conn.cursor(buffered=True)

total_notebooks = totalNotebookCount(c)
lang_counts = topLanguages(c, n)

languages = [] # Plot legend
vals = [] # Plot data
others = 1 # Fraction of languages not within the top N

for (name, count) in lang_counts:
    perc = count/total_notebooks
    others -= perc
    vals.append(perc)
    languages.append("{}, {}%".format(name, round(perc*100, 3)))

vals.append(others)
languages.append("Others, {}%".format(round(others,3)))

# Plot config
fig, ax = plt.subplots()
size = 0.3
cmap = plt.get_cmap("tab20c")
colors=['#ff9900','#b3b3cc','#00b33c','#cc0000', '#9900cc', '#90aa0c']
explode = [.1 for i in range(n+1)]
wedges, autotexts = ax.pie(vals, radius=1-size, colors=colors,
       wedgeprops=dict(width=size, edgecolor='k'), startangle=-40, explode=explode)

plt.figtext(.5,.9,'Language Distribution', fontsize=20, ha='center')
ax.legend(wedges, languages,
          title="Languages",
          loc="center left",
          bbox_to_anchor=(0.8, 0, 0.5, 1))

# plt.show()
plt.savefig("language_distribution.pdf")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
