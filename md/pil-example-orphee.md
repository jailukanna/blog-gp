---
title: pil example orphee (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'orphee'

Functions in program: 
* `def start(filename, choixinstru, optionlist):`
* `def playson(tabson, ratio):`
* `def imgarray(filename):`
* `def selectimage():`

Modules used in program: 
* `import PIL.Image, PIL.ImageTk`
* `import time, pygame.mixer, pygame.sndarray, samplerate, random`

## python orphee

Python pil example: orphee

```python
from tkinter import *
from tkinter import filedialog
import time, pygame.mixer, pygame.sndarray, samplerate, random
import PIL.Image, PIL.ImageTk
from math import *


#fonction choisir un fichier
def selectimage():
    global filename
    global select
    filename = filedialog.askopenfilename(initialdir="~/",title="Selectionnez une image",filetypes=(("fichier jpg",'*.jpg'),("fichier jpeg",'*.jpeg'),("fichier png",'*.png')))
    select = Label(fenetre, text="Image sélectionnée", bg = "#a7c1de")
    select.pack(pady=20)
    
#fonction tableau avec des valeurs de 0.5 à 1.5 en fonction des couleurs des pixels
def imgarray(filename):
    with PIL.Image.open(filename) as img:
        width, height = img.size

    #calcul nombre total de pixels
    width = int(width)
    height = int(height)
    totalpixels=width*height

    #nombre de pixels à analyser (arr. inf)
    touslesxpixels=floor(totalpixels/42)

    #ouvir l'image avec PIL
    im = PIL.Image.open(filename, 'r')

    #toutes les valeurs RGB de l'image
    pix_val = list(im.getdata())

    #sélection des données nécessaires (pour qu'il y en ait 42 en tout)
    listecouleurspixels= [0]*42
    for i in range (42) :
        selection = pix_val[touslesxpixels*i]
        listecouleurspixels[i] = selection

    pix_val_flat = [x for sets in listecouleurspixels for x in sets]

    n = 0
    compteur = 0
    valeursfiniespixels = [0]*42

    for i in range (42) :
        valtot = 0
        for z in range (3) :
            valtot += pix_val_flat[n]
            n+=1
        valeurdeci = valtot/765 + 0.5
        valeursfiniespixels[i] = valeurdeci
    return valeursfiniespixels


def playson(tabson, ratio):
    # resampler pour augmenter la fréquence du son et donc le pitcher
    resampleson = samplerate.resample(tabson, ratio, "sinc_fastest").astype(tabson.dtype)
    
    # jouer le son obtenue
    sortie = pygame.sndarray.make_sound(resampleson)
    sortie.play()

def start(filename, choixinstru, optionlist):
    #ouvrir nouvelle fenetre avec l'image
    fenetreimage = Toplevel()
    fenetreimage.resizable(False,False)

    #conversion image pour pouvoir l'utiliser
    imgpil = PIL.Image.open(filename)
    bgimage = PIL.ImageTk.PhotoImage(imgpil)

    #création canvas
    largeur = bgimage.width()
    hauteur = bgimage.height()
    canvas = Canvas(fenetreimage, width = largeur-5, height = hauteur-5, bg = 'black')
    canvas.pack()
    canvas.create_image(0, 0, image = bgimage, anchor= NW)
    fenetreimage.update()
    
    # récupérer le fichier son correspondant
    son = pygame.mixer.Sound(optionlist[choixinstru])
    
    # charger le son dans un tableau en fonction des fréquences
    tabson = pygame.sndarray.array(son)
    valeursfiniespixels = imgarray(filename)
    for i in valeursfiniespixels:
        playson(tabson, i)
        time.sleep(random.uniform(0.15, 0.20))

    #fermer fenetreimage et indicateur de sélection
    select.destroy()
    fenetreimage.destroy()

#configuration fenetre TKinter
fenetre = Tk()
fenetre.configure(bg="#1B4D7C")
fenetre.geometry('900x500')
fenetre.title("Orphée - Projet d'ISN")

label=Label(fenetre, text="Orphée", font=("Arial", 44), bg = "#a7c1de").pack(side="top", pady=30)      
label2 = Label(fenetre, text= "Projet d'ISN 2017-2018 / Professeur : M. Elophe", font=("Arial", 22), bg="#a7c1de").pack(side=BOTTOM, padx=5, pady=20)
optionlist = {"Piano":"piano.wav", "Violon":"violon.wav", "Guitare":"guitare.wav", "Xylophone":"xylo.wav"}
var = StringVar(fenetre)
var.set("Piano")
option = OptionMenu(fenetre, var, "Piano", "Violon", "Guitare", "Xylophone")
option["bg"] = "#1B4D7C"
option.pack(pady=20)
choixinstru=var.get()


#bouton activant fonction selectimage
filename =""
boutonfichier = Button(fenetre, text="Choisir une image", command=selectimage, highlightbackground= "#1B4D7C")
boutonfichier.pack(pady=20)

#initiation du module pygame
pygame.mixer.init(44100,-16,2,4096)
       
boutondebut = Button(fenetre, text="Démarrer", command=lambda:start(filename, choixinstru, optionlist), highlightbackground= "#1B4D7C").pack(pady=20)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
