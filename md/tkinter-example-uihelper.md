---
title: tkinter example uihelper (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'uihelper'

Functions in program: 
* `def handleSubmit(fileName, patientName, patientAmount):`
* `def createGUI():`
* `def createButton(tkGUI, textItem, commandFunction, itemRow, itemColumn, dirSticky=None, font=None):`

Modules used in program: 
* `import tkinter.font as tkFont`
* `import tkinter as tk`

## python uihelper

Python tkinter example: uihelper

```python
# Import tkinter UI libraries
import tkinter as tk
import tkinter.font as tkFont

def createButton(tkGUI, textItem, commandFunction, itemRow, itemColumn, dirSticky=None, font=None):
    """
    Creates and returns a button for use in the GUI.
    
    Args: 
        tkGUI (class): The tkinter class for use in creating the GUI.
        textItem (str): The text content of the button.
        commandFunction (function): The function that used when the user selects the button.
        itemRow (int): The grid row where the label should be placed.
        itemColumn (int): The grid column where the label should be placed.
    """
    
    # Create button using tkinter
    button = tk.Button(tkGUI, text=textItem, command=commandFunction, font=tkFont.Font(family="Helvetica", size=10, weight="bold", font=font))
    # Place button within the UI window
    button.grid(row=itemRow, column=itemColumn, sticky=dirSticky)
    # Return the created button
    return button
   
def createGUI():
	"""
	Creates the tkinter GUI and starts the mainloop.
	"""

	# Set initial GUI resolution and title.
	root.geometry("750x150")
	root.title("Bulk Upload File Generator")
	
	# Create input field labels and entries (helper functions for createLabel and createEntry are not shown in this Gist)
	createLabel(root, "* File name", 0, 0, customFont)
	fileName = createEntry(root, 0, 1)
	
	createLabel(root, "Patient prefix", 1, 0, customFont)
	patientName = createEntry(root, 1, 1)

	createLabel(root, "* Number of patients", 2, 0, customFont)
	patientAmount = createEntry(root, 2, 1)
	
	# Create button for input submission
        # "root" is the tkinter UI window; "Submit" is the label for the button
        # The lambda function calls another function when the user presses the button
	submitButton = createButton(root, "Submit", lambda: self.handleSubmit(fileName, patientName, patientAmount), 3, 0)

def handleSubmit(fileName, patientName, patientAmount):
    # Insert code to execute when the user presses the "Submit" button in the UI
    # ... do something with the fileName, patientName, and patientAmount
    pass

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
