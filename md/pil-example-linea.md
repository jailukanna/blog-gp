---
title: pil example linea (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'linea'

Functions in program: 
* `def main():`
* `def DetectaLineas(umbral):`
* `def frecuentes(histo, cantidad):`
* `def Mascara(gx,gy):`
* `def convolucion(img, conv):`
* `def Binarizar():  # Funcion para los umbrales`
* `def Filtro():    `
* `def EscalaGrises():`
* `def window():`

Modules used in program: 
* `import sys `
* `import time`
* `import pygame`

## python linea

Python pil example: linea

```python
#!/usr/bin/python

import pygame
import time
from pygame.locals import *
from pygame import *
from PIL import Image
import sys 
from math import sqrt,fabs,sin,cos,floor,atan, ceil

image = str(raw_input('Dame el nombre de la imagen con extencion: ')) 

img = Image.open(image) # Abrimos imagen con PIL
width, height = img.size    #Obtencion de medidas de la imagen

def window():
    screen = pygame.display.set_mode((width, height)) # cargar ventana con medidas
    pygame.display.set_caption("window vision") # Mostrar ventana
    background = pygame.image.load(image) # carga de imagen
    screen.blit(background, (0,0)) #posicion de imagen en la ventana
    pygame.display.flip()   #Refrescar pantalla
    while True:  #ciclo para cerrar ventana
        for event in pygame.event.get(): # Para eventos en de pygame
            if event.type == QUIT: #Cerrar ventana
                sys.exit(0)
            if event.type == pygame.KEYDOWN:
               if event.key == pygame.K_g:
                   EscalaGrises()
                   background = pygame.image.load("grayim.jpg") # carga de imagen
                   screen.blit(background, (0,0)) #posicion de imagen en ventana
                   pygame.display.flip()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_f:
                    Filtro()
                    background = pygame.image.load("imfil.jpg") # carga de imagen
                    screen.blit(background, (0,0)) #posicion de imagen en ventana
                    pygame.display.flip()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_b:
                    Binarizar()
                    background = pygame.image.load("bin.jpg") # carga de imagen
                    screen.blit(background, (0,0)) #posicion de imagen en ventana
                    pygame.display.flip()
	    if event.type == pygame.KEYDOWN:
		if event.key == pygame.K_l:
		    DetectaLineas(10)
		    background = pygame.image.load("lineas.jpg")
		    screen.blit(background, (0,0))
		    pygame.display.flip() 
    return 0


def EscalaGrises():
    #tiempoInicial = time.time()
    img = Image.open(image) # Abrimos imagen con PIL
    pixel = img.load()  #Carga de matriz de pixeles
    width, height = img.size    #Obtencion de medidas de la imagen

    for i in range(width):  #Contar pixeles a lo ancho
        for j in range(height):  # Contar pixeles a lo largo
            (r, g, b) = pixel[i,j]
            gs = int((r + g + b) / 3)
            pixel [i, j] = (gs, gs, gs)
    img.save("grayim.jpg")
    #tiempoFinal = time.time()	
    #transcurrido = tiempoFinal - tiempoInicial
    #print("Tiempo transcurrido durante la escala de grises = ", transcurrido)
	    
def Filtro():    
    #tiempoInicial = time.time()
    img = Image.open("grayim.jpg")
    width, height = img.size
    pixel = img.load() 
    promedio = 0
    width = width-1
    height = height-1
    
    for x in range(height):
        for y in range(width):
            if y == 0 and x == 0:
                promedio = (sum(pixel[y + 1,x])/3 + sum(pixel[y,x + 1])/3 + sum(pixel[y,x])/3)/3
            if y == width and x == 0:
                promedio = (sum(pixel[y,x+1])/3 + sum(pixel[y-1,x])/3 + sum(pixel[y,x])/3)/3
            if y == 0 and x == height:
                promedio = (sum(pixel[y,x-1])/3 + sum(pixel[y+1,x])/3 + sum(pixel[y,x])/3)/3
            if y == height and x == width:
                promedio = (sum(pixel[y - 1,x])/3 + sum(pixel[y,x - 1])/3 + sum(pixel[y,x])/3)/3
            if y > 0 and y < width and x == 0:
                promedio = (sum(pixel[y+1,x])/3 + sum(pixel[y-1,x])/3 +sum(pixel[y,x+1])/3+ sum(pixel[y,x])/3)/4
            if y > 0 and y < width and x == height:
                promedio = (sum(pixel[y -1,x])/3 + sum(pixel[y,x-1])/3 +sum(pixel[y+1,x])/3+ sum(pixel[y,x])/3)/4
            if x >0 and x <height and y == 0:
                promedio = (sum(pixel[y+1,x])/3 + sum(pixel[y,x-1])/3 +sum(pixel[y,x +1])/3+ sum(pixel[y,x])/3)/4
            if y == width and x >0 and x < height:
                promedio = (sum(pixel[y - 1,x])/3 + sum(pixel[y,x-1])/3 + sum(pixel[y,x +1])/3+ sum(pixel[y,x])/3)/4
            if y > 0 and y< width and x>0 and x< height:
                promedio = (sum(pixel[y,x])/3 + sum(pixel[y + 1,x])/3 + sum(pixel[y - 1,x])/3 + sum(pixel[y,x + 1])/3 + sum(pixel[y,x -1])/3)/5	
                a = promedio
                b = promedio
                c = promedio	
                pixel[y, x] = (a,b,c)
    img.save('imfil.jpg')
    #tiempoFinal = time.time()	
    #transcurrido = tiempoFinal - tiempoInicial
    #print("Tiempo transcurrido durante la filtrado = ", transcurrido)
	

def Binarizar():  # Funcion para los umbrales
    #tiempoInicial = time.time()
    umbraln = 77 # Umbral para color negro en funcion umbral
    umbralb = 177 # Umbral para color blanco en funcion umbral
    img = Image.open("grayim.jpg") # Abrimos imagen con PIL
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
    img.save("bin.jpg")
    #tiempoFinal = time.time()	
    #transcurrido = tiempoFinal - tiempoInicial
    #print("Tiempo transcurrido durante la binarizacion = ", transcurrido)

def convolucion(img, conv):
    width, height = img.size
    pixeles = img.load()
    im = Image.new('RGB', (width,height))
    pix = im.load()

    for x in range(width):
        for y in range(height):
            resultado = 0
            for i in range(x-1, x+2):
                for j in range(y-1, y+2):
                    try:
                        resultado += g[i - (x-1)][j - (y-1)] * pixeles[i,j][1]
                    except:
                        pass
            pix[x,y] = (resultado,resultado,resultado)
    return im

def Mascara(gx,gy):
    width, height = gx.size
    conv = Image.new('RGB', (w,h))
    pixeles = conv.load()
    dx = gx.load()
    dy = gy.load()
    for x in range(width):
        for y in range(height):
            conv = int(sqrt( (dx[x,y][0])**2 + (dy[x,y][0])**2 ))
            pixeles[x,y] = (conv,conv,conv)
    return conv

#Codigo Doctora Elisa
def frecuentes(histo, cantidad):
    frec = list()
    for valor in histo:
        if valor is None:
            continue
        frecuencia = histo[valor]
        acepta = False
        if len(frec) <= cantidad:
            acepta = True
        if not acepta:
            for (v, f) in frec:
                if frecuencia > f:
                    acepta = True
                    break
        if acepta:
            frec.append((valor, frecuencia))
            frec = sorted(frec, key = lambda tupla: tupla[1])
            if len(frec) > cantidad:
                frec.pop(0)
    incluidos = list()
    for (valor, frecuencia) in frec:
        incluidos.append(valor)
    return incluidos

def DetectaLineas(umbral):
    img = Image.open("bin.jpg") # Abrimos imagen con PIL
    width, height = img.size    #Obtencion de medidas de la imagen
    sobelx = [[-1, -1, -1], [2, 2, 2], [-1, -1, -1]]
    sobely = [[-1, 2, -1], [-1, 2, -1], [-1, 2, -1]]
    imgx = convolucion(img, sobelx)
    imgy = convolucion(img, sobely)
    gx = imgx.load()
    gy = imgy.load()
    width,height = img.size
    combinaciones = {}
    pixeles = img.load()
    res = list()

    for x in range(width):
        data = list()
        for y in range(height):
            horizontal = gx[x,y][0]
            vertical = gy[x,y][0]
            if fabs(horizontal) + fabs(vertical) <= 0.0:#nada en ninguna direccion
                theta = None
            elif horizontal == 0 and vertical == 255:
                theta = 90
            elif fabs(horizontal) > 0.0:
                theta = atan(fabs(vertical/horizontal))
            if theta is not None:
                rho = fabs( x * cos(theta) + y * sin(theta))
                if x > 0 and x < w-1 and y > 0 and y < h-1:
                    if (rho, theta) in comb:
                        combinaciones[ (rho, theta) ] += 1
                    else:
                        combinaciones[ (rho, theta) ] = 1
                data.append( (rho, theta) )
            else:
                data.append((None,None))
        res.append(data)
    incluir = int(ceil (len(combinaciones) * umbral))
    frec = frecuentes(combinaciones, incluir)
    for x in range(width):
        for y in range(height):
            if x > 0 and x< width-1 and y > 0 and y < height-1:
                rho, theta = res[x][y]

                if (rho, theta) in frec:
		    theta = '%0.2f'%theta
                    if theta == "0.00":
                        pixeles[x,y] = (255,0,0)
                    elif theta == "90.00":
                        pixeles [x,y] = (0,0,255)
		    elif theta == "0.79":
			pixeles [x,y] = (0,255,0)
    img.save('lineas.jpg')

def main():
    pygame.init()
    window()
    EscalaGrises()
    Filtro() 
    Binarizar()
    DetectaLineas()
main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
