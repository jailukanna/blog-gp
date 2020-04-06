---
title: turtle example zelva (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'zelva'


## python zelva

Python turtle example: zelva

```python
# coding=utf8
# ke změně pozice želvičky je potřeba pár nových funkcí, jsou to window_width, window_height, penup, pendown, goto
# ty si na začátku naimportuj jak jsi zvyklá z hodin
from turtle import window_width, window_height, left, right, forward, penup, pendown, goto
# nebo také stačí import turtle
# pak budeš psát před každým příkazem turtle., např.: turtle.left(60) nebo turtle.penup() ;)

# funkce window_width, window_height slouží k zobrazení aktuální šířky a výšky okna v kterém želvička je
# můžeš se na ně kouknout zavoláním
print(window_width())
print(window_height())

# než s nimi budeme ovšem dál pracovat, je dobré si je uložit do promněnné
width = window_width()
# šířka nám v tuto chvíli bude stačit protože chceme želvičku přesunout pouze k levému kraji, nikoliv do levého horního rohu
# pokud chceš želvičku v levém horním rohu přidáš i výšku(height=window_height()) a postup bude analogický :)

# funkce penup() nám zvedne pero, takže při přesunu k levému kraji za sebou nezanecháme stopy :)
penup()

# funkce goto(x, y) nám přesune želvičku na určené souřadnice
# bez újmy na obecnosti víme že želvička vždy začíná ze středu obrazovky na souřadnicích (0,0), tedy v polovině šířky a výšky celého okna
#        výška(y, zároveň druhý parametr funkce goto())
#         |
#         |
#         | 
#<--------Ž-------->šířka(x, zároveň první parametr funkce goto())
#         |
#         |
#         |
# pokud ji tedy chceme přesunout k levému kraji
# musíme ji přesunot o polovinu celé šířky obrazovky to je první parametr
# a protože chceme aby se neposunula ani na horu, ni dolů druhý parametr necháme nula
# kdyby jsi naproti tomu chtěla aby se ti želvička přesunula jen nahoru a nepohla se přitom ani v pravo, ni vlevo stačilo by goto(0, height/2) a mít definovanou
# height=window_height()

goto(-width/2, 0)
# tím se ti želvička přesune na levý kraj, pokud by jsi chtěla pravý kraj stačí odmazat mínus

# teď musíme pero zase přiložit k papíru(plátnu), aby jsme mohli kreslit
pendown()

# mno a teď už můžeme kreslit co se nám zlíbí :D
forward(300)

for i in range(50):    
    forward(i*10)
    right(160)


exitonclick()
# ad domečky mimo okénko) je to pro to že samotné plátno na které želvička kreslí je, dalo by se říct, nekonečně velké, to že vidíme jen část je (u)způsobeno velikostí
# naší obrazovky, pokud by jsi na pravé straně okénko roztáhla, pravděpodobně by jsi našla další domečky :), s tímto se bohužel moc udělat nedá, resp. dá ale musela by jsi 
# želvičku omezovat jen na velikost okénka, např.: u kreslení vesničky by tedy základna celé vesničky nesměla přesáhnout šířku okna, u těžších útvarů už je to tedy 
# celkem dost práce uhlídat želvičku :D :D
# snad ti tato nápověda pomohla, je toho tu spousta, ale jednodušší způsob jsem nenašel, kdyby jsi i přesto měla nějakou otázku, nebo zádrhel, určitě napiš, něco vymyslíme ;)
# Jinak se samozřejmě neboj pro lepší porozumnění tento kód spustit a kouknout co dělá
# Enjoy python!

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
