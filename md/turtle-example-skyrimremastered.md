---
title: turtle example skyrimremastered (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'skyrimremastered'


## python skyrimremastered

Python turtle example: skyrimremastered

```python
from turtle import*

print("Welcome to Skyrim: Windows 95 Edition")
play = input("Would you like to play the game?")

if play.lower() =="yes" or play.lower() =="sure" or play.lower() =="yep" or play.lower() =="yeah" or play.lower() =="si" or play.lower() =="yes please":
    print("You are walking through the woods when all of the sudden something catches your attention: a wizard.")
    wizard = input ("You can A. Talk to him B. Leave him alone and continue on your journey. C. Attack him.")
    if wizard.lower() =="a." or wizard.lower() =="a":
        print("You approach him and he offers you a mysterious potion for 3 coin, that he says will give you magical powers.")
        potionoffer = input("A. Buy the potion. B. Nicely decline his offer. C. Attack him.")
        if potionoffer.lower() =="c." or potionoffer.lower() =="c":
            wizardattack = input("A. Dagger. B. Sword. C. Bow and arrow.")              
        if potionoffer.lower() =="b." or potionoffer.lower() =="b":
            print("You tell him that you don't want to buy his potion, but he keeps insisting.")
            insist = input("A. Attack him. B. Say no again. C. Tell him that he should shut up or else you'll have to attack him.")
            if insist.lower() =="a." or insist.lower() =="a":
                wizardattack = input("A. Dagger. B. Sword. C. Bow and arrow.")
            if insist.lower() =="b." or insist.lower() =="b":
                print("You kindly say no again but he starts to get angry and starts to shoot fire out of his hands at you, therefore you should probably attack him.")
                wizardattack = input("A. Dagger. B. Sword. C. Bow and arrow.") 
        if potionoffer.lower() =="a." or potionoffer.lower() =="a":
            print("You drink all of the potion in one sip, but nothing happens. It seems he has scammed you.")
            wizardkill = input("A. Kill him for scamming you. B. Walk away.")
            if wizardkill.lower() =="b." or wizardkill.lower() =="b":
                print("You walk away unsatified with your purchase.")
            if wizardkill.lower() =="a." or wizardkill.lower() =="a":
                wizardattack = input("A. Dagger. B. Sword. C. Bow and arrow.")
    if wizard.lower() == "b." or wizard.lower() == "b":
        print("You decide to go about your business and avoid the wizard.")
        print("After you turn around you notice the wizard is right behind you this time. This can't be possible.")
        wizardturn = input("A. Talk to him. B. c")
    if wizard.lower() == "c." or wizard.lower() == "b":
        wizardattack = input("A. Dagger. B. Sword. C. Bow and arrow.")

    if wizardattack.lower() =="a." or wizardattack.lower() =="a":
        print("You pull out your dagger and stap him in the face, and he dies.")
    if wizardattack.lower() =="b." or wizardattack.lower()=="b":
        print("You unsheathe your sword and slice his head off.")
    if wizardattack.lower() =="c." or wizardattack.lower() =="c":
        print("You take your bow off your back and remove and arrow from your quiver and shoot him.")
    else: print(" ")

    print("You continue on your journey and come across an ominous cave. Figuring it must have some nice loot, you enter.")
    print("As you walk through the cave, it slowly gets colder and darker the further you go into the abyss.")
    print("")
    print("Something doesn't feel right.")
    print("")
    turnaround = input("A. Turn around and get the heck out of there. B. Continues to explore the cave.")
    if turnaround.lower() =="a." or turnaround.lower() =="a":
        print("You turn around to escape the cave, and are greeted by a dozen giant spiders.")
        spiderattak = input("A. Attack B. Run for your life C. Pet the spiders")
        if spiderattak.lower == "a." or "a":
            spiderattack = input("A. Dagger. B. Sword. C. Bow and arrow.")
            if spiderattack.lower() == "a." or "a":
                print("You pull out your dagger and kill them all.")
            if spiderattack.lower() == "b." or "b":
                print("You unsheathe your sword and kill them all.")
            if spiderattack.lower() == "c." or "c":
                print("You take your bow off your back and remove an arrow from your quiver and kill them all.")
        if spiderattak.lower == "b." or "b":
            print("You run for your life and find a nice little rock to hide behind, and you hear the scary arachnids run past you, unaware of your hiding spot.")
        if spiderattak.lower == "c." or "c":
            print("You walk up to the closest spider and reach over to pet it. It bites off your hand.")
            petspider = input("A. Run. B. Attack with one of your one-handed weapons. C. Try and pet it with your other hand.")
            if petspider.lower() == "a." or "a":
                print("You run for your life and find a nice little rock to hide behind, and you hear the scary arachnids run past you, unaware of your hiding spot.")
            if petspider.lower() == "b." or "b":
                onehandspidey = input("A. Dagger. B. Sword.")
                if onehandspidey.lower() == "a." or onehandspidey.lower() == "a":
                    print("You pull out your little dagger and attempt to kill the spiders, but it is not enough.")

                    print("You have died. GAME OVER")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
