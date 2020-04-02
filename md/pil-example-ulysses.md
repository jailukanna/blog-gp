---
title: pil example ulysses (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'ulysses'

Functions in program: 
* `def getPunctuation(s):`

Modules used in program: 
* `import re`
* `import matplotlib.image as mpimg`
* `import matplotlib.pyplot as plt`
* `import sys`
* `import math`
* `import string`

## python ulysses

Python pil example: ulysses

```python
#built on the brilliant work by Adam J Calhourn from his post
#https://medium.com/@neuroecology/punctuation-in-novels-8f316d542ec4#.tz6i4h1ub

!pip install beautifulsoup4

import string
import math
import sys
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw
import matplotlib.pyplot as plt
import matplotlib.image as mpimg

from bs4 import BeautifulSoup as bs
from urllib.request import urlopen
import re


## VARIABLES
# size of output canvas in pixels
#moved to dynamically size after scraping
#canvasHeight = 8000;
#canvasWidth = 8000;

# pixel border width
trim = 100;

font1size = 48;
font2size = 72;



#web scrape
var = input("Enter a page: ")

page = urlopen(var).read()

soup = bs(page,'html.parser')
title = soup.title.string
title = title.strip()

soup = soup.find("div", class_="articleContent")
content = soup.get_text()



#original code for reading a file

#bookname = 'ulysses'

#file = open('/Users/Ian/Documents/'+bookname + '.txt','r')
#txt = file.read()
#file.close()

bookname = title
txt = content

include = set(string.punctuation)

def getPunctuation(s):
   return ''.join(ch for ch in s if ch in include)

punct = getPunctuation(txt);

# file = open('blood-punct.txt','w')
file = open(bookname + '-punct.txt','w')
file.write(punct)
file.close()

# number of symbols to be output on each line
# and the number of lines

size = int(math.floor(math.sqrt(len(punct))))

symbolsPerLine = size
#linesOfText = int(math.floor(len(punct)/symbolsPerLine));
linesOfText = size


canvasHeight = size * 100
canvasWidth = size * 100

deltaW = (canvasWidth - trim*2)/symbolsPerLine
deltaH = (canvasHeight - trim*2)/linesOfText

bkgColor = (238,212,187)
bkgColor = (255,255,255)

img = Image.new("RGB", [canvasWidth,canvasHeight], bkgColor)
draw = ImageDraw.Draw(img)
# font from (SEE LICENSE!): http://www.fontsquirrel.com/fonts/glacial-indifference
font1 = ImageFont.truetype("GlacialIndifference-Bold.otf", font1size)
font2 = ImageFont.truetype("GlacialIndifference-Bold.otf", font2size)

# transitionFill = (0,0,0);
# endSentenceFill = (125,0,0);
# parentheticalFill = (235,235,235);

# in case you want to change by transition
transitionFill = (0,0,0);
endSentenceFill = (0,0,0);
parentheticalFill = (0,0,0);

# getTextSize
for ii in range(linesOfText):
   for jj in range(symbolsPerLine):
      symb = punct[jj + ii*symbolsPerLine]
      if (symb == '.'):
         draw.text((trim + jj*deltaW,trim + ii*deltaH - round(font2size/4)), symb,fill=endSentenceFill,font=font2)
      elif (symb == ','):
         draw.text((trim + jj*deltaW,trim + ii*deltaH - round(font2size/4)), symb,fill=transitionFill,font=font2)
      elif (symb == '!') or (symb == '?'):
         draw.text((trim + jj*deltaW,trim + ii*deltaH), symb,fill=endSentenceFill,font=font2)
      elif (symb == '"') or (symb == '\'') or (symb == '(') or (symb == ')') or (symb == '[') or (symb == ']'):
         draw.text((trim + jj*deltaW,trim + ii*deltaH), symb,fill=parentheticalFill,font=font2)
      elif (symb == ';') or (symb == '-') or (symb == ':'):
         draw.text((trim + jj*deltaW,trim + ii*deltaH), symb,fill=transitionFill,font=font2)
      else:
         draw.text((trim + jj*deltaW,trim + ii*deltaH), symb,fill="green",font=font2)

img.save(bookname + '.png')

#img = mpimg.imread(bookname + '.png')
#plt.imshow(img)
#plt.show()

f = Image.open(bookname + '.png').show()


print(len(punct))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
