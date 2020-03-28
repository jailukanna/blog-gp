---
title: tkinter example iss138 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'iss138'

Functions in program: 
* `def main():`

Modules used in program: 
* `import pygubu`
* `import os`

## python iss138

Python tkinter example: iss138

```python
import os
try:
    import tkinter as tk  # for python 3
except:
    import Tkinter as tk  # for python 2
import pygubu


CURDIR = os.path.dirname(__file__)
UI_FILE = os.path.join(CURDIR,'issue138.ui')


class App138:
    def __init__(self):

        #1: Create a builder
        self.builder = builder = pygubu.Builder()

        #2: Load an ui file
        builder.add_from_string(uixml)

        #3: Create the widget using a master as parent
        self.mainwindow = builder.get_object('toplevel1')
        
        #4: connect callbacks
        self.builder.connect_callbacks(self)
        
    def on_print(self):
        entry = self.builder.get_object('Entry_1')
        value = entry.get()
        print('Entry_1:', repr(value))
        
        tkvar = self.builder.get_variable('myvar')
        value = tkvar.get()
        print('Entry_2:', repr(value))
        
    def run(self):
        self.mainwindow.mainloop()


#if __name__ == '__main__':
def main():
    app = App138()
    app.run()

uixml = """<?xml version='1.0' encoding='utf-8'?>
<interface>
  <object class="tk.Toplevel" id="toplevel1">
    <property name="height">200</property>
    <property name="padx">4</property>
    <property name="pady">4</property>
    <property name="title" translatable="yes">Entry</property>
    <property name="width">200</property>
    <child>
      <object class="ttk.Frame" id="Frame_2">
        <property name="height">200</property>
        <property name="width">200</property>
        <layout>
          <property name="column">0</property>
          <property name="propagate">True</property>
          <property name="row">0</property>
        </layout>
        <child>
          <object class="ttk.Label" id="Label_1">
            <property name="text" translatable="yes">Entry:</property>
            <layout>
              <property name="column">0</property>
              <property name="propagate">True</property>
              <property name="row">0</property>
              <property name="sticky">e</property>
            </layout>
          </object>
        </child>
        <child>
          <object class="ttk.Label" id="Label_2">
            <property name="text" translatable="yes">Entry with tk variable:</property>
            <layout>
              <property name="column">0</property>
              <property name="propagate">True</property>
              <property name="row">1</property>
            </layout>
          </object>
        </child>
        <child>
          <object class="ttk.Entry" id="Entry_1">
            <property name="text" translatable="yes">Value A</property>
            <layout>
              <property name="column">1</property>
              <property name="propagate">True</property>
              <property name="row">0</property>
            </layout>
          </object>
        </child>
        <child>
          <object class="ttk.Entry" id="Entry_2">
            <property name="text" translatable="yes">Value B</property>
            <property name="textvariable">string:myvar</property>
            <layout>
              <property name="column">1</property>
              <property name="propagate">True</property>
              <property name="row">1</property>
            </layout>
          </object>
        </child>
        <child>
          <object class="ttk.Frame" id="Frame_3">
            <property name="height">200</property>
            <property name="padding">0 10 0 0</property>
            <property name="width">200</property>
            <layout>
              <property name="column">0</property>
              <property name="columnspan">2</property>
              <property name="propagate">True</property>
              <property name="row">2</property>
              <property name="sticky">ew</property>
              <rows>
                <row id="0">
                  <property name="weight">0</property>
                </row>
              </rows>
              <columns>
                <column id="0">
                  <property name="weight">1</property>
                </column>
              </columns>
            </layout>
            <child>
              <object class="ttk.Button" id="button1">
                <property name="command">on_print</property>
                <property name="text" translatable="yes">Print</property>
                <layout>
                  <property name="column">0</property>
                  <property name="propagate">True</property>
                  <property name="row">0</property>
                  <property name="sticky">e</property>
                </layout>
              </object>
            </child>
          </object>
        </child>
      </object>
    </child>
  </object>
</interface>
"""


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
