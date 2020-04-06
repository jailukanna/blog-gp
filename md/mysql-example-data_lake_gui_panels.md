---
title: mysql example data lake gui panels (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'data lake gui panels'


## python data lake gui panels

Python mysql example: data lake gui panels

```python
from data_lake.gui.ui import dashboard,data_source,schema_discovery,schema_matching,schema_summary,queries,data_quality,about
from PyQt5.QtWidgets import QWidget
from PyQt5.QtSql import QSqlTableModel
from data_lake.gui.model import queries_model
from data_lake.gui.delegate import queries_delegate


class DashboardPanel(QWidget,dashboard.Ui_Form):
    def __init__(self):
        super().__init__()
        self.setupUi(self)
class DataSourcePanel(QWidget,data_source.Ui_Form):
    def __init__(self):
        super().__init__()
        self.setupUi(self)

class SchemaDiscoveryPanel(QWidget,schema_discovery.Ui_Form):
    def __init__(self):
        super().__init__()
        self.setupUi(self)

class SchemaMatchingPanel(QWidget,schema_matching.Ui_Form):
    def __init__(self):
        super().__init__()
        self.setupUi(self)


class SchemaSummaryPanel(QWidget,schema_summary.Ui_Form):
    def __init__(self):
        super().__init__()
        self.setupUi(self)

class QueriesPanel(QWidget,queries.Ui_Form):
    def __init__(self):
        super().__init__()
        self.setupUi(self)

class DataQuality(QWidget,data_quality.Ui_Form):
    def __init__(self):
        super().__init__()
        self.setupUi(self)

class AboutPanel(QWidget,about.Ui_Form):
    def __init__(self):
        super().__init__()
        self.setupUi(self)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
