---
title: PIL example esketit (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'esketit'

Functions in program: 
* `def generateTweet(daysList, saveAs="test.png"):`
* `def generateRandomSendTime():`
* `def genNumberEnding(num):`
* `def genMessageDates(dayCount):`
* `def genFutureDate(dayCount):`
* `def getDateRange(startDate, endDate):`
* `def convertDateTimeToString(datetimeObject):`
* `def extractDay(datetimeObject):`
* `def extractMonthName(datetimeObject):`

Modules used in program: 
* `import random`
* `import datetime`

## python esketit

Python PIL example: esketit

```python
import datetime
import random
from PIL import Image
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

START_Y = 75
# Starting coords of first message
TODAY = datetime.date.today()
# Datetime object for current date
MAX_TWEETS = 13
# Maximum amount of tweets on 1 template
TEMPLATE_IMAGE = "template.png"
# Image containing message container
MESSAGE_IMAGE = "message.png"
# Image containing message without dates
CHECKMARK_IMAGE = "check.png"
# Image that contains the check mark
FONT_FILE = "arial.ttf"
# Contains the font file used in the image

def extractMonthName(datetimeObject):
	# Returns May, June, etc.
	return datetimeObject.strftime("%b")

def extractDay(datetimeObject):
	# Returns 1, 2, 3, etc.
	s = datetimeObject.strftime("%d")
	return (s[1:] if s.startswith('0') else s)
	# Removes initial 0

def convertDateTimeToString(datetimeObject):
	# Returns May 10th, May 11th, etc.
	monthVal = extractMonthName(datetimeObject)
	# Returns May, June, etc.
	dayVal = extractDay(datetimeObject)
	# Returns 1, 2, 3, etc.
	dayEnding = genNumberEnding(dayVal)
	# Returns th, st, nd...
	return "{} {}{}".format(monthVal, dayVal, dayEnding)

def getDateRange(startDate, endDate):
	listOfDates = []
	while startDate < endDate:
		startDate = startDate + datetime.timedelta(days=1)
		listOfDates.append(startDate)
	return listOfDates

def genFutureDate(dayCount):
	# Returns a time delta
	return TODAY + datetime.timedelta(days=int(dayCount))

def genMessageDates(dayCount):
	# Returns dates of the tweets in the screenshot
	futureDate = genFutureDate(dayCount)
	return getDateRange(TODAY, futureDate)[-MAX_TWEETS:]

def genNumberEnding(num):
	# Generates the ending of the number
	# nd, th, st...
    value = str(num)
    if len(value) > 1:
        secondToLastDigit = value[-2]
        if secondToLastDigit == '1':
            return 'th'
    lastDigit = value[-1]
    if (lastDigit == '1'):
        return 'st'
    elif (lastDigit == '2'):
        return 'nd'
    elif (lastDigit == '3'):
        return 'rd'
    else:
        return 'th'

def generateRandomSendTime():
	# Generates a random send time for the last message
	timeEndings = ["s", "m"]
	return "{}{}".format(random.randint(1, 60), random.choice(timeEndings))

def generateTweet(daysList, saveAs="test.png"):
	# daysList is a list of tuples
	im = Image.open(TEMPLATE_IMAGE).convert('RGB')
	# Loads the blank message screen
	t_width, t_height = im.size
	# Template = t | template width and size
	im2 = Image.open(MESSAGE_IMAGE).convert('RGB')
	# This is a message without date
	im3 = Image.open(CHECKMARK_IMAGE).convert('RGB')
	# This is the checkmark character
	m_width, m_height = im2.size
	# Message = m | message width and size
	draw = ImageDraw.Draw(im,'RGB')
	# Allows you to "draw" text on the image
	font = ImageFont.truetype(FONT_FILE, 11)
	# Sets font to arial / whatever font you want to use
	heightVal = START_Y
	# HeightVal == starting coords for each message
	for i, vals in enumerate(daysList):
		# Iterates through all datetime objects
		if vals != daysList[-1]:
			# Makes sure the item is not the last in the list
			val = convertDateTimeToString(vals)
			# Converts datetime object to ie: May 8th, etc.
		else:
			# The current item is the last in the list
			val = generateRandomSendTime()
			# Generates a random time: 1m, 2s, etc.
		im.paste(im2, (t_width-m_width, heightVal))
		# Pastes the message at that coord
		heightVal += (m_height)
		# Adds the message height to the next message coord
		text = val.rjust(17)
		# rjust adds leading spaces
		draw.text((t_width-m_width + 15, heightVal-1), text, (100,116,129), font=font)
		# "Draws" the text onto the template
		im.paste(im3, (t_width-m_width+font.getsize(text)[0]+10+7, heightVal+3))
		# Pastes the check mark
		heightVal += 10
		# Adds a space between messages
		#print("X: {} Y: {}".format(t_width-m_width, START_Y+(m_height*i)))
	im.save(saveAs)

if __name__ == '__main__':
	dayCount = raw_input("Number of Days: ")
	saveAs = raw_input("Save as Filename: ")
	messageList = genMessageDates(dayCount)
	generateTweet(messageList, saveAs)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
