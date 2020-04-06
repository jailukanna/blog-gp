---
title: mysql example data lake gui ui home (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'data lake gui ui home'


## python data lake gui ui home

Python mysql example: data lake gui ui home

```python
# -*- coding: utf-8 -*-

# Form implementation generated from reading ui file 'home.ui'
#
# Created by: PyQt5 UI code generator 5.12
#
# WARNING! All changes made in this file will be lost!

from PyQt5 import QtCore, QtGui, QtWidgets


class Ui_MainWindow(object):
    def setupUi(self, MainWindow):
        MainWindow.setObjectName("MainWindow")
        MainWindow.resize(995, 767)
        self.centralwidget = QtWidgets.QWidget(MainWindow)
        self.centralwidget.setObjectName("centralwidget")
        self.gridLayout = QtWidgets.QGridLayout(self.centralwidget)
        self.gridLayout.setObjectName("gridLayout")
        self.horizontalLayout = QtWidgets.QHBoxLayout()
        self.horizontalLayout.setObjectName("horizontalLayout")
        self.frame = QtWidgets.QFrame(self.centralwidget)
        self.frame.setEnabled(True)
        self.frame.setMaximumSize(QtCore.QSize(200, 800))
        self.frame.setFrameShape(QtWidgets.QFrame.StyledPanel)
        self.frame.setFrameShadow(QtWidgets.QFrame.Raised)
        self.frame.setObjectName("frame")
        self.gridLayout_2 = QtWidgets.QGridLayout(self.frame)
        self.gridLayout_2.setObjectName("gridLayout_2")
        self.groupBox = QtWidgets.QGroupBox(self.frame)
        self.groupBox.setTitle("")
        self.groupBox.setObjectName("groupBox")
        self.verticalLayout = QtWidgets.QVBoxLayout(self.groupBox)
        self.verticalLayout.setObjectName("verticalLayout")
        self.dashboard = QtWidgets.QPushButton(self.groupBox)
        self.dashboard.setObjectName("dashboard")
        self.verticalLayout.addWidget(self.dashboard)
        self.data_source = QtWidgets.QPushButton(self.groupBox)
        self.data_source.setObjectName("data_source")
        self.verticalLayout.addWidget(self.data_source)
        self.schema_discovery = QtWidgets.QPushButton(self.groupBox)
        self.schema_discovery.setObjectName("schema_discovery")
        self.verticalLayout.addWidget(self.schema_discovery)
        self.schema_matching = QtWidgets.QPushButton(self.groupBox)
        self.schema_matching.setObjectName("schema_matching")
        self.verticalLayout.addWidget(self.schema_matching)
        self.schema_summary = QtWidgets.QPushButton(self.groupBox)
        self.schema_summary.setObjectName("schema_summary")
        self.verticalLayout.addWidget(self.schema_summary)
        self.queries = QtWidgets.QPushButton(self.groupBox)
        self.queries.setObjectName("queries")
        self.verticalLayout.addWidget(self.queries)
        self.data_quality = QtWidgets.QPushButton(self.groupBox)
        self.data_quality.setObjectName("data_quality")
        self.verticalLayout.addWidget(self.data_quality)
        self.about = QtWidgets.QPushButton(self.groupBox)
        self.about.setObjectName("about")
        self.verticalLayout.addWidget(self.about)
        self.gridLayout_2.addWidget(self.groupBox, 0, 0, 1, 1)
        self.horizontalLayout.addWidget(self.frame)
        self.frame_2 = QtWidgets.QFrame(self.centralwidget)
        self.frame_2.setMaximumSize(QtCore.QSize(1000, 800))
        self.frame_2.setFrameShape(QtWidgets.QFrame.StyledPanel)
        self.frame_2.setFrameShadow(QtWidgets.QFrame.Raised)
        self.frame_2.setObjectName("frame_2")
        self.horizontalLayout.addWidget(self.frame_2)
        self.gridLayout.addLayout(self.horizontalLayout, 0, 0, 1, 1)
        MainWindow.setCentralWidget(self.centralwidget)
        self.menubar = QtWidgets.QMenuBar(MainWindow)
        self.menubar.setGeometry(QtCore.QRect(0, 0, 995, 26))
        self.menubar.setObjectName("menubar")
        MainWindow.setMenuBar(self.menubar)
        self.statusbar = QtWidgets.QStatusBar(MainWindow)
        self.statusbar.setObjectName("statusbar")
        MainWindow.setStatusBar(self.statusbar)

        self.retranslateUi(MainWindow)
        self.data_source.clicked.connect(MainWindow.show_panel)
        self.schema_discovery.clicked.connect(MainWindow.show_panel)
        self.dashboard.clicked.connect(MainWindow.show_panel)
        self.schema_matching.clicked.connect(MainWindow.show_panel)
        self.schema_summary.clicked.connect(MainWindow.show_panel)
        self.queries.clicked.connect(MainWindow.show_panel)
        self.data_quality.clicked.connect(MainWindow.show_panel)
        self.about.clicked.connect(MainWindow.show_panel)
        QtCore.QMetaObject.connectSlotsByName(MainWindow)

    def retranslateUi(self, MainWindow):
        _translate = QtCore.QCoreApplication.translate
        MainWindow.setWindowTitle(_translate("MainWindow", "Data_Lake"))
        self.dashboard.setText(_translate("MainWindow", "DASHBOARD"))
        self.data_source.setText(_translate("MainWindow", "DATA SOURCES"))
        self.schema_discovery.setText(_translate("MainWindow", "SCHEMA DISCOVERY"))
        self.schema_matching.setText(_translate("MainWindow", "SCHEMA MATCHING"))
        self.schema_summary.setText(_translate("MainWindow", "SCHEMA SUMMARY"))
        self.queries.setText(_translate("MainWindow", "QUERIES"))
        self.data_quality.setText(_translate("MainWindow", "DATA QUALITY"))
        self.about.setText(_translate("MainWindow", "ABOUT"))




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
