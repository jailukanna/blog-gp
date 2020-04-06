---
title: mysql example data lake test main ui (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'data lake test main ui'


Modules used in program: 
* `import sys`

## python data lake test main ui

Python mysql example: data lake test main ui

```python
import sys
from PyQt5.QtWidgets import QApplication,QMainWindow,QStackedLayout
from data_lake.gui.ui.home import Ui_MainWindow
from data_lake.gui.panels import *
from data_lake.gui.model import queries_model
from data_lake.gui.delegate import queries_delegate

class MyMainWindow(QMainWindow,Ui_MainWindow):
    def __init__(self):
        super(MyMainWindow,self).__init__()
        self.setupUi(self)

        self.qsl = QStackedLayout(self.frame_2)
        dashboard_panel  = DashboardPanel()
        data_source_panel = DataSourcePanel()
        schema_discovery_panel = SchemaDiscoveryPanel()
        schema_matching_panel = SchemaMatchingPanel()
        schema_summary_panel = SchemaSummaryPanel()


        queries_panel = QueriesPanel()
        query_model = queries_model.QuriesPanelModel()
        self.m = query_model
        queries_panel.tableView.setModel(query_model)
        #queries_panel.tableView.setItemDelegate(queries_delegate.QueriesDelegate())
        queries_panel.lineEdit.returnPressed.connect(self.query)


        data_quality_panel = DashboardPanel()
        about_panel = AboutPanel()

        self.qsl.addWidget (dashboard_panel)
        self.qsl.addWidget(data_source_panel)
        self.qsl.addWidget(schema_discovery_panel)
        self.qsl.addWidget (schema_matching_panel)
        self.qsl.addWidget (schema_summary_panel)
        self.qsl.addWidget (queries_panel)
        self.qsl.addWidget (data_quality_panel)
        self.qsl.addWidget (about_panel)


    def query(self):
        info =  self.qsl.widget(5).lineEdit.text()
        self.m.get_data(info)

    def show_panel(self):
        dic = {'dashboard':0,
               'data_source':1,
               'schema_discovery': 2,
               'schema_matching': 3,
               'schema_summary': 4,
               'queries': 5,
               'data_quality': 6,
               'about': 7}
        index = dic[self.sender().objectName()]
        self.qsl.setCurrentIndex(index)




if __name__ == '__main__':
    app = QApplication([])
    main_window = MyMainWindow()
    main_window.show()

    sys.exit(app.exec_())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
