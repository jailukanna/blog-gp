---
title: mysql example db ui (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'db ui'


Modules used in program: 
* `import datetime`

## python db ui

Python mysql example: db ui

```python
import datetime

class DatabaseUIBase(object):
	''' Base class for ex5 UIs '''
	def __init__(self, db, msg):
		''' Create a UI object and call the mainUI '''
		self.db = db
		print(msg)
		print('-' * 10)
		self.mainUI()

	def __add_entry(self, table):
		''' Add an entry to the table '''
		# Get the column names
		columns = self.db.get_columns(table)
		values = ['' for col in columns]
		print('Please enter the following details:')
		for index, column in enumerate(columns):
			# If the column type is a date
			if column[1] == 10:
				print('Please enter date in the following format: DD.MM.YYYY')
			values[index] = raw_input('%s: ' % str(column[0]))
			# If the column type is a date
			if column[1] == 10:
				try:
					# Try to parse values and create a datetime object
					day, mnt, year = values[index].split('.')
					values[index] = datetime.datetime(int(year), int(mnt), int(day))
				except (TypeError, ValueError):
					# If the user didn't follow the format, present an erro an send default values
					print("Error in date format, inserting '1'.'1'.'1' as date")
					values[index] = datetime.datetime(1, 1, 1)
		try:
			self.db.insert(table, [i[0] for i in columns], values)
		except ValueError, e:
			print('Error: %s' % str(e))
			return
		print('Inserted the follwoing entry into %s:' % str(table))
		print(' | '.join([str(val) for val in values]))

	def __print_columns(self, table):
		''' Print the columns of a table '''
		columns = ['#'] + self.db.get_column_names(table)
		print(' | '.join(columns))

	def __print_results(self, table, enumerator):
		''' Print a query result in a nice form '''
		# Print the columns of the table
		self.__print_columns(table)
		cnt = 0
		# Print each entry in a formatted line
		for index, entry in enumerator:
			entry = [str(val) for val in entry]
			print(('%d | ' % (index + 1)) + ' | '.join(entry))
			cnt += 1
		# Display the amount of entries found
		print('%d results found.' % cnt)

	def __print_table(self, table):
		''' Print a table '''
		table_enum = enumerate(self.db.iter_entries(table))
		self.__print_results(table, table_enum)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
