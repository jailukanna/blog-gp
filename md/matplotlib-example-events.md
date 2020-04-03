---
title: matplotlib example events (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'events'

Functions in program: 
* `def load_data(file_name, file_path):`
* `def ontype(event):`
* `def onpick(event):`
* `def onclick(event):`

Modules used in program: 
* `import os`
* `import sys`
* `import numpy as np`
* `import pylab as plt`

## python events

Python matplotlib example: events

```python
import pylab as plt
import numpy as np
from scipy.interpolate import splrep,splev
import sys
import os

def onclick(event):
    # when none of the toolbar buttons is activated and the user clicks in the
    # plot somewhere, compute the median value of the spectrum in a 10angstrom
    # window around the x-coordinate of the clicked point. The y coordinate
    # of the clicked point is not important. Make sure the continuum points
    # `feel` it when it gets clicked, set the `feel-radius` (picker) to 5 points
    toolbar = plt.get_current_fig_manager().toolbar
    if event.button==1 and toolbar.mode=='':
        window = ((event.xdata-5)<=wave) & (wave<=(event.xdata+5))
        y = np.median(flux[window])
        plt.plot(event.xdata,y,'rs',ms=10,picker=5,label='cont_pnt')
    plt.draw()

def onpick(event):
    # when the user clicks right on a continuum point, remove it
    if event.mouseevent.button==3:
        if hasattr(event.artist,'get_label') and event.artist.get_label()=='cont_pnt':
            event.artist.remove()

def ontype(event):
    # when the user hits enter:
    # 1. Cycle through the artists in the current axes. If it is a continuum
    #    point, remember its coordinates. If it is the fitted continuum from the
    #    previous step, remove it
    # 2. sort the continuum-point-array according to the x-values
    # 3. fit a spline and evaluate it in the wavelength points
    # 4. plot the continuum
    if event.key=='enter':
        cont_pnt_coord = []
        for artist in plt.gca().get_children():
            if hasattr(artist,'get_label') and artist.get_label()=='cont_pnt':
                cont_pnt_coord.append(artist.get_data())
            elif hasattr(artist,'get_label') and artist.get_label()=='continuum':
                artist.remove()
        cont_pnt_coord = np.array(cont_pnt_coord)[...,0]
        sort_array = np.argsort(cont_pnt_coord[:,0])
        x,y = cont_pnt_coord[sort_array].T
        spline = splrep(x,y,k=3)
        continuum = splev(wave,spline)
        plt.plot(wave,continuum,'r-',lw=2,label='continuum')

    # when the user hits 'n' and a spline-continuum is fitted, normalise the
    # spectrum
    elif event.key=='n':
        continuum = None
        for artist in plt.gca().get_children():
            if hasattr(artist,'get_label') and artist.get_label()=='continuum':
                continuum = artist.get_data()[1]
                break
        if continuum is not None:
            plt.cla()
            plt.plot(wave,flux/continuum,'k-',label='normalised')

    # when the user hits 'r': clear the axes and plot the original spectrum
    elif event.key=='r':
        plt.cla()
        plt.plot(wave,flux,'k-')

    # when the user hits 'w': if the normalised spectrum exists, write it to a
    # file.
    elif event.key=='w':
        for artist in plt.gca().get_children():
            if hasattr(artist,'get_label') and artist.get_label()=='normalised':
                data = np.array(artist.get_data())
                np.savetxt(os.path.splitext(filename)[0]+'.nspec',data.T)
                print('Saved to file')
                break
    plt.draw()

def load_data(file_name, file_path):
    """
    Loads data from the EDX csv file
    :param file_name:
    :param file_path:
    :return: x_column, y_column raw data
    """

    file_name_and_path = file_path + file_name

    x_column, y_column = np.loadtxt(file_name_and_path, dtype=float, delimiter=',', skiprows=0,
                      usecols=(0, 1), unpack=True)
    return x_column, y_column
    
if __name__ == "__main__":
    # Get the filename of the spectrum from the command line, and plot it
    file_name = 'FTIR_ATR_After DSC 30-300C 10K-min_Hole_01.dpt'
    file_path = '/home/kolan/mycode/python/FTIR/data/'    
    
    filename = file_name
    wave,flux = load_data(file_name, file_path)
    spectrum, = plt.plot(wave,flux,'k-',label='spectrum')
    plt.title(filename)

    # Connect the different functions to the different events
    plt.gcf().canvas.mpl_connect('key_press_event',ontype)
    plt.gcf().canvas.mpl_connect('button_press_event',onclick)
    plt.gcf().canvas.mpl_connect('pick_event',onpick)
    plt.show() # show the window

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
