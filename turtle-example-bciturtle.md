---
title: turtle example bciturtle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'bciturtle'

Functions in program: 
* `def tiltedCircle(tccenter, tcstart, tcangle, tchrad, tcvrad, tctilt, tcsteps, tcfallbackx = 0, tcfallbacky = 0, begfill = 0, dopd = 0, mts = 0, sts = (1,1,1), ets = (1,1,1), tspeed = 0):`

## python bciturtle

Python turtle example: bciturtle

```python
from turtle import*
from math import*


def tiltedCircle(tccenter, tcstart, tcangle, tchrad, tcvrad, tctilt, tcsteps, tcfallbackx = 0, tcfallbacky = 0, begfill = 0, dopd = 0, mts = 0, sts = (1,1,1), ets = (1,1,1), tspeed = 0):
    if tchrad - tcfallbackx < 0:
        tcfallbackx = tchrad
    if tcvrad - tcfallbacky < 0:
        tcfallbacky = tcvrad
    tcanglestep = tcangle/tcsteps
    tcstep = 0
    tcmaxy, tcmaxy = tccenter
    tcmaxy = tcmaxy + abs(tcvrad)
    tcminy, tcminy = tccenter
    tcminy = tcminy - abs(tcvrad)
    while tcstep <= tcsteps:
        tccurangle = tcstart + tcstep * tcanglestep
        tccurx, tccury = tccenter
        tccurx = tccurx + (tchrad - tcfallbackx) * cos(radians(tccurangle))
        tccury = tccury + (tcvrad - tcfallbacky) * sin(radians(tccurangle))
        tccurypct = (tccury - tcminy) / (tcmaxy - tcminy)
        tccurypct = 2 * tccurypct - 1
        tccurx = tccurx + 1 * tcvrad * tccurypct * tan(radians(tctilt))
        tccurpos = (tccurx, tccury)
        seth(towards(tccurpos))
        setpos(tccurpos)
        tcstep = tcstep + 1
        speed(tspeed)
        if mts:
            stsw, stsl, stso = sts
            etsw, etsl, etso = ets
            turtlesize(stsw + (etsw - stsw) * tcstep/tcsteps, stsl + (etsl - stsl) * tcstep/tcsteps, stso + (etso - stso) * tcstep/tcsteps)
        if begfill == 1:
            begfill = 0
            begin_fill()
        if dopd == 1:
            dopd == 0
            pd()
    speed(baseSpeed)



lineWidth = 1
sizeMult = 2
strokeWidthV = 12.5 * sizeMult
strokeWidthH = 15 * sizeMult
bWidth = 2.6
cWidth = 3
iSpacing = 0.1
iWidth = 1 + iSpacing * 2
tWidth = 3
oWidth = 3
totalHeight = strokeWidthV * 4.25
totalWidth = (bWidth + cWidth + iWidth + tWidth) * strokeWidthH
tiltAngleDegree = 15
tiltAngle = radians(tiltAngleDegree)
tiltCos = cos(tiltAngle)
tiltSin = sin(tiltAngle)
tiltTan = tan(tiltAngle)
tiltmod = totalHeight * tiltTan
angleHeight = totalHeight / tiltCos
baseSpeed = 1;
width(lineWidth)
mainColor = '#00477F'
bgColor = 'white'
orbitColor = '#F4821F'

totalHeight = max(totalHeight, strokeWidthV * 3)
lineWidth = max (lineWidth, 1)
speed(baseSpeed)
pu()

drawB = 1
drawC = 1
drawI = 1
drawT = 1
drawL = 1
drawO = 1
drawR = 1


# ---------- B ----------
if drawB == 1:
    bRad = totalHeight / 2
    bDist = tiltTan * totalHeight / 2
    bHMod = [0.1, 0.1]
    bLCM = 1.0
    bLD = 0.1
    bMMod = bWidth - 2
    bMTMod = max(bHMod[0], bHMod[1] + bLD)
    if bMTMod > bMMod:
        print("Tryint to create a B wider than the width of B")
        bHMod = [bHMod[0]/bMTMod, bHMod[1]/bMTMod]
        bLD = bLD / bMTMod
    bEWidth = bWidth * strokeWidthH - (max(bHMod[0],bHMod[1]) + bLD) * strokeWidthH
    bTWSH = totalHeight - 3 * strokeWidthV #total white space height in the B
    bWSH = [bTWSH / (1 + bLCM), bTWSH * bLCM / (1 + bLCM)] #white space height in each of the B circles
    bVMod = [(2 * strokeWidthV + bWSH[0]) / totalHeight, (2 * strokeWidthV + bWSH[1]) / totalHeight]

#      ----- f -----
    color(mainColor)
    seth(90)
    setpos(-totalWidth / 2, -totalHeight/2)
    bblc = pos() #b bottom left corner
    pd()
    begin_fill()
    right(tiltAngleDegree)
    forward(angleHeight)
    btlc = pos() #b top left corner
    right(90 - tiltAngleDegree)
    forward(bEWidth - strokeWidthH * (1 + bHMod[0]))

    startPos = pos()
    btccu = startPos - (bDist * bVMod[0] - bHMod[0] * strokeWidthH, bRad * bVMod[0])
    tiltedCircle(btccu, 90, -180, strokeWidthH * (1 + bHMod[0]), bRad * bVMod[0], tiltAngleDegree, 20)
    btccl = startPos - (bDist * (2 - bVMod[1]) - bHMod[1] * strokeWidthH, bRad * (2 - bVMod[1]))
    pu()
    seth(180)
    right(90 + tiltAngleDegree)
    forward(strokeWidthV)
    pd()
    bld = bLD * strokeWidthH
    btccl = btccl + (bld, 0)
    tiltedCircle(btccl, 90, -180, strokeWidthH * (1 + bHMod[1]), bRad * bVMod[1], tiltAngleDegree, 20)
    seth(180)
    forward(bEWidth + strokeWidthH * (-1 + bHMod[1] - bHMod[0]) + bld)
    
    end_fill()
    pu()

#      ----- w -----
    color(bgColor)
    seth(90)
    buwsp = btlc + (strokeWidthH - strokeWidthV * tiltTan - lineWidth * 2, - strokeWidthV - lineWidth * 2)
    setpos(buwsp)
    pd()
    begin_fill()

    tiltedCircle(btccu, 90, -180, strokeWidthH * (1 + bHMod[0]), bRad * bVMod[0], tiltAngleDegree, 20, strokeWidthH + lineWidth * 2, strokeWidthV + lineWidth * 2)
    seth(180)
    setpos(buwsp - (bWSH[0] * tiltTan, bWSH[0] - lineWidth * 4))
    right(90 + tiltAngleDegree)
    setpos(buwsp)
    end_fill()
    pu()

    seth(90)
    blwsp = bblc + (strokeWidthH + strokeWidthV * tiltTan - lineWidth * 2, strokeWidthV + lineWidth * 2)
    blwsp = blwsp + (bWSH[1] * tiltTan, bWSH[1] - lineWidth * 4)
    setpos(blwsp)
    pd()
    begin_fill()

    tiltedCircle(btccl, 90, -180, strokeWidthH * (1 + bHMod[1]), bRad * bVMod[1], tiltAngleDegree, 20, strokeWidthH + lineWidth * 2, strokeWidthV + lineWidth * 2)
    setpos(blwsp - (bWSH[1] * tiltTan, bWSH[1] - lineWidth * 4))
    setpos(blwsp)
        
    end_fill()
    pu()


# ---------- C ----------
cOpenAngle = 60
if drawC == 1:
    cOpenAngle = max(0, min(180, cOpenAngle))
    cStartAngle = cOpenAngle/2
    cTotalAngle = 360 - cOpenAngle

#      ----- f -----
    color(mainColor)
    seth(90)
    
    ctccx = -totalWidth / 2 + (bWidth + cWidth / 2) * strokeWidthH + totalHeight * tiltTan / 2
    ctccy = 0
    ctcc = (ctccx, ctccy)
    
    ccorh = cWidth * strokeWidthH / 2
    ccorv = totalHeight / 2
    setpos(ctcc)
    tiltedCircle(ctcc, cStartAngle, cTotalAngle, ccorh, ccorv, tiltAngleDegree, 40, strokeWidthH, strokeWidthV, 1, 1)
    cciex, cciey = pos()
    ccosa = degrees(asin((cciey - ctccy) / (ccorv)))
    ccota = 360 - abs(ccosa) * 2
    seth(0)

    tiltedCircle(ctcc, ccosa, -ccota, ccorh, ccorv, tiltAngleDegree, 40)

    seth(180)
    tiltedCircle(ctcc, cStartAngle, 0, ccorh, ccorv, tiltAngleDegree, 1, strokeWidthH, strokeWidthV)

    end_fill()
    pu()


# ---------- I ----------
if drawI == 1:

#      ----- f -----
    color(mainColor)
    seth(90)
    setpos(-totalWidth / 2 + (bWidth + cWidth + iSpacing) * strokeWidthH, -totalHeight/2)
    pd()
    begin_fill()
    right(tiltAngleDegree)
    forward(angleHeight)
    right(90 - tiltAngleDegree)
    forward((iWidth - iSpacing * 2) * strokeWidthH)
    right(90 + tiltAngleDegree)
    forward(angleHeight)
    right(90 - tiltAngleDegree)
    forward((iWidth - iSpacing * 2) * strokeWidthH)
    end_fill()
    pu()


# ---------- T ----------
if drawT == 1:
    ttoph = totalHeight / 2 - (strokeWidthV * sin(radians(cOpenAngle / 2)))
    tbasew = 1
    ttopd = (tWidth - tbasew) * strokeWidthH / 2
#      ----- f -----
    color(mainColor)
    seth(90)
    setpos(-totalWidth / 2 + (bWidth + cWidth + iWidth) * strokeWidthH + ttopd, -totalHeight/2)
    pd()
    begin_fill()
    right(tiltAngleDegree)
    forward((totalHeight - ttoph) / tiltCos)
    left(90 + tiltAngleDegree)
    forward(ttopd)
    right(90 + tiltAngleDegree)
    forward(ttoph / tiltCos)
    right(90 - tiltAngleDegree)
    forward(tWidth * strokeWidthH)
    right(90 + tiltAngleDegree)
    forward(ttoph / tiltCos)
    right(90 - tiltAngleDegree)
    forward(ttopd)
    left(90 - tiltAngleDegree)
    forward((totalHeight - ttoph) / tiltCos)
    right(90 - tiltAngleDegree)
    forward(tbasew * strokeWidthH)
    end_fill()
    pu()


# ---------- L ----------
if drawL == 1:
    totalhlines = 7
    hlines = 0
    linestep1 = 1.5
    linestep2 = 1
    linestep3 = 0.5
    linestep4 = 1
    totalsteps = totalhlines * (linestep1 + linestep2 + linestep3 + linestep4) + linestep1
    stepheight = totalHeight / totalsteps
    linewidth = (totalWidth) * 1.5 + tiltmod
    
#     ----- f -----
    color(bgColor)
    seth(90)
    speed(5)
    setpos((linewidth + tiltmod) / 2, totalHeight/2)
    while hlines < totalhlines:
        pd()
        begin_fill()
        seth(-90)
        forward(stepheight * linestep1)
        seth(towards(pos() - (linewidth, stepheight * linestep2)))
        setpos(pos() - (linewidth, stepheight * linestep2))
        seth(-90)
        forward(stepheight * linestep3)
        seth(towards(pos() + (linewidth, - stepheight * linestep4)))
        setpos(pos() + (linewidth, - stepheight * linestep4))
        hlines = hlines + 1
        end_fill()
    pu()
    speed(baseSpeed)
    
    

# ---------- R ----------
if drawR == 1:
    color(mainColor)
    width(sizeMult * lineWidth)
    seth(90)
    rCR = strokeWidthV / 2
    setpos(totalWidth / 2 + totalHeight * tiltTan + 1.5 * rCR, totalHeight/2)
    pd()
    tiltedCircle(pos()-(0,rCR), 90, -360, rCR, rCR, 0, 20)
    pu()
    seth(90)
    setpos(pos() - (rCR/2, 1.5 * rCR))
    pd()
    forward(rCR)
    right(90)
    forward(rCR * 0.75)
    tiltedCircle(pos()-(0,rCR/4), 90, -180, rCR/4, rCR/4, 0, 5)
    seth(180)
    forward(rCR * 0.75)
    seth(0)
    pu()
    forward(rCR * 0.5)
    pd()
    right(45)
    forward(rCR * cos(radians(45)))

    pu()


# ---------- O ----------
if drawO == 1:
    color(orbitColor)
    width(sizeMult * lineWidth)
    ocenter = (- tiltmod * 0.75, -totalHeight/2 + strokeWidthV)
    oas = 85
    oat = 275
    oWidth = totalWidth * 0.65
    otilt = tiltAngleDegree - 75
    startsize = (1,1,1)
    ospeed = 5
    endsize = (sizeMult, sizeMult * 1.5, sizeMult * lineWidth)
    tiltedCircle(ocenter, oas, oat, oWidth, totalHeight - strokeWidthV, otilt, 40, 0, 0, 0, 1, 1, startsize, endsize, ospeed)
    pu()
        

done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
