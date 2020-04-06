---
title: mysql example get champ skills (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'get champ skills'


## python get champ skills

Python mysql example: get champ skills

```python
from html.parser import HTMLParser
from urllib.request import Request, urlopen
from urllib import parse

start_url = "http://battlerite.gamepedia.com/Ashka"
champs = ["Ashka","Ezmo","Iva","Jade","Jumong","Taya","Varesh","Bakko","Croak","Freya","Rook","Ruh_Kaan","Shifu","Lucie","Oldur","Pearl","Pestilus","Poloma","Sirius"]


# ########## Get HTML version of chosen link ########## #
req = Request(start_url, headers={'User-Agent': 'Mozilla/5.0'})
webpage = urlopen(req).read()

str_page = webpage.decode("utf-8")

#print(webpage)

# ########## Create Parser ########## #

#global ability_name, ability_btn
#next_data = False


class AnHTMLParser(HTMLParser):
    global ability_name
    global ability_btn
    global ability_energy
    global ability_cooldown
    ability_name = False
    ability_btn = False
    ability_energy = False
    ability_cooldown = False

    global image, title, btn, cooldown, energy


    def handle_starttag(self, tag, attrs):
        #print("Encountered a start tag:", tag)
        #for (key, value) in attrs:
            #print(" key:", key, " value:", value)

                # ### Image ### #
        if tag == "img":
            for(key, value) in attrs:
                if key == "width" and value == "68":
                    for(key2, value2) in attrs:
                        if key2 == "src":
                            print("Link to Skill Image:", value2)

                # ##### Ability Title && Ability Hotkeys ##### #
        if tag == "div":
            for (key, value) in attrs:

                        # ### Title - pt1 ### #
                if key == "class" and value == "ability--title":
                    global ability_name
                    ability_name = True

                        # ### Hotkeys - pt1 ### #
                if key == "class" and value == "ability--hotkey":
                    global ability_btn
                    ability_btn = True

                        # ### Energy - pt1 ### #
                if key == "class" and value == "ability--energy-text":
                    global ability_energy
                    ability_energy = True

                        # ### Cooldown - pt1 ### #
                if key == "class" and value == "ability--cooldown-text":
                    global ability_cooldown
                    ability_cooldown = True

    # def handle_endtag(self, tag):
        # print("Encountered an end tag :", tag)

    def handle_data(self, data):
        global image, title, btn, cooldown, energy

        global ability_name, ability_btn, ability_energy, ability_cooldown

                # ### Title - pt2 ### #
        if ability_name:
            print("NAME OF SKILL IS: ", data)
            ability_name = False

                # ### Hotkeys - pt2 ### #
        if ability_btn:
            if data != "Shift":
                print("ABILITY BTN IS: ", data)
                ability_btn = False

                # ### Cooldown - pt2 ### #
        if ability_cooldown:
            print("Skill cooldown is: ", data)
            ability_cooldown = False

                # ### Energy - pt2 ### #
        if ability_energy:
            print("Skill uses ", data, " energy")
            ability_energy = False







# ########## Pull out info ########## #


    # ### Test if HTML is str ### #
print(type(str_page))

    # ### Create parser instance ### #
parser = AnHTMLParser()

    # ### Print out feed results ### #
print(parser.feed(data=str_page))



#print(HTMLParser.feed(str_page))


# class HTMLParserr(HTMLParser):
#
#    def handle_starttagg(self, tag, attrs):
#        print("Start tag:", tag)
#        for attr in attrs:
#            print("     attr:", attr)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
