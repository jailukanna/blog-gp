---
title: mysql example objCreator (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'objCreator'


Modules used in program: 
* `import mysql.connector`

## python objCreator

Python mysql example: objCreator

```python
from html.parser import HTMLParser
from champ_obj import *
import mysql.connector
from mysql.connector import errorcode


class AnHTMLParser(HTMLParser):
    # ##### Declaring Globals ##### #
    global ability_name, ability_btn, ability_energy, ability_cooldown
    ability_name = False
    ability_btn = False
    ability_energy = False
    ability_cooldown = False

    global image, title, btn, cooldown, energy


    # ##### MYSQL Connector ##### #
    try:
        cnx = mysql.connector.connect(user='root',
                                      password='',
                                      host='127.0.0.1',
                                      database='allskills')
    except mysql.connector.Error as err:
        if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
            print("Something is wrong with your user name or password")
        elif err.errno == errorcode.ER_BAD_DB_ERROR:
            print("Database does not exist")
        else:
            print(err)
    else:
        cnx.close()




    def handle_starttag(self, tag, attrs):

        # ### Image ### #
        if tag == "img":
            for (key, value) in attrs:
                if key == "width" and value == "68":
                    for (key2, value2) in attrs:
                        if key2 == "src":
                            global image
                            image = value2
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
            global title
            title = data
            ability_name = False

            # ### Hotkeys - pt2 ### #
        if ability_btn:
            if data != "Shift":
                print("ABILITY BTN IS: ", data)
                global btn
                btn = data
                ability_btn = False

                # ### Cooldown - pt2 ### #
        if ability_cooldown:
            print("Skill cooldown is: ", data)
            global cooldown
            cooldown = data
            ability_cooldown = False

            # ### Energy - pt2 ### #
        if ability_energy:
            print("Skill uses ", data, " energy")
            global energy
            energy = data

            ability_energy = False

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
