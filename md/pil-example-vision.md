---
title: pil example vision (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'vision'

Functions in program: 
* `def main():`
* `def convolucion():`
* `def filtro(): #Filtrada por el metodo de los vecinos`
* `def umbrales():  # Funcion para los umbrales`
* `def escalagrises(): # Funcion para hacer a escala de grises`
* `def window():`

Modules used in program: 
* `import math`
* `import time`
* `import sys`
* `import numpy  #Libreria para arreglos`
* `import pygame  # interfaz`

## python vision

Python pil example: vision

```python
#!/usr/bin/env python

import pygame  # interfaz
from pygame.locals import *  # para funcion de botones y raton
from pygame import * 
from PIL import Image # Para cargar imagen
from math import *
import numpy  #Libreria para arreglos
import sys
import time
import math

image = str(raw_input('Dame el nombre de la imagen con extencion: ')) # Pedimos imagen

img = Image.open(image) # Abrimos imagen con PIL
width, height = img.size    #Obtencion de medidas de la imagen


def window():
    screen = pygame.display.set_mode((width, height)) # cargar ventana con medidas
    pygame.display.set_caption("window vision") # Mostrar ventana
    background = pygame.image.load(image) # carga de imagen
    #button = pygame.Surface((100, 25))
    screen.blit(background, (0,0)) #posicion de imagen en la ventana
    pygame.display.flip()   #Refrescar pantalla
    while True:  #ciclo para cerrar ventana
        for event in pygame.event.get(): # Para eventos en de pygame
            if event.type == QUIT: #Cerrar ventana
                sys.exit(0)
            if event.type == pygame.KEYDOWN:
               if event.key == pygame.K_g:
                   escalagrises()
                   background = pygame.image.load("grayim.jpg") # carga de imagen
                   screen.blit(background, (0,0)) #posicion de imagen en ventana
                   pygame.display.flip()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_f:
                    filtro()
                    background = pygame.image.load("imfil.jpg") # carga de imagen
                    screen.blit(background, (0,0)) #posicion de imagen en ventana
                    pygame.display.flip()
	    if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_c:
	            convolucion()
		    background = pygame.image.load("sobel.jpg") # carga de imagen
                    screen.blit(background, (0,0)) #posicion de imagen en ventana
                    pygame.display.flip()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_b:
                    umbrales()
                    background = pygame.image.load("bina.jpg") # carga de imagen
                    screen.blit(background, (0,0)) #posicion de imagen en ventana
                    pygame.display.flip()
    return 0

def escalagrises(): # Funcion para hacer a escala de grises
    tiempoInicial = time.time() #para contar el tiempo de ejecucion.
    img = Image.open(image) # Abrimos imagen con PIL
    pixel = img.load()  #Carga de matriz de pixeles
    width, height = img.size    #Obtencion de medidas de la imagen

    for i in range(width):  #Contar pixeles a lo ancho
        for j in range(height):  # Contar pixeles a lo largo
            (r, g, b) = pixel[i,j] 
            gs = int((r + g + b) / 3) 
            pixel [i, j] = (gs, gs, gs)
    img.save("grayim.jpg")
    tiempoFinal = time.time()
    transcurrido = tiempoFinal - tiempoInicial
    print("Tiempo transcurrido durante la escala de grises = ", transcurrido)
	    
def umbrales():  # Funcion para los umbrales
    tiempoInicial = time.time() #para contar tiempo de ejecucion.
    umbraln = 77 # Umbral para color negro en funcion umbral
    umbralb = 177 # Umbral para color blanco en funcion umbral
    img = Image.open("sobel.jpg") # Abrimos imagen con PIL
    pixel = img.load()  #Carga de matriz de pixeles
    width, height = img.size    #Obtencion de medidas de la imagen

    for i in range(width):
        for j in range(height):
	    (r, g, b) = pixel[i,j]
            prom = int((r + g + b) / 3)
            if prom < umbraln:
	        pixel[i,j] = (0, 0, 0)
	    elif prom > umbralb:
	        pixel[i,j] = (255, 255, 255)
            else:   
                pixel[i,j] = (prom, prom, prom)  
    #img.save("umb.png")	       
    img.save("bina.jpg")
    tiempoFinal = time.time()	
    transcurrido = tiempoFinal - tiempoInicial
    print("Tiempo transcurrido durante la binarizacion = ", transcurrido)
	
def filtro(): #Filtrada por el metodo de los vecinos
	tiempoInicial = time.time()
        img = Image.open("grayim.jpg")
	width, height = img.size
	pixel = img.load() 
	promedio = 0
	width = width-1
	height = height-1

	for x in range(height):
		for y in range(width):
			#esquina superior izquierda
			if y == 0 and x == 0:
				promedio = (sum(pixel[y + 1,x])/3 + sum(pixel[y,x + 1])/3 + sum(pixel[y,x])/3)/3
			#esquina superior derecha
			if y == width and x == 0:
				promedio = (sum(pixel[y,x+1])/3 + sum(pixel[y-1,x])/3 + sum(pixel[y,x])/3)/3
                        # esquina inferior izquierda
			if y == 0 and x == height:
				 promedio = (sum(pixel[y,x-1])/3 + sum(pixel[y+1,x])/3 + sum(pixel[y,x])/3)/3
                        #esquina inferior derecha
			if y == height and x == width:
				 promedio = (sum(pixel[y - 1,x])/3 + sum(pixel[y,x - 1])/3 + sum(pixel[y,x])/3)/3
                        #barra de arriba
			if y > 0 and y < width and x == 0:
				promedio = (sum(pixel[y+1,x])/3 + sum(pixel[y-1,x])/3 +sum(pixel[y,x+1])/3+ sum(pixel[y,x])/3)/4
                        #barra de abajo
			if y > 0 and y < width and x == height:
				promedio = (sum(pixel[y -1,x])/3 + sum(pixel[y,x-1])/3 +sum(pixel[y+1,x])/3+ sum(pixel[y,x])/3)/4
                        #barra lateral izquierda
			if x >0 and x <height and y == 0:
				promedio = (sum(pixel[y+1,x])/3 + sum(pixel[y,x-1])/3 +sum(pixel[y,x +1])/3+ sum(pixel[y,x])/3)/4
                        #barra lateral derecha
			if y == width and x >0 and x < height:
				promedio = (sum(pixel[y - 1,x])/3 + sum(pixel[y,x-1])/3 + sum(pixel[y,x +1])/3+ sum(pixel[y,x])/3)/4
                        #4 vecinos
			if y > 0 and y< width and x>0 and x< height:
				promedio = (sum(pixel[y,x])/3 + sum(pixel[y + 1,x])/3 + sum(pixel[y - 1,x])/3 + sum(pixel[y,x + 1])/3 + sum(pixel[y,x -1])/3)/5	

			a = promedio
			b = promedio
			c = promedio	
			pixel[y, x] = (a,b,c)
	img.save('imfil.jpg')
	tiempoFinal = time.time()	
	transcurrido = tiempoFinal - tiempoInicial
	print("Tiempo transcurrido durante la filtrado = ", transcurrido)
	

def convolucion():
	tiempoInicial = time.time()
        img = Image.open("imfil.jpg")
	width, height = img.size
	pixel = img.load()
	sobelx = ([-1, 0, 1], [-2, 0, 2], [-1, 0, 1])  #Valor del operador Sobel
	sobely = ([1, 2, 1], [0, 0, 0], [-1, -2, -1])  #en "x" e "y".
	resx = 0
	resy = 0 	

	for x in range(height):
		for y in range(width):
			resx = 0
			resy = 0
			if x != 0 and y != 0 and y != width and x != height: #Para obtener un centrado de la mascara.	
				for i in range(3): #Matriz 3x3
					for j in range(3):
						try:
							resgx = sobelx[i][j]*pix[y+j, x+i][1]#gradientes 
							resgy = sobely[i][j]*pix[y+j, x+i][1]
						except:
							resgx = 0
							resgy = 0

						resx = resgx+resgx
						resy = resgy+resy

				expx = pow(resx, 2) #sacamos potencia
				expy = pow(resy, 2) 
				resultadog = int(sqrt(expx+expy)) #sacamos raiz
				if resultadog > 255: #Por si se pasan los valores
					resultadog = 255

				if resultadog < 0: 
					resultadog = 0

				pixel[y,x] = (resultadog , resultadog, resultadog) 			

	img.save('sobel.jpg')					
	tiempoFinal = time.time()	
	transcurrido = tiempoFinal - tiempoInicial
	print("Tiempo transcurrido durante la convolucion = ", transcurrido)
	 
def main():
    pygame.init()
    window()
    escalagrises()
    filtro() 
    umbrales()
    normaliza()
main()	

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
