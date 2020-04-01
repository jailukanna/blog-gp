---
title: socket example Smtas%2520sockets%2520with%2520graph%252011 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'Smtas%2520sockets%2520with%2520graph%252011'

Functions in program: 
* `def exitApp(sender):`
* `def shiftTemps():    #and free last element`
* `def disconnect(sender):`
* `def connect(sender):`
* `def stopRead(sender):`
* `def startRead(sender):`
* `def msg(msgStr):`
* `def readSmtasOnce():`
* `def getDateAndTime():`
* `def update(sender):`
* `def makeTheGraph(self): `

Modules used in program: 
* `import ui`
* `import array`
* `import ImageDraw`
* `import Image`
* `import random`
* `import select`
* `import os`
* `import time`
* `import datetime`
* `import sys`

## python Smtas%2520sockets%2520with%2520graph%252011

Python socket example: Smtas%2520sockets%2520with%2520graph%252011

```python

import sys
from console import hud_alert
import datetime
import time
import os
#import console
from socket import *
import select
import random
import Image
import ImageDraw
import array
import ui


temp=[]
for i in range(4):
  temp.append([]) # create 4 arrays
bufferLen=4
noOfTemps=0
offset=[16,17,18,19]
for i in range(0):
  for j in range(4):
    temp[j].append(offset[j]+random.random())
  noOfTemps=noOfTemps+1
    
run =True 
viewHeight=400
viewWidth=600
run=True
reading =False
connected=False
msgSet=False
i=0
tMsg=0
interval=5  #time interval between requests
theTime=''
theDate=''
noMeasurements=0
#host = '192.168.1.13'
#host='86.193.161.111'
#host='raspberry.smartec.fr'
port = 13000
buf = 1024
#addr = (host,port)
UDPSock = socket(AF_INET, SOCK_DGRAM)
xMaxRect=40
yMaxRect=40
#t=array.array('f')
#for i in range(20):
#  t.append(75+random.random()*50)

#for i in range(t.buffer_info()[1]):
#   print(t[i])



class CustomViewGraph():       
  tColor=('red','green','yellow','blue')     
  borderColor='red'
  backgroundColor=0.40, 0.80, 1.00
  axisColor='black'
  frameColor=1.00, 0.00, 0.5
  borderLineWidth=2
  frameLineWidth=2
  mainBorder=5
  graphBorder=50
  xOrigPix=mainBorder+graphBorder
  yOrigPix=viewHeight-mainBorder-graphBorder
  xAxisPix=viewWidth-2*mainBorder-2*graphBorder
  yAxisPix=viewHeight-2*mainBorder-2*graphBorder
  minYval=17
  maxYval=18
  tempInterval=maxYval-minYval
  noYticIntervals=10
  ticLen = 8
  textOffset=-45
  xStep=xAxisPix/bufferLen
  
  def drawGraphFrame(self):
    global viewWidth
    global viewHeight
    p=ui.Path.rect(self.mainBorder,self.mainBorder,viewWidth-2*self.mainBorder,viewHeight-2*self.mainBorder)
    ui.set_color(self.frameColor)
    p.line_width=self.frameLineWidth
    p.stroke()
    
  def drawAxes(self):   
 #   print(self.yAxisPix)
    p=ui.Path.rect(0,0,0,0)
    ui.set_color(self.axisColor)
    self.tempInterval=self.maxYval-self.minYval
    tempIntervalPerTic=float(self.tempInterval)/float(self.noYticIntervals)
  #  pixPerTic=(self.maxYval-self.minYval)/self.noYticIntervals
    pixPerTic=self.yAxisPix/self.noYticIntervals
    p.move_to(self.xOrigPix,self.yOrigPix)
    p.line_to(self.xOrigPix,self.yOrigPix-self.yAxisPix)
    p.move_to(self.xOrigPix,self.yOrigPix)
    p.line_to(self.xOrigPix+self.xAxisPix,self.yOrigPix)
    print((self.tempInterval,self.noYticIntervals,tempIntervalPerTic))
    for i in range(self.noYticIntervals+1):      
      p.move_to(self.xOrigPix,self.yOrigPix-(i)*pixPerTic)               
      p.line_to(self.xOrigPix+self.ticLen,self.yOrigPix-(i)*pixPerTic)
      l=ui.draw_string(str(self.minYval+tempIntervalPerTic*i),rect=(self.xOrigPix+self.textOffset,self.yOrigPix-(i)*pixPerTic-10,500,500))
    p.stroke()  
#    l=ui.Label

#    l.text('dd')
    
  def drawBackground(self):  
    print('this is drawbackgr')
    print((viewWidth,viewHeight))
    border=ui.Path.rect(0,0,viewWidth,viewHeight)
    border.line_width=self.borderLineWidth
    ui.set_color(self.borderColor)
 #   border.stroke()
    ui.set_color(self.backgroundColor)
    border.fill()   

  def plotTheData(self):
    self.drawBackground()   
#     self.drawGraphFrame()
    self.drawAxes()
    if noOfTemps>0:      
      for j in range(4):
        p=ui.Path.rect(0,0,0,0)
        p.line_width=1
        ui.set_color(self.tColor[j])
        t0Pix=self.yOrigPix-int((temp[j][0]-self.minYval)/self.tempInterval*self.yAxisPix)
        p.move_to(self.xOrigPix,t0Pix)
        for i in range(noOfTemps):
          t0Pix=self.yOrigPix-(temp[j][i]-self.minYval)/self.tempInterval*self.yAxisPix
          p.line_to(self.xOrigPix+(i)*self.xStep,t0Pix)      
        p.stroke()

class myGraphView(ui.View):#  created for custom view handling
  def __init__(self):
  # This will also be called without arguments when the view is loaded from a UI file.
  # You don't have to call super. Note that this is called *before* the attributes
  #defined in the UI file are set. Implement `did_load` to customize a view after
  # it's been fully loaded from a UI file.
    pass

      
  def will_close(self):
  # This will be called when a presented view is about to be dismissed.
  # You might want to save data here.
    pass

  def draw(self):
#    print('this is draw')
        # This will be called whenever the view's content needs to be drawn.
        # You can use any of the ui module's drawing functions here to render
        # content into the view's visible rectangle.
        # Do not call this method directly, instead, if you need your view
        # to redraw its content, call set_needs_display().            
    makeTheGraph(self)   
    
  def touch_began(self, touch):
    # Called when a touch begin
    print('you touched me')
#    t0[10]=25+random.random()*2
#    self.updateData90
    self.set_needs_display()
#    print(v.width,v.height)
  
  def touch_moved(self, touch):
    # Called when a touch moves.
 #   sys.exit()
    run=False   
    pass

  def touch_ended(self, touch):
    # Called when a touch ends.
    pass
 
def makeTheGraph(self): 
  g = CustomViewGraph()   
  g.plotTheData()



def update(sender):
  if connected:
    readSmtasOnce()
    print('this is update')
  else:
    msg('You must connect first')

def getDateAndTime():
  global theDate
  global theTime
  global second #for modulo  log intervals
  t = time.localtime()
  minute = t.tm_min
  second = t.tm_sec
  hour = t.tm_hour
  theTime=datetime.time(hour,minute,second).isoformat()
  dt=datetime.date.today()
  theDate=dt.isoformat()

def readSmtasOnce():
  global addr
  global theTime
  global data
  getDateAndTime()
#       global UDPSock
  UDPSock.sendto("X",addr)
  ready=select.select([UDPSock],[],[],2)
  while not ready[0]:
    UDPSock.sendto("X",addr) # retry
    ready=select.select([UDPSock],[],[],2)
    msg('Waiting for socket data')
  (data, addr) = UDPSock.recvfrom(buf)


def msg(msgStr):
  v['msg label'].text_color='red'
  v['msg label'].text='   '+msgStr
  global tMsg
  tMsg=os.times()[0]#snap time
  global msgSet
  msgSet=True

def startRead(sender):
  global reading
  if reading:
    msg('Already reading')
  else:
    if not connected:
      msg('You must connect first')
    else:
#      global reading
#      global interval
      reading=True
      if intervalTimeButton.selected_index==0: interval=2
      if intervalTimeButton.selected_index==1: interval=10
      if intervalTimeButton.selected_index==2: interval=30
      intervalTimeButton.enabled=False
      v['status read'].text=' Reading'

def stopRead(sender):
  global reading
  if not reading:
    msg('not reading')
  else:
    reading=False
    intervalTimeButton.enabled=True
    v['status read'].text=' Stopped'

def connect(sender):
  global connected
  if connected:
    msg('Already connected')
  else:
    if wanLanButton.selected_index == 0:
      host='raspberry.smartec.fr'
    else:
      host = '192.168.1.13'
    msg('Connecting')
    global addr
    addr = (host,port)
    UDPSock = socket(AF_INET, SOCK_DGRAM)

    v['status connect'].text=' Connected'
    v['host'].text=host
    wanLanButton.enabled=False
    connected=True

def disconnect(sender):
  global connected
  if not connected:
    msg('Not connected')
  else:
    if reading:
      msg('You must stop first')
    else:
      msg('Disconnecting')
#   UDPSock.close()
      v['status connect'].text='Disconnected'
      wanLanButton.enabled=True
      connected=False
      v['host'].text='X'

def shiftTemps():    #and free last element
  for j in range(4):
    for i in range(bufferLen-1):
      temp[j][i]=temp[j][i+1]    
 

def exitApp(sender):
  global run
  run=False
  v.close()
  UDPSock.close()
#   console.clear()

v = ui.load_view('Smtas sockets with graph 10')
w = v.add_subview(ui.View('myGraphView'))
v.background_color=1.00, 0.80, 0.40
v.border_width=4
v.border_color=0.00, 0.00, 0.00
v.corner_radius=6
v['status connect'].text='X'
v.present(style='popover',hide_title_bar=True   )
#v.present(style='sheet',hide_title_bar=True   )
#v.present(style='full_screen',hide_title_bar=True   )
#v.present(style='panel',hide_title_bar=True   )
#ui.View.add_subview(v['ytt'])
wanLanButton = v['accessStyle']
intervalTimeButton= v['intervalTime']
#wanLanButton.action=wanlanAction #not defined

while (run):  #run==false means exit programm
  time.sleep(1)
  getDateAndTime()
  if ((os.times()[0]-tMsg)>0.03) and msgSet: #clear msg label after a while
    v['msg label'].text=' '
    msgSet=False
  v['time label'].text='      Time: '+theTime
  v['date label'].text='      Date: '+theDate
  if reading:
    if second%interval==0:
      noOfTemps=noOfTemps+1
      readSmtasOnce()
#      noMeasurements=noMeasurements+1
      v['no measurements'].text='     # of values:  '+str(noOfTemps)
      data=data.rstrip('\r\n')
#           v['data label'].text='   '+data+'    ' + theTime
      temps=data.split()
 #     print(temp[0])
      if noOfTemps<bufferLen+5:
        for i in range(4):
          temp[i].append(float(temps[i]))
      else:
        shiftTemps()
        for i in range(4):
      #    temp[i][bufferLen-1]=float(temps[i])
          pass
      t0=float(temps[0])
      t1=float(temps[1])
      t2=float(temps[2])
      t3=float(temps[3])
#           print(t1,'+++',t2)
      v['temp0'].text='   '+str(t1)+' ''C'
      v['temp1'].text='   '+str(t0)+' ''C'
      v['temp2'].text='   '+str(t2)+' ''C'
      v['temp3'].text='   '+str(t3)+' ''C'
#           print('try to call set needs display here')
      xMaxRect=30+int(random.random()*10)
      v.set_needs_display()
      
#***********************************

# coding: utf-8





#while run:
#  time.sleep(1)
#  t0New=offset[0]+random.random()*2  
#  updateData()
             
             




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
