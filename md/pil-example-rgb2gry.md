---
title: pil example rgb2gry (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'rgb2gry'

Functions in program: 
* `def main():`

Modules used in program: 
* `import numpy as np`
* `import cv`

## python rgb2gry

Python pil example: rgb2gry

```python
#!/usr/bin/python
#coding: utf-8
from PIL import Image
from numpy import array
#from Tkinter import *
import cv
import numpy as np

def main():
    
    #Abrimos imagen con lo de PIL e Image
    im = Image.open('figuras.png')
    im.show() #mostramos imagen cargada con PIL
    #lo convertimos en una matriz de 3D con numpy
    arr_rgb = array(im) #convertimos en arreglo la imagen im
    arr_gry = np.zeros(shape = (arr_rgb.shape[0],arr_rgb.shape[1]))   #creamos un arreglo en 0 con el mismo tamaño que la imagen
    for n in (range(arr_rgb.shape[0])):    #Barremos por Filas
      for m in (range(arr_rgb.shape[1])):  #Barremos por Columnas
        #Primero pasamos el arreglo a escala de grises
        #se asegura con el int que sea numerp entero
        arr_gry[n,m] = int((np.sum(arr_rgb[n,m]))/3)
           
    #Salvamos imagen con nuevo nombre
    cv.SaveImage("Gray_figuras.png",cv.fromarray(arr_gry))
    im_gry = cv.LoadImage("Gray_figuras.png")
    cv.ShowImage('imagen',im_gry) #la visualizo con openCV
    cv.WaitKey(0)

    #Abajo esta la fomula de los vecinos guardada en b
    #b = array([(a[i-1,j-1],a[i-1,j],a[i-1,j+1]),(a[i,j-1],a[i,j],a[i,j+1]),(a[i+1,j-1],a[i+1,j],a[i+1,j+1])])
    #np.sum() Recuerda poner un areglo y sumara todos sus datos
if __name__ == '__main__':
    main()
##################################################################
#Referencias:
#Blog de Visión Computacional de Esteban Sifuentes Samaniego, 2013
#http://esteban-vision.blogspot.mx/

#Blog de Isaias Garza acerca de Visión Computacional, 2013
#http://isaias-garza.blogspot.mx/search/label/Visi%C3%B3n%20Computacional

#Programa de manipulación de imágenes de GNU: Manual de usuario, by Equipo de documentación de GIMP and Ignacio AntI (ant.ign@gmail.com)
#http://docs.gimp.org/es/plug-in-convmatrix.html

#http://wiki.scipy.org/Tentative_NumPy_Tutorial#head-c5f4ceae0ab4b1313de41aba9104d0d7648e35cc

#http://docs.scipy.org/doc/numpy/reference/generated/numpy.sum.html
##################################################################

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
