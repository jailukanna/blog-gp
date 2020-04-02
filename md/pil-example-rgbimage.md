---
title: pil example rgbimage (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'rgbimage'


Modules used in program: 
* `import astropy.io.fits`
* `import tkinter.filedialog`
* `import tkinter`
* `import PIL.ImageTk`
* `import PIL.Image`
* `import numpy`
* `import os`

## python rgbimage

Python pil example: rgbimage

```python

QLOOK_SIZE = 500


# ---

import os
import numpy
import PIL.Image
import PIL.ImageTk
import tkinter
import tkinter.filedialog
import astropy.io.fits

#import base

class main_menu(object):
    app_name = 'RGB Image'
    window = None
    
    qlook_size = QLOOK_SIZE
    
    def __init__(self, qlook_size=QLOOK_SIZE):
        self.qlook_size = qlook_size
        
        self.window = tkinter.Frame()
        self.window.master.title(self.app_name)
        
        self.image_view = image_view(self, self.window)
        
        self.title = tkinter.Label(self.window, text=self.app_name, font='Helvetica 14 bold')
        self.title.pack()
        self.add_horizontalline()
        
        self.rfits_controller = fits_controller('Red', '#e56060', qlook_size)
        self.rfits_view = fits_view(self, self.window, self.rfits_controller)
        self.gfits_controller = fits_controller('Green', '#34ba34', qlook_size)
        self.gfits_view = fits_view(self, self.window, self.gfits_controller)
        self.bfits_controller = fits_controller('Blue', '#608bdb', qlook_size)
        self.bfits_view = fits_view(self, self.window, self.bfits_controller)
        
        #self.add_horizontalline(5)
        
        self.save_controller = save_controller()
        self.save_view = save_view(self, self.window, self.save_controller)
        
        self.window.pack()    
        
        while True:
            try:
                self.window.mainloop()
            except AttributeError:
                continue
                pass
            break
        pass
        
    def update_image(self):
        rimg = self.rfits_controller.qlook_scaled
        gimg = self.gfits_controller.qlook_scaled
        bimg = self.bfits_controller.qlook_scaled
        
        if  None in [rimg, gimg, bimg]:
            return
            
        self.image_view.update_image(rimg, gimg, bimg)
        return
    
    def save_image(self, saveto):
        rimg = self.rfits_controller.get_scaled_original_data()
        gimg = self.gfits_controller.get_scaled_original_data()
        bimg = self.bfits_controller.get_scaled_original_data()
        
        if  None in [rimg, gimg, bimg]:
            return
            
        self.save_controller.save_image(saveto, rimg, gimg, bimg)
        return
        
    def add_horizontalline(self, height=1):
        hl = tkinter.Frame(self.window, height=height, bg='#DDD')
        hl.pack(fill='x', expand=1)
        return
        



class fits_controller(object):
    name = ''
    color = ''
    
    hdu = None
    data = None
    qlook_original = None
    qlook_scaled = None
    qlook_size = QLOOK_SIZE
    
    path = ''
    shape = ()
    ndim = 0
    dmax = 0
    dmin = 0
    
    scale_stretch = 'linear'
    scale_stretch_available = ['linear', 'log', 'sqrt']
    scale_min = None
    scale_max = None
    
    def __init__(self, name, color, qlook_size=QLOOK_SIZE):
        self.name = name
        self.color = color
        self.qlook_size = qlook_size
        pass
    
    def open(self, path):
        self.path = path
        self.hdu = astropy.io.fits.open(path)[0]
        self.data = self.hdu.data[::-1,:]
        self.data[numpy.where(self.data!=self.data)] = numpy.nanmax(self.data)
        self.shape = self.hdu.data.shape
        self.ndim = self.hdu.data.ndim
        self.dmax = numpy.nanmax(self.data)
        self.dmin = numpy.nanmin(self.data)
        self.qlook_original = self._resize(self.data)
        self.qlook_scale()
        pass
    
    def _resize(self, data):
        nx = data.shape[-1]
        ny = data.shape[-2]
        
        if max(nx, ny) < self.qlook_size:
            return data
        
        if nx > ny:
            resize_ratio = self.qlook_size / nx
        else:
            resize_ratio = self.qlook_size / ny
            pass
        
        resize_nx = int(nx * resize_ratio)
        resize_ny = int(ny * resize_ratio)
        
        resized = PIL.Image.fromarray(data).resize([resize_nx, resize_ny])
        return numpy.asarray(resized)
    
    def qlook_scale(self, scale_min=None, scale_max=None, scale_stretch=None):
        if self.hdu is None: return
        if scale_min is None: scale_min = self.scale_min
        if scale_max is None: scale_max = self.scale_max
        if scale_stretch is None: scale_stretch = self.scale_stretch
        
        self.qlook_scaled = self._scale(self.qlook_original, scale_min, scale_max, scale_stretch)
        return
    
    def get_scaled_original_data(self):
        return self._scale(self.data, self.scale_min, self.scale_max, self.scale_stretch)
        
    def _scale(self, data, scale_min=None, scale_max=None, stretch='linear'):
        if stretch == 'linear':
            scale_func = self._scale_linear
        elif stretch == 'log':
            scale_func = self._scale_log
        elif stretch == 'sqrt':
            scale_func = self._scale_sqrt
        else:
            scale_func = self._scale_linear
            pass
        
        if scale_min is None:
            scale_min = numpy.nanmin(data)
            pass
        
        if scale_max is None:
            scale_max = numpy.nanmax(data)
            pass
            
        self.scale_stretch = stretch
        self.scale_min = scale_min
        self.scale_max = scale_max
            
        scaled = scale_func(data, scale_min, scale_max)
        scaled = numpy.uint8(scaled * 255)
        return scaled

    def _scale_linear(self, data, scale_min, scale_max):
        scaled = (data - scale_min) / (scale_max - scale_min)
        scaled[numpy.where(scaled<0)] = 0
        scaled[numpy.where(scaled>=1)] = 1
        return scaled

    def _scale_log(self, data, scale_min, scale_max):
        scaled = self._scale_linear(data, scale_min, scale_max)
        scaled = numpy.log10((scaled * 9) + 1)
        return scaled
    
    def _scale_sqrt(self, data, scale_min, scale_max):
        scaled = self._scale_linear(data, scale_min, scale_max)
        scaled = numpy.sqrt(scaled)
        return scaled


class fits_view(object):
    def __init__(self, app, master, controller):
        self.app = app
        self.master = master
        self.controller = controller
        self.layout()
        pass
    
    def open_fits(self):
        filetypes = [('FITS file', '*.gz'), ('FITS file', '*.fits')]
        path = tkinter.filedialog.askopenfilename(filetypes=filetypes)
        if path == '': return
        
        filename = os.path.basename(path)
        self.controller.open(path)
        
        self.filename.configure(text=filename)
        self.shape.configure(text=str(self.controller.shape))
        self.ndim.configure(text=str(self.controller.ndim))
        self.scalebar_from.set(self.controller.dmin)
        self.scalebar_to.set(self.controller.dmax)
        self.scalebar_min.config(from_=self.controller.dmin, to=self.controller.dmax)
        self.scalebar_max.config(from_=self.controller.dmin, to=self.controller.dmax)
        self.scale_min_var.set(self.controller.dmin)
        self.scale_max_var.set(self.controller.dmax)
        
        self.update_scale_stretch()
        return
        
    def update_scale_stretch(self, *event):
        self.controller.qlook_scale(scale_stretch=self.stretch_var.get())
        self.app.update_image()
        return
        
    def update_scale_min(self, *event):
        self.controller.qlook_scale(scale_min=self.scale_min_var.get())
        self.app.update_image()
        return        
        
    def update_scale_max(self, *event):
        self.controller.qlook_scale(scale_max=self.scale_max_var.get())
        self.app.update_image()
        return        

    def update_scalebar_from(self, *event):
        self.scalebar_min.config(from_=self.scalebar_from.get())
        self.scalebar_max.config(from_=self.scalebar_from.get())
        return
        
    def update_scalebar_to(self, *event):
        self.scalebar_min.config(to=self.scalebar_to.get())
        self.scalebar_max.config(to=self.scalebar_to.get())
        return
        
    def layout(self):
        frame = tkinter.Frame(self.master)
        
        title_frame = tkinter.Frame(frame, bg=self.controller.color)
        title_frame.pack(fill='x', expand=1)
        title_label = tkinter.Label(title_frame, text=self.controller.name, bg=self.controller.color, fg='white').pack(side='left', padx=10)
        
        filename_frame = tkinter.Frame(frame)
        filename_frame.pack(padx=10, pady=2, fill='x', expand=1)
        filename_label = tkinter.Label(filename_frame, text='file:').pack(side='left')
        self.filename = tkinter.Label(filename_frame, text='(Click "Open" to load FITS file)')
        self.filename.pack(side='left', padx=5)
        filename_open = tkinter.Button(filename_frame, text='Open', command=self.open_fits)
        filename_open.pack(side='right')
        
        array_frame = tkinter.Frame(frame)
        array_frame.pack(padx=10, pady=0, fill='x', expand=1)
        ndim_label = tkinter.Label(array_frame, text='ndim:').pack(side='left')
        self.ndim = tkinter.Label(array_frame, text='')
        self.ndim.pack(side='left', padx=2)
        shape_label = tkinter.Label(array_frame, text='shape:').pack(side='left')
        self.shape = tkinter.Label(array_frame, text='()')
        self.shape.pack(side='left', padx=2)
        
        self.scale_min_var = tkinter.DoubleVar()
        self.scale_min_var.set(0.0)
        self.scale_min_var.trace('w', self.update_scale_min)
        self.scale_max_var = tkinter.DoubleVar()
        self.scale_max_var.set(1.0)
        self.scale_max_var.trace('w', self.update_scale_max)
        
        self.scalebar_from = tkinter.DoubleVar()
        self.scalebar_from.set(0.0)
        self.scalebar_from.trace('w', self.update_scalebar_from)
        self.scalebar_to = tkinter.DoubleVar()
        self.scalebar_to.set(1.0)
        self.scalebar_to.trace('w', self.update_scalebar_to)
        
        scale_frame = tkinter.Frame(frame)
        scale_frame.pack(padx=10, fill='x', expand=1)
        self.stretch_var = tkinter.StringVar()
        self.stretch_var.set(self.controller.scale_stretch_available[0])
        self.stretch_var.trace('w', self.update_scale_stretch)
        self.stretch = tkinter.OptionMenu(scale_frame, self.stretch_var, *self.controller.scale_stretch_available)
        self.stretch.pack(side='left')
        scale_min_label = tkinter.Label(scale_frame, text='min:').pack(side='left')
        self.scale_min = tkinter.Entry(scale_frame, textvariable=self.scalebar_from)
        self.scale_min.pack(side='left', padx=2)
        scale_max_label = tkinter.Label(scale_frame, text='max:').pack(side='left')
        self.scale_max = tkinter.Entry(scale_frame, textvariable=self.scalebar_to)
        self.scale_max.pack(side='left', padx=2)
        
        scalebar_min_frame = tkinter.Frame(frame)
        scalebar_min_frame.pack(padx=10, fill='x', expand=1)
        scalebar_min_label = tkinter.Label(scalebar_min_frame, text='min:').pack(side='left')
        self.scalebar_min = tkinter.Scale(scalebar_min_frame, from_=self.scalebar_from.get(), to=self.scalebar_to.get(), resolution=0.01, orient='horizontal', variable=self.scale_min_var)
        self.scalebar_min.pack(side='left', padx=2, fill='x', expand=1)
        
        scalebar_max_frame = tkinter.Frame(frame)
        scalebar_max_frame.pack(padx=10, fill='x', expand=1)
        scalebar_max_label = tkinter.Label(scalebar_max_frame, text='max:').pack(side='left')
        self.scalebar_max = tkinter.Scale(scalebar_max_frame, from_=self.scalebar_from.get(), to=self.scalebar_to.get(), resolution=0.01, orient='horizontal', variable=self.scale_max_var)
        self.scalebar_max.pack(side='left', padx=2, fill='x', expand=1)
        
        
        bottom_padding = tkinter.Frame(frame, height=10).pack(fill='x', expand=1)
        
        frame.pack(fill='x', expand=1)
        return


class image_view(object):
    window_name = 'Qlook'
    image = None
    
    def __init__(self, app, master):
        self.app = app
        self.master = master
        self._init_image()
        self.layout()
        pass
    
    def _init_image(self):
        img_array = numpy.zeros([self.app.qlook_size, self.app.qlook_size, 3], dtype=numpy.uint8)
        img = PIL.Image.fromarray(img_array)
        self.image = PIL.ImageTk.PhotoImage(img)
        return
        
    def update_image(self, img_r, img_b, img_g):
        ny, nx = img_r.shape
        img_array = numpy.zeros([ny, nx, 3], dtype=numpy.uint8)
        img_array[:,:,0] = img_r
        img_array[:,:,1] = img_b
        img_array[:,:,2] = img_g
        img = PIL.Image.fromarray(img_array)
        self.image = PIL.ImageTk.PhotoImage(img)
        self.canvas.create_image(self.app.qlook_size / 2, self.app.qlook_size / 2, image=self.image)
        return
        
    def layout(self):
        window = tkinter.Toplevel()
        window.title(self.window_name)
        frame = tkinter.Frame(window)
        frame.pack(fill='x', expand=1)
        self.canvas = tkinter.Canvas(frame, width=self.app.qlook_size, height=self.app.qlook_size)
        self.canvas.place(x=0, y=0)
        self.canvas.create_image(self.app.qlook_size / 2, self.app.qlook_size / 2, image=self.image)
        self.canvas.pack(fill='x', expand=1)
        return


class save_controller(object):
    def save_image(self, saveto, img_r, img_g, img_b):
        ny, nx = img_r.shape
        img_array = numpy.zeros([ny, nx, 3], dtype=numpy.uint8)
        img_array[:,:,0] = img_r
        img_array[:,:,1] = img_g
        img_array[:,:,2] = img_b
        img = PIL.Image.fromarray(img_array)
        img.save(saveto)
        return

class save_view(object):
    def __init__(self, app, master, controller):
        self.app = app
        self.master = master
        self.controller = controller
        self.layout()
        pass
        
    def layout(self):
        frame = tkinter.Frame(self.master)
        
        title_frame = tkinter.Frame(frame, bg='#555555')
        title_frame.pack(fill='x', expand=1)
        title_label = tkinter.Label(title_frame, text='Save', bg='#555555', fg='white').pack(side='left', padx=10)
        
        saveto_frame = tkinter.Frame(frame)
        saveto_frame.pack(padx=10, pady=2, fill='x', expand=1)
        saveto_open = tkinter.Button(saveto_frame, text='Save as', command=self.saveas)
        saveto_open.pack(side='right')
        
        frame.pack(fill='x', expand=1)
        return
        
    def saveas(self):
        #path = tkinter.filedialog.asksaveasfilename(mode='w', defaultextension='.png')
        path = tkinter.filedialog.asksaveasfilename(defaultextension='.png')
        if path is None: return
        
        self.app.save_image(path)
        return


if __name__=='__main__':
    m = main_menu()
    pass


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
