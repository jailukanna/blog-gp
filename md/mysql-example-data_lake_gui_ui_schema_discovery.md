---
title: mysql example data lake gui ui schema discovery (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'data lake gui ui schema discovery'


## python data lake gui ui schema discovery

Python mysql example: data lake gui ui schema discovery

```python
# -*- coding: utf-8 -*-

# Form implementation generated from reading ui file 'schema_discovery.ui'
#
# Created by: PyQt5 UI code generator 5.13.0
#
# WARNING! All changes made in this file will be lost!


from PyQt5 import QtCore, QtGui, QtWidgets


class Ui_Form(object):
    def setupUi(self, Form):
        Form.setObjectName("Form")
        Form.resize(400, 300)
        self.gridLayout = QtWidgets.QGridLayout(Form)
        self.gridLayout.setObjectName("gridLayout")
        self.label = QtWidgets.QLabel(Form)
        self.label.setObjectName("label")
        self.gridLayout.addWidget(self.label, 0, 0, 1, 1)

        self.retranslateUi(Form)
        QtCore.QMetaObject.connectSlotsByName(Form)

    def retranslateUi(self, Form):
        _translate = QtCore.QCoreApplication.translate
        Form.setWindowTitle(_translate("Form", "Schema_Discorvery"))
        self.label.setText(_translate("Form", "schema_discovery"))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
