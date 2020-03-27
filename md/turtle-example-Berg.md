---
title: turtle example Berg (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Berg'


## python Berg

Python turtle example: Berg

```python
#import
from turtle import*

#Vorbereitung
title ("Berg") 
speed (10)
penup ()
bgcolor ("blue")
backward (500)
right (90)
forward (400)
left (160)


#Programm
#Berg1
pendown ()
color ("darkgrey")
begin_fill ()
forward (500)
right (70)
forward (130)
right (80)
forward (500)
end_fill ()

backward (500)
right (150)
color ("white")
begin_fill ()
forward (85)
left (90)
forward (100)
end_fill ()

#Berg2
penup ()
home ()
right (140)
pendown ()
color ("darkgrey")
begin_fill ()
forward (300)
backward (300)
left (140)
pendown ()
forward (100)
right (80)
forward (400)
right (90)
forward (300)
right (80)
forward (300)
end_fill ()

color ("white")
penup ()
home ()
pendown ()
begin_fill ()
left (30)
forward (85)
right (90)
forward (50)
end_fill ()

#Berg 3
begin_fill ()
left (90)
forward (300)
right (90)
forward (190)
home ()
end_fill ()

color ("darkgrey")
penup ()
begin_fill ()
forward (100)
right (80)
pendown ()
forward (400)
left (80)
forward (300)
left (90)
forward (389)
left (90)
forward (450)
end_fill ()

#Mond
penup ()
home ()
left (90)
forward (200)
color ('grey')
begin_fill ()
circle (40)
end_fill ()
left (55)
forward (50)
color ('blue')
begin_fill ()
circle (30)
end_fill ()
hideturtle ()






```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
