---
title: tkinter example Files n things (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'Files n things'


Modules used in program: 
* `import errorMod`
* `import tkinter`

## python Files n things

Python tkinter example: Files n things

```python
import tkinter
import errorMod

class FileCreator():
    
    enter_file = input('Enter file name: ').strip()

    def createFile(self, enter_file):
        """ Creates and over-writes existing files"""
        try:
            with open(enter_file, 'w') as pplwrite:
                text_To_Be_Written = str(input('enter text to be written to file'))
                create = pplwrite.write(text_To_Be_Written)
        except FileNotFoundError:
            print('We had trouble writing to that file')
        else:
            print(f'File update successful, your input : {textToBeWritten}')

    

    def seekNadd(self, enter_file):
        """ adds text to a file"""
        try:
            with open(FileCreator.enter_file, 'a') as ppladd:
                text_To_Be_Written = input('Enter Text to add to file').strip()
                stuff_Added = ppladd.write(text_To_Be_Written)
        except FileNotFoundError:
            print('Error writing to that file')

        else:
            print('Write successfull')


    def readFile(self, enter_file):
        """ Reads a file"""
        try:
            with open(FileCreator.enter_file, 'r') as pplread:
                read_file = pplread.read()
                print(read_file)
        except FileNotFoundError:
            self.storeError()
        else:
            print('Read Successful!')
            

    def storeError(self):
        """ Stores and then sends error to 'displayError """
              
        tk = tkinter.Tk()
        tk.title('Error')
        errorMessage = tkinter.Label(tk, text='Reading the file was Unsuccessful, click for more Details')
        errorMessage.pack()
        detailsButton = tkinter.Button(tk, text='Details', command=errorMod.displayError)
        detailsButton.pack()

     
job1 = FileCreator()
job1.readFile(FileCreator.enter_file)
                                           


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
