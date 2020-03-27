---
title: turtle example boemisho (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'boemisho'

Functions in program: 
* `def speel_beurt():`
* `def volg_dichtstbijzijnde_speler():`
* `def is_gebotst_met(a, b):`
* `def speler1_rechts():`
* `def speler1_links():`

## python boemisho

Python turtle example: boemisho

```python
from turtle import Turtle, Screen

# Dit is het BOEM IS HO! spelletje
#
# Je kan de turtle van speler 1 (de blauwe schildpad) besturen met 
# de A en S toetsen.
#
# Na een tijdje wordt je achterna gezeten door een monster die je
# probeert op te eten. Probeer dat monster dus voor te blijven!
#
# Run het programma eerst om het uit te proberen. Voer daarna de
# opdrachten uit die hieronder staan

TIJD_PER_BEURT = 10
AFGELEGDE_AFSTAND_PER_BEURT = 4
X_SIZE = 800
Y_SIZE = 800
DRAAI_SNELHEID = 15

scherm = Screen()

scherm.setup(X_SIZE, Y_SIZE)
scherm.title("BOEM IS HO!")
scherm.delay(1)

speler1 = Turtle("turtle")
speler1.color("blue")
speler1.penup()
speler1.setposition(-200, 200)

# OPDRACHT
# --------
# maak hier een tweede speler en geef die een mooie kleur, 
# en een andere positie op het scherm



# OPDRACHT
# --------
# voeg de tweede speler hieronder toe:

actieve_spelers = [speler1] # , speler2]

# hieronder worden de functies gemaakt om de eerste speler te besturen

def speler1_links():
    speler1.left(DRAAI_SNELHEID)

def speler1_rechts():
    speler1.right(DRAAI_SNELHEID)

# en hier worden ze gekoppeld aan de toetsen op het toetsenbord

scherm.onkey(speler1_links, "a")
scherm.onkey(speler1_rechts, "s")

# OPDRACHT
# --------
# Maak functies voor de besturing en koppel die aan (andere) 
# toetsen voor Speler 2
# Nu ben je klaar om met 2 spelers te vluchten voor het monster!


# OPDRACHT
# --------
# Kan je een derde speler toevoegen?

# hieronder staat niets meer waar je wat hoeft aan te passen

monster = Turtle("square")
monster.hideturtle()
monster.color("purple")
monster.penup()
monster.setposition(0,0)

aantal_beurten = 0
snelheid_monster = -1

def is_gebotst_met(a, b):
    return a.distance(b) < 20

def volg_dichtstbijzijnde_speler():
    dichtsbijzijnde = min(actieve_spelers, key = lambda x: monster.distance(x))
    if dichtsbijzijnde:
        monster.setheading(monster.towards(dichtsbijzijnde))

def speel_beurt():
    global aantal_beurten
    global snelheid_monster

    aantal_beurten = aantal_beurten + 1

    for speler in actieve_spelers:
        if monster.isvisible() and is_gebotst_met(monster, speler):
            actieve_spelers.remove(speler)
            print("BOEM!!")
            

        speler.forward(AFGELEGDE_AFSTAND_PER_BEURT)

    if not actieve_spelers:
        print("GAME OVER")
        scherm.bye()

    if aantal_beurten > 100:
        if not monster.isvisible():
            monster.showturtle()

        monster.forward(AFGELEGDE_AFSTAND_PER_BEURT + snelheid_monster)
        volg_dichtstbijzijnde_speler()
        snelheid_monster = -1 + max(0, aantal_beurten / 200)

    scherm.ontimer(speel_beurt, TIJD_PER_BEURT)

scherm.listen()

speel_beurt()

scherm.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
