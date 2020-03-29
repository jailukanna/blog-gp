---
title: pygtk example proyecto (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'proyecto'

Functions in program: 
* `def aplicar(self):`

Modules used in program: 
* `import scipy`
* `import serial`
* `import matplotlib.pyplot as plt`
* `import numpy`
* `import gtk.glade`
* `import gtk`
* `import pygtk`

## python proyecto

Python pygtk example: proyecto

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import pygtk
import gtk
import gtk.glade
import numpy
import matplotlib.pyplot as plt
import serial
import scipy
from time import sleep

from matplotlib.figure import Figure
from matplotlib.backends.backend_gtkagg import FigureCanvasGTKAgg as FigureCanvas
from matplotlib.backends.backend_gtkagg import NavigationToolbar2GTKAgg as NavigationToolbar

def aplicar(self):
    puertoserie = str(entry4.get_text())
    puerto = serial.Serial(puertoserie, baudrate=9600)

    # Reseteo placa Arduino
    # Sólo necesario en Linux
    puerto.setDTR(False)
    sleep(1)
    puerto.flushInput()
    puerto.setDTR(True)

    Fs = float(entry1.get_text())  # Frecuencia de muestreo
    N = int(entry2.get_text())  # Número de muestras
    n = float(entry2.get_text()) 
    L = int(entry3.get_text())   # Longitud de los segmentos
    l = float(entry3.get_text()) 

    data = []  # Lista contenedora del muestreo
    data_seg = numpy.zeros((N / L, L))  # Lista contenedora de los segmentos de longitud L
    resultado = numpy.zeros((N / L, L))
    promedio = numpy.zeros(L)

    # Lectura de muestras por el puerto serie
    for i in range(0, N):
        line = puerto.readline()
        if not line:
            continue
        dato = numpy.fromstring(line.decode('ascii', errors='replace'), sep=' ')
        dato = float(dato)
        # Escalado
        dato = (dato - 128.0) / 25.6
        data.append(dato)

    t = numpy.arange(0, N * (1 / Fs), (1 / Fs))

    tiempo.set_canvas(canvastiempo)
    tiempoplot.clear()
    tiempoplot.plot(t, data)
    canvastiempo.draw()

    # División de la señal en los distintos segmentos
    for i in range(0, (N / L)):
        data_seg[i] = data[(L * i):(L + (L * i))]

    # Cálculos del Método de Bartlett
    ventana = numpy.hamming(L)
    for i in range(0, (N / L)):
        ejeXV = (data_seg[i] * ventana)
        resultado[i] = numpy.abs(numpy.fft.fft(ejeXV))
        resultado[i] = resultado[i] * resultado[i]
        resultado[i] /= 1 / l

    # Promedio de todos los segmentos de señal
    for columna in range(0, L):
        for fila in range(0, (N / L)):
            promedio[columna] = promedio[columna] + resultado[fila][columna]
    promedio = promedio / (n / l)

    k = scipy.arange(l)
    T = l / Fs  # Periodo
    frq = k / T  # Frecuencia

    # Graficar la Frecuencia entre |0... Fs/2|
    # Si se eliminan ambas líneas, se graficará entre |0... Fs|
    freq = freq[range(L / 2)]
    promedio = promedio[range(L / 2)]

    frecuencia.set_canvas(canvasfrecuencia)
    frecuenciaplot.clear()
    frecuenciaplot.plot(freq, promedio)
    canvasfrecuencia.draw()

gladefile = "proyecto.glade"
proyecto = gtk.Builder()
proyecto.add_from_file(gladefile)

eventos = {
    'evento_terminar': gtk.main_quit,
    'evento_aplicar': aplicar
}
proyecto.connect_signals(eventos)
vbox3 = proyecto.get_object("vbox3")
vbox4 = proyecto.get_object("vbox4")
vbox1 = proyecto.get_object("vbox1")
hbox1 = proyecto.get_object("hbox1")
entry1 = proyecto.get_object("entry1")
entry2 = proyecto.get_object("entry2")
entry3 = proyecto.get_object("entry3")
entry4 = proyecto.get_object("entry4")
win = proyecto.get_object("window1")
#Definición del objeto gráfico para el dominio del tiempo
tiempo = Figure(figsize=(300, 100), dpi=50)
tiempoplot = tiempo.add_subplot(111)
tiempo.subplots_adjust(bottom=0.2)
tiempoplot.set_title("Vibracion Temporal")
tiempoplot.set_xlabel("Tiempo (s)")
tiempoplot.set_ylabel("Aceleracion (G)")
tiempoplot.plot()
canvastiempo = FigureCanvas(tiempo)
vbox3.pack_start(canvastiempo)
toolbartiempo = NavigationToolbar(canvastiempo, win)
vbox3.pack_start(toolbartiempo, False, False)
#Definición del objeto gráfico para el dominio de la frecuencia
frecuencia = Figure(figsize=(300, 100), dpi=50)
frecuenciaplot = frecuencia.add_subplot(111)
frecuencia.subplots_adjust(bottom=0.2)
frecuenciaplot.set_title("Espectro de Frecuencia")
frecuenciaplot.set_xlabel("Frecuencia (hz)")
frecuenciaplot.set_ylabel("Magnitud")
frecuenciaplot.plot()
canvasfrecuencia = FigureCanvas(frecuencia)
vbox4.pack_start(canvasfrecuencia)
toolbarfrecuencia = NavigationToolbar(canvasfrecuencia, win)
vbox4.pack_start(toolbarfrecuencia, False, False)

win.show_all()
gtk.main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
