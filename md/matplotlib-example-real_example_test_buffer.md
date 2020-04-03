---
title: matplotlib example real example test buffer (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'real example test buffer'


Modules used in program: 
* `import mock`
* `import unittest`
* `import matplotlib.pyplot`
* `import io`

## python real example test buffer

Python matplotlib example: real example test buffer

```python
import io
import matplotlib.pyplot

import unittest
import mock
from mock import mock_open, patch, MagicMock, Mock


class BufferMatplotlib(object):
	def __init__(self):
		self.buffer = io.BytesIO()
		self.buffers = []

	def append_plt(self, plt, name, extension):
		plt.savefig(self.buffer, format=extension)
		self.buffer.seek(0)
		d = {'buffer':self.buffer, 'name':name, 'extension':extension}
		self.buffers.append(d)
		plt.close()
		self.buffer = io.BytesIO()

	def save_buffers(self, path):
		for buf in self.buffers:
			name_extension = buf['name'] + '.' + buf['extension']
			full_path = os.path.join(path, name_extension)

			with open(full_path, 'wb') as f:
				f.write(buf['buffer'].getvalue())


class TestBufferMatplotlib(unittest.TestCase):

	def test_append_plt(self):
		buf = BufferMatplotlib()		

		with patch('matplotlib.pyplot') as my_mocked_plot:
			buf.append_plt(my_mocked_plot, name='name1', extension='png')

		self.assertTrue(len(buf.buffers) == 1)
		
		for i in range(10):
			with patch('matplotlib.pyplot') as my_mocked_plot:
				buf.append_plt(my_mocked_plot, name='n' + str(i), extension='png')
		
		self.assertTrue(len(buf.buffers) == 11)
		

	def test_save_buffers(self):
		pass
					
			
if __name__ == '__main__':
	unittest.main()				

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
