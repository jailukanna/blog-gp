---
title: matplotlib example rigolScope-spectrumAnalyzer (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'rigolScope-spectrumAnalyzer'

Functions in program: 
* `def powerSpectrum2(y,samplingRate):`
* `def powerSpectrum(y,samplingRate):`

Modules used in program: 
* `import tkFileDialog`
* `import sys`
* `import rigolScope`
* `import matplotlib.cm as cm`
* `import matplotlib.dates as mdates`
* `import matplotlib.animation as animation`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import time`
* `import os`
* `import numpy as np`
* `import pylab as plt`

## python rigolScope-spectrumAnalyzer

Python matplotlib example: rigolScope-spectrumAnalyzer

```python
# Get waveforms from Rigol scope and do spectrum analysis, fast version
# Amar Vutha
# version 1, 2013-12-30

from __future__ import division
import pylab as plt
import numpy as np
from scipy import signal
from scipy.constants import e
import os
from datetime import date
import time
from functools import partial

import matplotlib
import matplotlib.pyplot as plt
import matplotlib.animation as animation
from matplotlib.widgets import Slider, Button, RadioButtons
import matplotlib.dates as mdates
import matplotlib.cm as cm
matplotlib.use('TkAgg')
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg, NavigationToolbar2TkAgg
from matplotlib.figure import Figure

os.chdir("c:/data/googledrive/code")
import rigolScope

import sys
if sys.version_info[0] < 3:
    import Tkinter as Tk
else:
    import tkinter as Tk
import tkFileDialog
from collections import deque

Navg=64
averagingQueue = deque([])

def powerSpectrum(y,samplingRate):
    #win = signal.get_window( ('gaussian',600/10),600 )
    Fy = np.fft.fft(y)/np.sqrt(len(y) * samplingRate) 
    f = np.fft.fftfreq(len(y),1/samplingRate)
    return f[1:],(2 * np.abs(Fy)**2)[1:]  # Throw away zero frequency component

def powerSpectrum2(y,samplingRate):
    win = signal.get_window( ('gaussian',len(y)/7),len(y) )
    return signal.welch(y,fs=samplingRate,window=win,return_onesided=True, scaling='density')    


if __name__ == '__main__':
    root = Tk.Tk()
    root.wm_title("Spectrum Analyzer")

    os.chdir("c:/data/googledrive/logs")
    address = "USB0::0x1AB1::0x0588::DS1EB142003911::INSTR"

    #Alpha = "USB0::0x1AB1::0x0588::DS1EB142004009::INSTR"
    #Beta = "USB0::0x1AB1::0x0588::DS1EB142003911::INSTR"
    
    s = rigolScope.Scope(address)
    print(s.id)

    fig = Figure()
    ax = fig.add_subplot(111)
    ax2=ax.twinx()
    canvas = FigureCanvasTkAgg(fig, master=root)
    canvas.show()
    canvas.get_tk_widget().pack(side=Tk.TOP, fill=Tk.BOTH, expand=1)
    toolbar = NavigationToolbar2TkAgg( canvas, root )
    toolbar.update()
    canvas._tkcanvas.pack(side=Tk.TOP, fill=Tk.BOTH, expand=1)
        
    t,V = s.fastGet()
    samplingRate = 1/(t[1] - t[0])    
    f,W = powerSpectrum(V,samplingRate)
    df = f[1]-f[0]
    
    line, = ax.loglog(f,np.sqrt(W)/np.abs(np.average(V)),'b',lw=2)
    line2, = ax2.loglog(f,np.sqrt(W),'g',lw=2)
    Vtext_template = ' avg = %.2g V \n rms = %.2g V'

    text = ax.text(0.07,0.07,Vtext_template%(np.average(V), np.sqrt( np.trapz(W,dx=df) )),transform=ax.transAxes, \
                   family='monospace',fontsize=20,weight='heavy')
    ax.set_xlim(min(f),max(f))
    ax.set_ylim(0.1 * min(np.sqrt(W))/np.abs(np.average(V)),10 * max(np.sqrt(W))/np.abs(np.average(V)))
    ax2.set_ylim(0.1* min(np.sqrt(W))/np.abs(np.average(V)),10 * max(np.sqrt(W))/np.abs(np.average(V)))
    ax.set_xlabel("Frequency, $f$ [Hz]")
    #ax.set_ylabel("SSB Spectral density, $\sqrt{W_V}$ [V/$\sqrt{\mathrm{Hz}}$]")
    ax.set_ylabel("NSR, $G_V$ [/$\sqrt{\mathrm{Hz}}$]",color='b')
    ax2.set_ylabel("Noise, $\sqrt{W_V}$ [V/$\sqrt{\mathrm{Hz}}$]",color='g')
    ax.grid(which='major',axis='both',linewidth=2)
    
    averagingQueue.append(W)
    
    def animate(i):
        t,V = s.fastGet()
        samplingRate = 1/(t[1] - t[0])    
        f,W = powerSpectrum(V,samplingRate)
        averagingQueue.append(W)
        if len(averagingQueue) >= Navg: averagingQueue.popleft()
        Wavg = np.average(averagingQueue,axis=0)
        line.set_ydata(np.sqrt(Wavg)/np.abs(np.average(V)))  # update the data
        line2.set_ydata(np.sqrt(Wavg))  # update the data
        text.set_text(Vtext_template%(np.average(V), np.sqrt( np.trapz(Wavg,dx=df) ) ))
        return (line,) + (line2,) + (text,)

    #Init only required for blitting to give a clean slate.
    def init():
        line.set_ydata(np.zeros(len(f)))
        line2.set_ydata(np.zeros(len(f)))
        text.set_text(" \n ")
        return (line,) + (line2,) + (text,) 

    ani = animation.FuncAnimation(fig, animate, init_func=init,
        frames=25,interval=10, blit=True)

    Tk.mainloop()
    #plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
