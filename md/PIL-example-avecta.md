---
title: PIL example avecta (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'avecta'

Functions in program: 
* `def levelsdump(fn,tg,tf,numbers=True):`
* `def leveldump(fn,count,d,tg,tf,numbers):   `
* `def floortile(d,tf):`
* `def hex(d):`
* `def unpack_txt(b):`
* `def unpack_4bit(g, tw, th, stride, rows):`
* `def palette_color(x):`

Modules used in program: 
* `import PIL.ImageDraw`
* `import PIL.ImageFont`
* `import PIL.Image`

## python avecta

Python PIL example: avecta

```python
# Avecta I: Ebora is an Atari ST game published in STart Magazine, September 1989
#
# Information here:
# https://www.atarimagazines.com/startv4n2/avecta.html
#
# This program parses its data files, and generates maps from it.
# The file formats can be deduced from the program.
# Some of the data is described in comments.

import PIL.Image
import PIL.ImageFont
import PIL.ImageDraw

info = True

prg = open("AVECTA.PRG","rb").read()
grafxdat = open("GRAFX.DAT","rb").read()
filldat = open("FILL.DAT","rb").read()

font = PIL.ImageFont.truetype("ProggyTiny.ttf",16)
# Font available here: https://proggyfonts.net/download/

palette = [
    0x000,
    0x700,
    0x007,
    0x520,
    0x560,
    0x050,
    0x741,
    0x444,
    0x770,
    0x077,
    0x641,
    0x111, #0x000,
    0x707,
    0x765,
    0x505,
    0x777,
    ]

pattern = []

pattern_color_remap = [
    0x0,0xF,0x2,0x3,
    0x4,0x6,0x6,0x5,
    0x7,0x9,0x9,0xB,
    0xC,0xD,0xE,0xD,
    ] # 1,4,5,7,8,A,C,F are known. 0,2,3,6,9,B,D,E are unused/unknown.

# fill patterns derived from GFA Basic DEFFILL
fill_patterns = [
    [0xFFFF,0xFFFF,0xFFFF,0xFFFF,0xFFFF,0xFFFF,0xFFFF,0xFFFF,0xFFFF,0xFFFF,0xFFFF,0xFFFF,0xFFFF,0xFFFF,0xFFFF,0xFFFF,], # 0
    [0xFFFF,0xBBBB,0xFFFF,0xEEEE,0xFFFF,0xBBBB,0xFFFF,0xEEEE,0xFFFF,0xBBBB,0xFFFF,0xEEEE,0xFFFF,0xBBBB,0xFFFF,0xEEEE,], # 1
    [0xFFFF,0xAAAA,0xFFFF,0xAAAA,0xFFFF,0xAAAA,0xFFFF,0xAAAA,0xFFFF,0xAAAA,0xFFFF,0xAAAA,0xFFFF,0xAAAA,0xFFFF,0xAAAA,], # 2
    [0x7777,0xAAAA,0xDDDD,0xAAAA,0x7777,0xAAAA,0xDDDD,0xAAAA,0x7777,0xAAAA,0xDDDD,0xAAAA,0x7777,0xAAAA,0xDDDD,0xAAAA,], # 3
    [0x5555,0xAAAA,0x5555,0xAAAA,0x5555,0xAAAA,0x5555,0xAAAA,0x5555,0xAAAA,0x5555,0xAAAA,0x5555,0xAAAA,0x5555,0xAAAA,], # 4
    [0x5555,0x2222,0x5555,0x8888,0x5555,0x2222,0x5555,0x8888,0x5555,0x2222,0x5555,0x8888,0x5555,0x2222,0x5555,0x8888,], # 5
    [0x5555,0x0000,0x5555,0x0000,0x5555,0x0000,0x5555,0x0000,0x5555,0x0000,0x5555,0x0000,0x5555,0x0000,0x5555,0x0000,], # 6
    [0x1111,0x0000,0x4444,0x0000,0x1111,0x0000,0x4444,0x0000,0x1111,0x0000,0x4444,0x0000,0x1111,0x0000,0x4444,0x0000,], # 7
    [0x0000,0x0000,0x0000,0x0000,0x0000,0x0000,0x0000,0x0000,0x0000,0x0000,0x0000,0x0000,0x0000,0x0000,0x0000,0x0000,], # 8
    [0x0000,0x7F7F,0x7F7F,0x7F7F,0x0000,0xF7F7,0xF7F7,0xF7F7,0x0000,0x7F7F,0x7F7F,0x7F7F,0x0000,0xF7F7,0xF7F7,0xF7F7,], # 9
    [0xDFDF,0xBFBF,0x7F7F,0xBEBE,0xDDDD,0xEBEB,0xF7F7,0xEFEF,0xDFDF,0xBFBF,0x7F7F,0xBEBE,0xDDDD,0xEBEB,0xF7F7,0xEFEF,], # 10
    [0xFFFF,0xFFFF,0xEFEF,0xD7D7,0xFFFF,0xFFFF,0xFEFE,0x7D7D,0xFFFF,0xFFFF,0xEFEF,0xD7D7,0xFFFF,0xFFFF,0xFEFE,0x7D7D,], # 11
    [0xFDFD,0xFDFD,0x5555,0xAFAF,0xDFDF,0xDFDF,0x5555,0xFAFA,0xFDFD,0xFDFD,0x5555,0xAFAF,0xDFDF,0xDFDF,0x5555,0xFAFA,], # 12
    [0xBFBF,0x7F7F,0xFFFF,0xF7F7,0xFBFB,0xFDFD,0xFFFF,0xDFDF,0xBFBF,0x7F7F,0xFFFF,0xF7F7,0xFBFB,0xFDFD,0xFFFF,0xDFDF,], # 13
    [0x99F9,0x3939,0x2727,0xE7E7,0x7E7E,0x724E,0xF3CC,0x9FFF,0x99F9,0x3939,0x2727,0xE7E7,0x7E7E,0x724E,0xF3CC,0x9FFF,], # 14
    [0xFFFF,0xFFFF,0xFBFF,0xFFFF,0xFFEF,0xFFFF,0x7FFF,0xFFFF,0xFFFF,0xFFFF,0xFBFF,0xFFFF,0xFFEF,0xFFFF,0x7FFF,0xFFFF,], # 15
    [0x0707,0x9393,0x3939,0x7070,0xE0E0,0xC9C9,0x9C9C,0x0E0E,0x0707,0x9393,0x3939,0x7070,0xE0E0,0xC9C9,0x9C9C,0x0E0E,], # 16
    [0x5555,0xFFFF,0x7777,0xEBEB,0xDDDD,0xBEBE,0x7777,0xFFFF,0x5555,0xFFFF,0x7777,0xEBEB,0xDDDD,0xBEBE,0x7777,0xFFFF,], # 17
    [0xF7F7,0xFFFF,0x5555,0xFFFF,0xF7F7,0xFFFF,0x7777,0xFFFF,0xF7F7,0xFFFF,0x5555,0xFFFF,0xF7F7,0xFFFF,0x7777,0xFFFF,], # 18
    [0x8888,0x6767,0x0707,0x0707,0x8888,0x7676,0x7070,0x7070,0x8888,0x6767,0x0707,0x0707,0x8888,0x7676,0x7070,0x7070,], # 19
    [0x7F7F,0x7F7F,0xBEBE,0xC1C1,0xF7F7,0xF7F7,0xEBEB,0x1C1C,0x7F7F,0x7F7F,0xBEBE,0xC1C1,0xF7F7,0xF7F7,0xEBEB,0x1C1C,], # 20
    [0x7E7E,0xBDBD,0xDBDB,0xE7E7,0xF9F9,0xFEFE,0x7F7F,0x7F7F,0x7E7E,0xBDBD,0xDBDB,0xE7E7,0xF9F9,0xFEFE,0x7F7F,0x7F7F,], # 21
    [0x0F0F,0x0F0F,0x0F0F,0x0F0F,0xF0F0,0xF0F0,0xF0F0,0xF0F0,0x0F0F,0x0F0F,0x0F0F,0x0F0F,0xF0F0,0xF0F0,0xF0F0,0xF0F0,], # 22
    [0xF7F7,0xE3E3,0xC1C1,0x8080,0x0000,0x8080,0xC1C1,0xE3E3,0xF7F7,0xE3E3,0xC1C1,0x8080,0x0000,0x8080,0xC1C1,0xE3E3,], # 23
    ]
 
for i in range(0,24):
    dst = PIL.Image.new("RGBA",(16,16),(255,255,255,0))
    dp = dst.load()
    for y in range(16):
        a = fill_patterns[i][y]
        for x in range(16):
            if ((a >> x) & 1) != 0:
                dp[x,y] = (0,0,0,255)
    pattern.append(dst)

def palette_color(x):
    p = palette[x]
    a = 0 if x == 0 else 255
    return (
        (((p>>8)&7) * 255) // 7,
        (((p>>4)&7) * 255) // 7,
        (((p>>0)&7) * 255) // 7,
        a )

def unpack_4bit(g, tw, th, stride, rows):
    def read16(x):
        return (g[x+0] << 8) | g[x+1]       
    cols = ((len(g) // stride) + rows - 1) // rows
    img = PIL.Image.new("RGB",(cols*tw,rows*th),(255,0,255))
    tiles = []
    for c in range(cols):
        for r in range(rows):
            od = ((c * rows) + r) * stride
            ox = c * tw
            oy = r * th
            tile = PIL.Image.new("RGBA",(tw,th),(255,0,255,0))
            pixels = tile.load()
            for y in range(th):
                for x in range(tw):
                    if (od + 8) >= len(g):
                        continue
                    p = 0
                    for plane in range(3,-1,-1):
                    #for plane in range(4):
                        m = read16(od+(plane*2))
                        p = (p << 1) | ((m >> (15-x)) & 1)
                    pixels[x,y] = palette_color(p)
                od = od + 8
            img.paste(tile,(ox,oy))
            tiles.append(tile)
    return (img, tiles)

def unpack_txt(b):
    s = ""
    addr = 0
    ss = ""
    def emit(x):
        e = ""
        if len(x) > 0:
            e += "%04X: " % addr
            e += ss + "\n"
        return e
    for i in range(len(b)):
        c = b[i]
        if c == 0:
            s += emit(ss)
            ss = ""
            addr = i+1
        else:
            pc = "*"
            if c >= 0x20 and c < 128:
                pc = chr(c)
            ss += pc
    s += emit(ss)
    return s

def hex(d):
    s = ""
    for v in d:
        s += " %02X" % v
    if len(s) > 0:
        s = s[1:]
    return s

def floortile(d,tf):
    if d[0] == 0xFF: # background tile
        return tf[d[1]]
    elif d[0] == 0x02: # pattern fill tile (colours are remapped instead of direct?)
        t = PIL.Image.new("RGBA",(16,16),palette_color(pattern_color_remap[d[2]]))
        p = pattern[d[1]]
        t.paste(p,(0,0),p)
        return t
    return tf[0] # ?

def leveldump(fn,count,d,tg,tf,numbers):   
    def read16(n):
        return (d[n+0] << 8) | d[n+1]
    # skip empty data
    if all(v==0 for v in d):
        return
    # not empty:
    iw = 256 if info else 16
    ih = 0 if info else 0
    img = PIL.Image.new("RGBA",(16*16+iw,8*16+ih),(0,0,0,0))
    # floor
    ft1 = floortile(d[0x10:0x13],tf)
    ft2 = floortile(d[0x13:0x16],tf)
    fl = [[0]*9 for x in range(16)]
    coord0 = d[0]
    c0 = coord0
    for i in range(1,16):
        # draw the walls (a cycle of nybble coordinates drawing straight lines)
        c1 = d[i]
        x = c0 & 15
        y = c0 >> 4
        xt = c1 & 15
        yt = c1 >> 4
        while (x != xt) or (y != yt):
            fl[x][y] = 1
            if x < xt:
                x += 1
            if x > xt:
                x -= 1
            if y < yt:
                y += 1
            if y > yt:
                y -= 1
        if (c1 == coord0):
            break # wall ends when it meets the starting coordinate
        c0 = c1
    for y in range(0,8):
        for x in range(0,16):
            # fill the interior
            if fl[x][y] != 0:
                continue
            x0c = 0
            x1c = 0
            y0c = 0
            y1c = 0
            for i in range(0,x):
                if fl[i][y] == 1:
                    x0c += 1
            for i in range(x+1,16):
                if fl[i][y] == 1:
                    x1c += 1
            for i in range(0,y):
                if fl[x][i] == 1:
                    y0c += 1
            for i in range(y+1,8):
                if fl[x][i] == 1:
                    y1c += 1
            if x0c < 1 or x1c < 1 or y0c < 1 or y1c < 1:
                continue
            fl[x][y] = 2
    for y in range(0,8):
        # draw floor
        for x in range(0,16):
            if fl[x][y] == 2:
                img.paste(ft2,(x*16,y*16))
            if fl[x][y] == 1:
                img.paste(ft1,(x*16,y*16))
    # items
    color = [
        # every item has 9 data bytes:
        # 0 - tile
        # 1 - ?
        # 2 - door text?
        # 3 - exit room number
        # 4 - book/sign text?
        # 5 - FF solid, FE locked, 01 filled, 00 empty ?
        # 6 - X coordinate
        # 7 - Y coordinate
        # 8 - always 01 ?
        (255,255,  0),(255,255,255),(255,255,255),
        (  0,255,  0),(255,255,255),(255,  0,  0),
        (200,128,128),(128,200,128),(  0,255,255) ]
    draw = PIL.ImageDraw.Draw(img,"RGBA")
    for i in range(14):
        item = d[31+(i*9):31+(i*9)+9]
        if all(v==0 for v in item):
            continue
        it = item[0]
        ix = item[6]
        iy = item[7]
        img.paste(tg[it],(ix*16,iy*16),tg[it])
        if info:
            #s = ("%2d: " % i) + hex(item)
            #draw.text(((16*16)+8,(i*9)+1),s,font=font)
            draw.text(((16*16)+8,(i*9)+1),"%2d:"%i,font=font)
            for j in range(9):
                draw.text(((16*16)+26+(j*20),(i*9)+1),hex(item[j:j+1]),fill=color[j],font=font)
            if numbers:
                draw.rectangle((ix*16+2,iy*16+3,ix*16+13,iy*16+11),fill=(0,0,0,64))
                draw.text((ix*16+2,iy*16+3),"%2d"%i,font=font)
            if (item[8] == 0): # hidden
                draw.rectangle((ix*16,iy*16,ix*16+15,iy*16+15),outline=color[8])
    fn = fn + "." + ("%02X" % count)
    if info:
        ip = 16*16+iw-50
        draw.text((ip, 5),hex([count]),fill=color[3],font=font)
        # wall tile
        draw.text((ip,14),hex(d[0x10:0x13]),font=font,fill=(128,128,128))
        # floor tile
        draw.text((ip,23),hex(d[0x13:0x16]),font=font,fill=(128,128,128))
        # 9 more mystery bytes, the last one seems to indicate darkness
        # but the rest probably have to do with NPCs or enemies?
        draw.text((ip,32),hex(d[0x16:0x19]),font=font)
        draw.text((ip,41),hex(d[0x19:0x1C]),font=font)
        draw.text((ip,50),hex(d[0x1C:0x1F]),font=font)
    else:
        pos = (0,0)
        for x in range(16):
            for y in range(8):
                if fl[x][y] == 1:
                    if pos[0] < x:
                        pos = (x,y)
        if not numbers:
            pos = (pos[0]-1,pos[1]+1) # shift for world map which always has a black border
        draw.text(((pos[0]+1)*16+1,pos[1]*16+0),"%02X"%count,font=font,fill=(0,255,0))
    if info:
        img.save("info/" + fn + ".png")
    else:
        img.save("out/" + fn + ".png")
    print((fn + " ($%4X)" % (count * 157)))
    return

def levelsdump(fn,tg,tf,numbers=True):
    dat = open(fn,"rb").read()
    start = 0
    stride = 157 # each room is 157 bytes
    extra = 0
    count = 0
    while (start + stride + extra) <= len(dat):
        leveldump(fn,count,dat[start:start+stride+extra],tg,tf,numbers)
        count += 1
        start += stride
        if count >= 0x50:
            # there is extra data at the end of the file, unknown purpose
            break
    return

palimg = PIL.Image.new("RGB",(128,128))
paldrw = PIL.ImageDraw.Draw(palimg,"RGBA")
for y in range(0,4):
    for x in range(0,4):
        ci = x+(y*4)
        c = palette_color(ci)
        paldrw.rectangle((x*32,y*32,x*32+31,y*32+31),fill=c)
        paldrw.rectangle((x*32,y*32,x*32+ 7,y*32+ 8),fill=(0,0,0,128))
        paldrw.text((x*32+1,y*32-1),"%X"%ci)
palimg.save("palette.png")

(grafxpng, grafxt) = unpack_4bit(grafxdat,16,16,(8*16)+2,16)
(fillpng, fillt) = unpack_4bit(filldat, 16,16,(8*16)+2,16)
grafxpng.save("GRAFX.DAT.png")
fillpng.save("FILL.DAT.png")
open("AVECTA.PRG.txt","wt").write(unpack_txt(prg))

levelsdump("CAVE.DAT",grafxt,fillt)
levelsdump("TROND.DAT",grafxt,fillt)
levelsdump("DEMON.DAT",grafxt,fillt)
# These three seem to differ only by their "mystery" bytes.
# Not sure when they are used by the game.
# Presumably they alter which characers/NPCs appear in various rooms.
#levelsdump("DCAVE.DAT",grafxt,fillt)
#levelsdump("DTROND.DAT",grafxt,fillt)
#levelsdump("DDEMON.DAT",grafxt,fillt)
levelsdump("OUTSIDE.DAT",fillt[20:],fillt,numbers=False)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
