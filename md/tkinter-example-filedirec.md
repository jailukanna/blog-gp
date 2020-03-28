---
title: tkinter example filedirec (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'filedirec'

Functions in program: 
* `def save_file(text,filename=None,codec='utf_8'):`
* `def view_file(filename=None):`
* `def change_directory(directory=None):`
* `def get_directory():`
* `def get_filename_list(filetype=(u'全てのファイル','*.*')):`
* `def get_filename(filetype=(u'全てのファイル','*.*'),mode='load'):`

## python filedirec

Python tkinter example: filedirec

```python
def get_filename(filetype=(u'全てのファイル','*.*'),mode='load'):
    import Tkinter
    import tkFileDialog
    import tkMessageBox
    root=Tkinter.Tk()
    root.withdraw()
    tkMessageBox.showinfo('showinfo','ファイル名を取得します。')
    fTyp=[('テキスト','*.txt'),('コンマ区切りテキスト','*.csv'),('PNGファイル','*.PNG'),('HTMLファイル','*.htm;*.html;*.xhtml'),
    ('パイソンファイル','*.py'),filetype]
    if mode=='load':
        filename=tkFileDialog.askopenfilename(filetypes=fTyp)
    elif mode=='save':
        filename=tkFileDialog.asksaveasfilename(filetypes=fTyp)
    return filename


def get_filename_list(filetype=(u'全てのファイル','*.*')):
    import Tkinter
    import tkFileDialog
    import tkMessageBox
    root=Tkinter.Tk()
    root.withdraw()
    tkMessageBox.showinfo('showinfo','ファイル名のリストを取得します。')
    fTyp=[('テキスト','*.txt'),('コンマ区切りテキスト','*.csv'),('PNGファイル','*.PNG'),('HTMLファイル','*.htm;*.html;*.xhtml'),
    ('パイソンファイル','*.py'),filetype]
    filename_list=tkFileDialog.askopenfilenames(filetypes=fTyp)
    return filename_list


def get_directory():
    import Tkinter
    import tkFileDialog
    import tkMessageBox
    root=Tkinter.Tk()
    root.withdraw()
    tkMessageBox.showinfo('showinfo','ディレクトリ名を取得します。')
    dirname=tkFileDialog.askdirectory()
    return dirname

def change_directory(directory=None):
    import os
    if directory==None:
        directory=get_directory()
    os.chdir(directory)
    print(os.getcwd())

def view_file(filename=None):
    import codecs
    if filename==None:
        filename=get_filename()
    lookup = ('utf_8', 'euc_jp', 'euc_jis_2004', 'euc_jisx0213',
    'shift_jis', 'shift_jis_2004','shift_jisx0213',
    'iso2022jp', 'iso2022_jp_1', 'iso2022_jp_2', 'iso2022_jp_3',
    'iso2022_jp_ext','latin_1', 'ascii')
    for codec in lookup:
        f=codecs.open(filename,'r',codec)
        try:
            print(f.read())
            #print(u"エンコード："+codec)
            break
        except:
            pass

def save_file(text,filename=None,codec='utf_8'):
    import codecs
    if filename==None:
        filename=get_filename(mode='save')
    f=codecs.open(filename,'w',codec)
    f.write(text)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
