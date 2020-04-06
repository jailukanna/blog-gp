---
title: mysql example data lake gui delegate queries delegate (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'data lake gui delegate queries delegate'


## python data lake gui delegate queries delegate

Python mysql example: data lake gui delegate queries delegate

```python

from PyQt5.QtWidgets import QStyledItemDelegate,QLineEdit
from PyQt5.QtCore import Qt,QVariant


class QueriesDelegate(QStyledItemDelegate):
    def __init__(self):
        super().__init__()

    def createEditor(self, parent, option, index):
        text = QLineEdit (parent)
        return text

    def setEditorData(self, editor, index):
        item_var = index.data(Qt.DisplayRole)
        editor.setText(item_var)
        print(item_var)

    def setModelData(self, editor, model, index):
        value = editor.text()
        value_var = QVariant(value)
        model.setData(index,value_var)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
