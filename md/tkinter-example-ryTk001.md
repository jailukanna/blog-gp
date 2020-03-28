---
title: tkinter example ryTk001 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'ryTk001'


Modules used in program: 
* `import tkinter.filedialog`
* `import random`
* `import tkinter`

## python ryTk001

Python tkinter example: ryTk001

```python
'''
ryTk001.py
示範 tkinter 的範例。
Renyuan Lyu, 呂仁園，2014/07/23，颱風夜。

'''
import tkinter

範例01= '''
tk=     tkinter.Tk()

frame=  tkinter.Frame(tk)
label=  tkinter.Label(frame, text= "Hello, World")
button= tkinter.Button(frame,text= "Exit", command= tk.destroy)

frame.grid()
label.grid()
button.grid()

tk.mainloop()
'''

範例02= '''
class Application(tkinter.Frame):

    def __init__(self, master= None):
        tkinter.Frame.__init__(self, master)
        self.pack()
        self.createWidgets()

    def createWidgets(self):
        self.hi_there= tkinter.Button(self)
        self.hi_there["text"]= "Hello World\n(click me)"
        self.hi_there["command"]= self.say_hi
        self.hi_there.pack(side="top")

        self.QUIT= tkinter.Button(self, text= "QUIT", fg= "red",
                                            command= root.destroy)
        self.QUIT.pack(side= "bottom")

    def say_hi(self):
        print("hi there, everyone!")

root= tkinter.Tk()
app= Application()#master=root)
app.mainloop()
'''

import random

#------------------------------------------
# tkinter_tc.py
# 把 TK 中文化
# Renyuan Lyu, 呂仁園，2014/07/22
#------------------------------------------

class 窗類(tkinter.Tk):

      def __init__(我, *x,**y):
          super().__init__(*x,**y)

      def 主迴圈(我, *x,**y):
          我.mainloop(*x,**y)

class 框類(tkinter.Frame):

      def __init__(我, *x,**y):
           super().__init__(*x,**y)

class 鈕類(tkinter.Button):

      def __init__(我, *x,**y):
           super().__init__(*x,**y)

'''
class 布類(tkinter.Canvas):

      def __init__(我, *x,**y):
           super().__init__(*x,**y)
'''

class 文類(tkinter.Text):

      def __init__(我, *x,**y):
           super().__init__(*x,**y)

class 標類(tkinter.Label):

      def __init__(我, *x,**y):
           super().__init__(*x,**y)

class 入口類(tkinter.Entry):

      def __init__(我, *x,**y):
           super().__init__(*x,**y)

class 選單類(tkinter.Menu):

      def __init__(我, *x,**y):
           super().__init__(*x,**y)

class 布類(tkinter.Canvas):

    def __init__(我,*x,**y):
        try:
            y['bg']= y.pop('背景色')
        except:
            pass

        super().__init__(*x,**y)
    '''
    打包= pack

    造弧= create_arc
    造圖= create_bitmap
    造像= create_image
    造線= create_line
    造圓= create_oval
    造多邊= create_polygon
    造方= create_rectangle
    造字= create_text
    造窗= create_window
    '''

    def 造線(我,*x,**y):
        我.create_line(*x,**y)

    def 打包(我,*x,**y):
        我.pack(*x,**y)

    def 造弧(我,*x,**y):
        我.create_arc(*x,**y)

    def 造圖(我,*x,**y):
        我.create_bitmap(*x,**y)

    def 造像(我,*x,**y):
        我.create_image(*x,**y)

    def 造線(我,*x,**y):
        我.create_line(*x,**y)

    def 造圓(我,*x,**y):
        我.create_oval(*x,**y)

    def 造多邊(我,*x,**y):
        我.create_polygon(*x,**y)

    def 造方(我,*x,**y):
        我.create_rectangle(*x,**y)

    def 造字(我,*x,**y):

        try:
            y['text']= y.pop('字')
        except:
            pass

        我.create_text(*x,**y)

    def 造窗(我,*x,**y):
        我.create_window(*x,**y)

    def 刪除全部(我):
        我.delete('all')

#--------------------
# 應用程式由此開始。
#--------------------

class 應用框類(框類):

    def __init__(我, *x,**y):

        super().__init__(*x,**y)

        我.pack()

        我['bg']= 'yellow'
        我['border']= 10

        我.在框內造小元件()

    def 在框內造小元件(我):

        我.毀滅鈕= 鈕類(我)
        我.毀滅鈕["text"]=       "毀滅鈕"
        我.毀滅鈕["command"]=     我.毀滅

        我.招呼鈕= 鈕類(我)
        我.招呼鈕["text"]=       "招呼鈕"
        我.招呼鈕["command"]=     我.招呼



        我.標= 標類(我)
        我.標['text']= '標'
        我.標['border']= 10

        我.入口= 入口類(我)
        我.入口['border']= 10

        def 在入口取式算後刪除(e):

            式= 我.入口.get()
            try:    值= str(eval(式))
            except: 值= 式
            文= 式 + ' = ' + 值 +'\n'
            我.標['text']= 文
            我.文.insert(tkinter.END, 文)
            我.入口.delete(0, tkinter.END)
        我.入口.bind('<Key-Return>', 在入口取式算後刪除)


        我.文= 文類(我)
        我.文['bg']= 'cyan'
        我.文['fg']= 'red'


        我.布= 布類(我)
        我.布['bg']= 'pink'

        #
        # 在布上畫圖
        #
        我.布.造線(0,0,100,100)
        我.布.造圓(100,100,200,200)
        我.布.造方(100,100,200,200)
        我.布.造多邊(100,150,150,100,200,200)

        def 清(e):
            我.布['bg']= random.choice(['red','blue','green'])
            我.布.刪除全部()
        我.布.bind('<Button-3>',清)

        def 畫(e):
            我.布.造圓(e.x, e.y, e.x+10, e.y+10)
        我.布.bind('<B1-Motion>',  畫)


        元件群= [我.文, 我.布, 我.標, 我.入口, 我.招呼鈕, 我.毀滅鈕]

        i = 0
        N= round(len(元件群)**.5)
        for x in 元件群:
            x.grid(row=i//N, column=i%N)
            i += 1

    def 招呼(我):

        #print('你好！')

        x= random.randint(0, 我.布.winfo_width())
        y= random.randint(0, 我.布.winfo_height())

        招呼語= '你好(%d,%d)'%(x,y)

        #print(招呼語)

        我.標['text']= 招呼語
        我.布.create_text(x,y, text= 招呼語)
        我.文.insert(tkinter.END, 招呼語)


    def 毀滅(我):

        我.destroy()

import tkinter.filedialog

class 主選單類(選單類):

    def __init__(我, *x,**y):

        super().__init__(*x,**y)

        #主選單=    選單類(窗)
        #主選單.add_command(label= '主01')
        def 主01():
            global 應用框
            #print('主01()')
            應用框=  應用框類()
            pass

        def 主02():
            global 應用框
            #print('主02()')

            #print("開舊檔()....")
            檔名= tkinter.filedialog.askopenfilename()
            #print(檔名)

            try:
                f= open(檔名, mode= 'r', encoding='utf-8')
                檔案內容= f.read()
                f.close()

                應用框.文.insert(tkinter.END, 檔案內容)
                應用框.標['text']= 檔名
            except:
                   try:
                       應用框.布.影像= tkinter.PhotoImage(file= 檔名)
                       應用框.布.create_image(0,0, image= 應用框.布.影像)
                       應用框.標['text']= 檔名
                   except:
                          訊息= '不支援 '+ 檔名
                          應用框.標['text']= 訊息
                          pass
            pass

        我.add_command(label= '主01', command= 主01)
        我.add_command(label= '主02', command= 主02)
        我.add_command(label= '主03')

        父選單=    選單類(我)
        父選單.add_command(label= '父01')
        父選單.add_command(label= '父02')
        父選單.add_command(label= '父03')

        子選單=    選單類(父選單)
        子選單.add_command(label= '子01')
        子選單.add_command(label= '子02')
        子選單.add_command(label= '子03')

        孫選單=    選單類(子選單)
        孫選單.add_command(label= '孫01')
        孫選單.add_command(label= '孫02')
        孫選單.add_command(label= '孫03')

        子選單.add_cascade(label= '子04', menu= 孫選單)
        父選單.add_cascade(label= '父04', menu= 子選單)
        我.add_cascade(    label= '主04', menu= 父選單)

        窗['menu']= 我 #主選單


if __name__=='__main__':

    global 應用框

    窗=      窗類()

    主選單=  主選單類(窗)
    應用框=  應用框類(窗)

    窗.主迴圈()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
