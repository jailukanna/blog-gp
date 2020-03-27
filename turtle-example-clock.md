---
title: turtle example clock (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'clock'

Functions in program: 
* `def main():`
* `def tick(): #很重要(主要控制指針旋轉的def)`
* `def datum(z): #將日期設為一個數列，讓後面使用 【此處改編】加入斜線`
* `def wochentag(t): #將星期設為一個數列，讓後面使用 【此處改編】加入中文`
* `def setup(): #三個指針的各種設定`
* `def clockface(radius):#radius=半徑`
* `def make_hand_shape(name, laenge, spitze):`
* `def hand(laenge, spitze): #時針、分針、秒針，spitze尖端 & laenge長度 皆為德文`
* `def jump(distanz, winkel=0):#(德文)【distanz=距離】【winkel=角度】`

## python clock

Python turtle example: clock

```python
'''
深入研究每一行程式的意義，增進自己的理解能力，也讓更多初學者可以直接輕鬆地尋找到中文的分析。
另外，與時間相關的python程式，我曾寫在我的blog裡面，歡迎去看看~
https://jypythonlearning.blogspot.tw/2017/02/20170217jytime.html
'''
from turtle import * #import 後面一定要加上*，否則會產生語法錯誤
from datetime import datetime #有關時間的偵測

def jump(distanz, winkel=0):#(德文)【distanz=距離】【winkel=角度】
    penup() #提筆
    right(winkel)
    forward(distanz)
    left(winkel)
    pendown()#下筆

def hand(laenge, spitze): #時針、分針、秒針，spitze尖端 & laenge長度 皆為德文
    fd(laenge*1.15)  #fd(distance) 【distance=距離】【fd=forward】
    rt(90) #rt(angle) 【angle=角度】
    fd(spitze/2.0)
    lt(120)
    fd(spitze)
    lt(120)
    fd(spitze)
    lt(120)
    fd(spitze/2.0)

def make_hand_shape(name, laenge, spitze):
    reset()
    jump(-laenge*0.15)
    begin_poly()
    #Start recording the vertices of a polygon. Current turtle position is first vertex of polygon.
    #開始記錄一個多邊形的頂點。當前turtle的位置是多邊形的第一個頂點。
    hand(laenge, spitze)
    end_poly()
    #Stop recording the vertices of a polygon. Current turtle position is last vertex of polygon. This will be connected with the first vertex.
    #結束記錄一個多邊形的頂點。當前turtle的位置是多邊形的最後一個頂點，最後一個頂點會與地一個頂點相連接在一起。
    hand_form = get_poly()
    #Return the last recorded polygon.
    #回到最後一個紀錄的多邊錫。
    register_shape(name, hand_form) #register_shape(name,shape=None) name==>形狀名稱，如triangle，shape-->相對位置
    '''
    #1 name ==>可以是任何字符串  shape ==>必須為固定的標點座標(與name相對應的多邊形)
    #2 name ==> gif-file shape==> NONE
    #3 name ==>任何字符串  shape ==> (複合)的"形狀"物件(與name相對應的多邊複合形狀)
    #PS.複合形狀應該==>重疊形狀
    #"形狀物件"==> class turtle.Shape(type_,data) 
    type_	            data
 “polygon”	   a polygon-tuple 一個多邊形的元組, i.e. a tuple of pairs of coordinates 如交點座標
 “image”	   an image 一張照片 (in this form only used internally! 這種形式只能在內部使用)
“compound”	   None 沒有94狂(a compound shape has to be constructed using the addcomponent() method)

addcomponent(poly, fill, outline=None)  
poly==>多邊形，如交點座標
fill==>多邊形的顏色
outline==>多邊形的外框顏色;多邊形的輪廓顏色(如果有)
    '''
    
def clockface(radius):#radius=半徑
    reset()
    pensize(7) #筆跡大小
    for i in range(60): #for 迴圈 for變數 in 串列
        jump(radius) #回到jump()，距離=半徑
        if i % 5 == 0: #%=>求餘數
            fd(25)
            jump(-radius-25)
        else:
            dot(3) #turtle.dot(size=None, *color) #size==>an integer !!!size==>圓點的直徑
            jump(-radius)
        rt(6)#角度

def setup(): #三個指針的各種設定
    global second_hand, minute_hand, hour_hand, writer
    #global 是指定【變數】的可被看見的範圍，假設在函數內部使用 global x，代表這個 x 在函數外部，別的函數裡面都看得到。
    mode("logo") #預設為順時針
    #mode(standard)逆時針, mode(logo)順時針, mode(world)
    make_hand_shape("second_hand", 125, 25) #make_hand_shape(任何字符串,(交點座標))
    make_hand_shape("minute_hand",  130, 25)
    make_hand_shape("hour_hand", 90, 25)  
    clockface(160) #畫出clock的刻度
    second_hand = Turtle()
    second_hand.shape("second_hand")
    second_hand.color("gray20", "gray80")
    minute_hand = Turtle()
    minute_hand.shape("minute_hand")
    minute_hand.color("blue1", "red1")
    hour_hand = Turtle()
    hour_hand.shape("hour_hand")
    hour_hand.color("blue3", "red3")
    #除了clockface(160)以外，全部都在設定秒針、分針、時針的角度、顏色、外框...
    for hand in second_hand, minute_hand, hour_hand: #for變數 in 串列
        hand.resizemode("user")
        hand.shapesize(1, 1, 3)
        hand.speed(0)
    #再次設定長度、速度
    ht() #hide turtle 
    writer = Turtle()
    writer.ht()
    writer.pu()
    writer.bk(85)


def wochentag(t): #將星期設為一個數列，讓後面使用 【此處改編】加入中文
    wochentag = ["星期一 Monday", "星期二 Tuesday", "星期三 Wednesday",
        "星期四 Thursday", "星期五 Friday", "星期六 Saturday", "星期日 Sunday"]
    return wochentag[t.weekday()]

def datum(z): #將日期設為一個數列，讓後面使用 【此處改編】加入斜線
    monat = ["/ 1 /", "/ 2  /", "/ 3  /", "/ 4  /", "/ 5  /", "/ 6  /",
             "/ 7 /", "/ 8  /", "/ 9  /", "/ 10  /", "/ 11  /", "/ 12  /"]
    j = z.year
    m = monat[z.month - 1]
    t = z.day
    return "%d %s %d " % (j, m, t) #【此處改編】更改成台灣較熟悉的順序 年/月/日

def tick(): #很重要(主要控制指針旋轉的def)
    t = datetime.today() #偵測今日的日期
    sekunde = t.second + t.microsecond*0.000001  #秒鐘
    minute = t.minute + sekunde/60.0 #分鐘
    stunde = t.hour + minute/60.0 #時鐘
    try:
        tracer(False)  # Terminator can occur here
        writer.clear()
        writer.home()
        writer.forward(65)
        writer.write(wochentag(t),
                     align="center", font=("Courier", 14, "bold"))
        writer.back(150)
        writer.write(datum(t),
                     align="center", font=("Courier", 14, "bold"))
        writer.forward(85)
        writer.forward(220)
        writer.write("jyClock",
                     align="center", font=("Courier", 66, "bold"))
        writer.back(30)
        writer.write("【Remix by jy，orignial from python trutle demo】",
                     align="center", font=("Courier", 14, "bold"))
        #以上重製所有設定(包含星期)
        tracer(True)
        second_hand.setheading(6*sekunde)  # or here
        minute_hand.setheading(6*minute)
        hour_hand.setheading(30*stunde)
        tracer(True)
        ontimer(tick, 100)
    except Terminator:
        pass  # turtledemo user pressed STOP

def main():
    tracer(False)
    setup() #劃出clock的刻度+做好指針的設定
    #以上只是重製所有設定
    tracer(True)
    tick()
    return "EVENTLOOP"

if __name__ == "__main__":
    mode("logo") #非常重要的設定－順時針
    msg = main()
    print(msg)
    mainloop() 
    mainloop() 



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
