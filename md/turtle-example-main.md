---
title: turtle example main (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'main'

Functions in program: 
* `def rollback() :`
* `def dxdiag():`
* `def update(xt=1) :  `
* `def winver() :`
* `def taskman() :`
* `def main(p=0):`
* `def bootui(a) :`

## python main

Python turtle example: main

```python
from ion import *
from time import *
from math import *
from kandinsky import *
from random import *
from turtle import *
b = color(255,255,255)
stat = "Genuine"
#Microsoft Windows 10
def bootui(a) :
  if a == 1 :
    fill_rect(0,0,640,480,(0,0,0))
    fill_rect(120,50,40,40,(128,128,255))
    fill_rect(120,93,40,40,(128,128,255))
    fill_rect(163,40,50,50,(128,128,255))
    fill_rect(163,93,50,50,(128,128,255))  
    for i in range(10):  
      set_pixel(160,180,(255,255,255))
      set_pixel(161,180,(255,255,255))
      set_pixel(160,181,(255,255,255))
      set_pixel(161,181,(255,255,255))
      sleep(0.1)
      set_pixel(166,186,(255,255,255))
      set_pixel(167,186,(255,255,255))
      set_pixel(166,187,(255,255,255))
      set_pixel(167,187,(255,255,255))
      sleep(0.1)
      set_pixel(161,191,(255,255,255))
      set_pixel(160,191,(255,255,255))
      set_pixel(161,192,(255,255,255))
      set_pixel(160,192,(255,255,255))  
      sleep(0.1)
      set_pixel(157,187,(255,255,255))
      set_pixel(156,186,(255,255,255))
      set_pixel(156,187,(255,255,255))
      set_pixel(157,186,(255,255,255))
      if keydown(KEY_FIVE):
        break
      sleep(0.1)
      fill_rect(150,165,40,40,(0,0,0))
      sleep(0.5)
    print("Welcome to Windows 10 Home")  
    print("WARNING : When you quit an app, \npress exit")
  else : 
    print("sorry an error occured, \nor you are already in windows")
task = ["omega.exe","msedge.exe","MsMpEng.exe","winlogon.exe","registry","explorer.exe","minecraft.exe","iexplore.exe","MicrosoftEdge.exe","MEMZ.BAT","WIN.COM","Avira.exe","winword.exe","taskman.exe","notepad.exe","battlenet.exe","discord.exe","cmd.exe","000.exe","MEMZ-DESTRUCTIVE.BAT","starcraft.exe","javaw.exe","system","regedit","command.com","edit.com","scandisk.com"]
def main(p=0):
  fill_rect(0,0,640,480,(0,0,255))
  draw_string(a,17,5,(255,255,255),(0,0,255))
  draw_string(b,17,25,(255,255,255),(0,0,255))
  draw_string(c,17,45,(255,255,255),(0,0,255))
  draw_string(d,17,65,(255,255,255),(0,0,255))
  draw_string(e,17,85,(255,255,255),(0,0,255))
  draw_string(f,17,105,(255,255,255),(0,0,255))
  draw_string(g,17,125,(255,255,255),(0,0,255))
  draw_string(h,17,145,(255,255,255),(0,0,255))
  draw_string(i,17,165,(255,255,255),(0,0,255))
  draw_string(j,17,185,(255,255,255),(0,0,255))
  if p == 1:
    draw_string(w,17,205,(255,255,255),(0,0,255))
  else:
    draw_string(k,17,205,(255,255,255),(0,0,255))
  sleep(5)
a = "----  -------   ---      ---"
b = "----  --------  ----    ----"
c = " --    --   --   ----  ---- "
d = " --    -------    --------  "
e = " --    -------   -- ---- -- "
f = " --    --   --   --  --  -- "
g = "----  --------  ---      ---"
h = "----  -------   ---      ---"
i = "                            "
j = "        IBM SOFTWARE        "
k = "    IBM ThinkPad Software   "
w = "      IBM OS/2 warp 4       "
def taskman() :
  ta = choice(task)
  tb = choice(task)
  tc = choice(task)
  td = choice(task)
  te = choice(task)
  tf = choice(task)
  tg = choice(task)
  th = choice(task)
  ti = choice(task)
  tj = choice(task)
  print("Task Manager - detail")
  print(ta,"\n",tb,"\n",tc,"\n",td,"\n",te,"\n",tf,"\n",tg,"\n",th,"\n",ti,"\n",tj,"\n")
manaf = "Microsoft Windows 10"
versi = "Home N"
build = "19564.3467"
activ = stat
def winver() :
  print("About Windows")
  print(manaf,"\n",versi,"\n",build,"\n","Windows is ",activ)
def update(xt=1) :  
    fill_rect(0,0,640,480,(128,128,255))
    draw_string("Working on Windows update",60,100,(0,0,0),(128,128,255))
    draw_string("Dont turn off your pc",100,140,(0,0,0),(128,128,255))
    ul = int()
    up = int()
    while ul <= 100:
      ulc = str(ul)
      draw_string(ulc,90,125,(0,0,0),(128,128,255))
      draw_string("%",120,125,(0,0,0),(128,128,255))
      fill_rect(130,125,200,15,(128,128,255))
      up = randint(0,1)
      ul = ul + up
      sleep(xt)
    print("Sucessfully upgraded Windows 10")
    from sp1 import *
def dxdiag():
  while True :
    fill_rect(0,0,640,480,(0,0,255))
    draw_string("DirectX Diagnostic tool",5,5,(255,255,255),(0,0,255))
    draw_string("1\ CPU informations",5,25,(255,255,255),(0,0,255))
    draw_string("2\ graphics informations",5,45,(255,255,255),(0,0,255))
    draw_string("3\ system informations",5,65,(255,255,255),(0,0,255))
    draw_string("4\ system test",5,85,(255,255,255),(0,0,255))
    draw_string("5\ exit dxdiag",5,105,(255,255,255),(0,0,255))
    draw_string("select your action : 1/2/3/4/5",5,125,(255,255,255),(0,0,255))
    sleep(0.025)
    if keydown(KEY_ONE):
      draw_string("CPU : Intel Core i9 9900k",5,155,(255,255,255),(0,0,255))
      draw_string("VT-x Enabled, 8 core 16 threads",5,175,(255,255,255),(0,0,255))
      draw_string("base : 3.7 GHz, boost : 6.9-GHz",5,195,(255,255,255),(0,0,255))
      while True:
        if keydown(KEY_OK):
          break
    if keydown(KEY_TWO):
      draw_string("GPU : NVidia GeForce GTX 2080",5,155,(255,255,255),(0,0,255))
      draw_string("16 Go GDDR6, DirectX 10",5,175,(255,255,255),(0,0,255))
      draw_string("OpenGL 4.5, 1920x1080 144Hz",5,195,(255,255,255),(0,0,255))
      while True:
        if keydown(KEY_OK):
          break
    if keydown(KEY_THREE):
      draw_string("RAM : 16384 Mo installed",5,155,(255,255,255),(0,0,255))
      draw_string("Serial-ATA Disk",5,175,(255,255,255),(0,0,255))
      draw_string("Solid State Drive of 2048 Go",5,195,(255,255,255),(0,0,255))
      while True:
        if keydown(KEY_OK):
          break
    if keydown(KEY_FOUR):
      fill_rect(0,0,640,480,(0,0,255))
      draw_string("Running performance test",5,5,(255,255,255),(0,0,255))
      draw_string("Press EXE to continue...",5,25,(255,255,255),(0,0,255))
      while True:
        if keydown(KEY_EXE):
          break
      fill_rect(0,0,640,480,(0,0,0))
      sleep(0.5)
      fill_rect(0,0,640,480,(255,0,0))
      sleep(0.5)
      fill_rect(0,0,640,480,(0,255,0))
      sleep(0.5)
      fill_rect(0,0,640,480,(0,0,255))
      sleep(0.5)
      fill_rect(0,0,640,480,(255,255,255))
      sleep(0.5)
      fill_rect(0,0,640,480,(0,0,255))
      draw_string("Screen test - PASSED",5,5,(255,255,255),(0,0,255))
      draw_string("Press OK to continue",5,205,(255,255,255),(0,0,255))
      while True:
        if keydown(KEY_OK):
          break
    if keydown(KEY_FIVE):  
      break
def rollback() :
  print("nothing to restore")
bootui(1)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
