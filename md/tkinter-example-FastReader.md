---
title: tkinter example FastReader (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'FastReader'


Modules used in program: 
* `import tkinter.messagebox`
* `import tkinter.filedialog`
* `import tkinter.ttk`
* `import tkinter`

## python FastReader

Python tkinter example: FastReader

```python
import tkinter
import tkinter.ttk
import tkinter.filedialog
import tkinter.messagebox

class FastReader(object):

	WPM = 250

	def __init__(self):
		self.filename = self.getFile()
		self.running = True

	def display(self):
		"""
		Display the window and call the updateLabel function in order to update the Label
		with the appropiate text.
		"""

		self.root = tkinter.Tk()
		self.root.wm_title("Fast Reader.")
		self.root.geometry("530x160")

		self.text = tkinter.ttk.Label(self.root, text = "")
		self.text.pack()

		# Stop button
		self.stop = tkinter.ttk.Button(self.root, text = "Stop", command = lambda: self.root.destroy())
		self.stop.pack()

		self.words = self.getWords()
		self.updateLabel()

		self.root.update_idletasks()
		self.root.mainloop()

	def updateLabel(self):
		"""
		Updates the label with words from the file.
		It will check the words list and it will try to change the label's text.
		It also handles the case when the list is empty.
		"""
		
		word = self.words.pop()
		self.text.config(text = word)

		if not self.words:
			self.words = self.getWords()

		if self.words:
			self.root.after(FastReader.WPM, self.updateLabel)

	def getFile(self):
		file_ = tkinter.filedialog.askopenfilename()
		return file_

	def getWords(self):
		"""
		Opens the file and splits the line into words witch are appended into a list.
		It will handle IOERROR/Exception(s) with a error message.
		"""

		word_list = []

		try:

			with open(self.filename) as file_:
				for line in file_:
					for word in line.strip("\n").split(" "):
						word_list.append(word)

			return word_list

		except Exception as e:
			tkinter.messagebox.showerror("Error", "Error while opening file.\nRaw: {}".format(e))

if __name__ == "__main__":
	fr = FastReader()
	fr.display()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
