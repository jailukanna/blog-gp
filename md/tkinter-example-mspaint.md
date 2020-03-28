---
title: tkinter example mspaint (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'mspaint'


Modules used in program: 
* `import pickle`
* `import tkinter.messagebox`
* `import tkinter.ttk as ttk`
* `import tkinter as tk`

## python mspaint

Python tkinter example: mspaint

```python
import tkinter as tk
import tkinter.ttk as ttk
#from tkinter import LEFT, RIGHT, UP, BOTTOM
from tkinter import *
from tkinter.colorchooser import *
import tkinter.messagebox
from tkinter.filedialog import *
import pickle


class Circle():
    """
    Blueprint(for every circle painted)
    """
    def __init__(self, _id, coords, fc, oc):
        self.id = _id
        self.x0 = coords[0]
        self.y0 = coords[1]
        self.x1 = coords[2]
        self.y1 = coords[3]
        self.fill_color = fc
        self.outline_color = oc


class Screen():
    """
    Handles drawing code for graphics
    """
    default_brush_size = 5.0
    default_color = 'black'

    def __init__(self, width, height):
        self.root = tk.Tk()
        self.root.title("Microsoft Paint Clone")
        self.root.resizable(True, True)
        self.root.geometry(height + 'x' + width)
        self.height = height
        self.width = width

        self.brush_size = self.default_brush_size
        self.color = self.default_color
        self.bgcolor = None
        self.resolution = 1
        self.circles_painted = []


    def alert_message(self, header, subject):
        """
        Prints a graphical alert message
        """
        tkinter.messagebox.showinfo(header, subject)


    def show_cascade_menu(self):
        """
        Makes a menu at the top to save and load paintings
        """
        self.root_menu = Menu(self.root)
        self.file_menu = Menu(self.root_menu, tearoff=0)

        #Adds commands
        #self.file_menu.add_command(label='New', command=self.nothing)
        self.file_menu.add_command(label='Open', command=self.file_open)
        self.file_menu.add_command(label='Save', command=self.file_save)
        self.root_menu.add_cascade(label='File', menu=self.file_menu)
        self.root.config(menu=self.root_menu)


    def file_new(self):
        """
        Asks to save current painting and then creates a new one
        """
        pass


    def file_open(self):
        """
        Opens a painting
        """
        def nothing():
            pass
        #Asks to save current file before opening a new painting
        save_before_opening = Dialog(screen, screen.root, 'Save current painting?')
        save_before_opening.set_choices({'Yes':self.file_save, 'No':save_before_opening.cancel})
        save_before_opening.show()

        #Opens a new painting
        initial_dir = "C:\Temp"
        mask = \
        [("Pickled file","*.pickle")]
        filen = askopenfile(initialdir=initial_dir, filetypes=mask)
        if filen:
            filen = filen.name
            with open(filen, 'rb') as f:
                #Checks for corruption in the painting
                try:
                    list_of_circles = pickle.load(f)
                    #print('List of circles:', list_of_circles)
                except:
                    self.alert_message('Corrupted', 'This painting is corrupted')
                else:
                    #If there is no corruption the screen will repaint the painting onto the screen
                    self.canvas.delete(ALL)
                    self.circles_painted = list_of_circles
                    for circle in list_of_circles:
                        #self.canvas.create_oval(circle.x0, circle.y0, circle.x1, circle.y1, fill=circle.fill_color, outline=circle.outline_color)
                        self.canvas.create_oval(circle['x0'], circle['y0'], circle['x1'], circle['y1'], fill=circle['fill_color'], outline=circle['outline_color'])


    def file_save(self):
        """
        Saves painting in the current directory
        """
        initial_dir = "C:\Temp"
        mask = \
        [("Pickled file","*.pickle")]
        #try:
        filen = asksaveasfile(initialdir=initial_dir, filetypes=mask, mode='w', defaultextension='.pickle')
        if filen:
            filename = filen.name
            with open(filename, 'wb') as f:
                pickle.dump(self.circles_painted, f)
        return filen


    def show_canvas(self, parent, color):
        Grid.rowconfigure(parent, 0, weight=1)
        Grid.columnconfigure(parent, 0, weight=1)
        #Create & Configure frame
        #self.frame = Frame(parent)
        #self.frame.grid(row=0, column=0, sticky=N+S+E+W)

        self.canvas = Canvas(parent, bg=color, height=self.height, width=self.width, highlightthickness=0)
        self.canvas.grid(row=0, column=0, sticky=N+S+E+W)
        #self.canvas.bind('<Configure>', self.resize)
        self.bgcolor = color


    def resize(self, event):
        w,h = event.width, event.height
        self.canvas.config(width=w, height=h)


    def start_mainloop(self):
        self.root.mainloop()


    def size(self, size):
        self.brush_size = size
        self.slider.set(size)


    def get_color(self):
        """
        Gets the color from the user
        """
        gotten_color = askcolor()
        self.color = gotten_color[1]


    def set_color(self, _set):
        self.color = _set


    def show_buttons(self, button_map):
        """
        Puts the buttons on the screen.
        Standard buttons are pen, pencil, brush, change color, and size slider
        """
        Grid.rowconfigure(self.canvas, 0, weight=1)
        Grid.rowconfigure(self.canvas, 1, weight=300)
        Grid.columnconfigure(self.canvas, 0, weight=1)


        for button_label, col_index in zip(button_map, range(len(button_map))):
            Grid.columnconfigure(self.canvas, col_index, weight=1)
            btn = Button(self.canvas, text=button_label, command=button_map[button_label]) #create a button inside frame
            btn.grid(row=0, column=col_index, sticky=N+S+E+W)

        #Brush size slider
        Grid.columnconfigure(self.canvas, len(button_map)+1, weight=1)
        self.slider = Scale(self.canvas, from_=1, to=50, orient=HORIZONTAL, command=self.update_size) #create a button inside frame
        self.slider.grid(row=0, column=len(button_map)+1, sticky=N+S+E+W)
        self.slider.set(self.brush_size)


    def paint(self, event):
        """
        Paints on the screen while the left mouse button is held down
        """
        x, y = event.x, event.y
        x0 = x - (int(self.brush_size)/2)
        y0 = y - (int(self.brush_size)/2)
        x1 = x + (int(self.brush_size)/2)
        y1 = y + (int(self.brush_size)/2)
        id = self.canvas.create_oval(x0, y0, x1, y1, fill=self.color, outline=self.color)

        #Makes a circle dictionary to append to all of the circles painted
        #This dictionary will prevent corruption in the future when reopening the painting
        circle = {'id':id, 'x0':x0, 'y0':y0, 'x1':x1, 'y1':y1, 'fill_color':self.color, 'outline_color':self.color}
        #circle = Circle(id, (x0, y0, x1, y1), self.color, self.color)
        self.circles_painted.append(circle)


    def fill(self):
        """
        To make the painting smaller, fill gets rid of all previously drawn
        Circles and fills the screen with the given color
        """
        gotten_color = askcolor()
        color = gotten_color[1]
        self.canvas.delete(ALL)
        height, width = self.canvas.winfo_screenheight(), self.canvas.winfo_screenwidth()
        id = self.canvas.create_rectangle(0, 0, width, height, fill=color)
        self.circles_painted = [{'id':id, 'x0':0, 'y0':0, 'x1':width, 'y1':height, 'fill_color':color, 'outline_color':color}]


    def change_instrument(self, newsize):
        self.canvas.bind("<B1-Motion>", screen.paint)
        self.canvas.bind("<Button-1>", screen.paint)
        self.brush_size = newsize
        self.slider.set(newsize)


    def update_size(self, newsize):
        self.brush_size = newsize


    def eraser_bind(self, bind):
        self.canvas.bind("<B1-Motion>", bind)
        self.canvas.bind("<Button-1>", bind)


    def erase(self, event):
        """
        Finds overlapping objects and removes them
        """
        x, y = event.x, event.y
        max_eraser_distance = int(self.brush_size)
        id_gotten = event.widget.find_overlapping(x, y, x, y)
        self.canvas.delete(id_gotten)
        for circle in self.circles_painted:
            if circle['id'] == id_gotten:
                self.circles_painted.pop(self.circles_painted.index(circle))


class Dialog(Toplevel):
    """
    Customizable dialog box that only works with a parent window
    """

    def __init__(self, screen, parent, title):
        Toplevel.__init__(self, parent)
        self.protocol("WM_DELETE_WINDOW", self.close_everything)
        self.transient(parent)
        if title:
            self.title(title)

        self.parent = parent
        self.screen = screen
        self.result = None
        self.choices = None
        self.eval_text = None


    def show(self):
        """
        Shows itself
        """
        body = Frame(self)
        self.initial_focus = body
        body.pack(padx=5, pady=5)

        #Button box
        box = Frame(self)
        for choice in self.choices:
            w = Button(box, text=choice, width=10, command=self.choices[choice], default=ACTIVE)
            w.pack(side=LEFT, padx=5, pady=5)
        box.pack()


        self.grab_set()
        if not self.initial_focus:
            self.initial_focus = self

        self.geometry("+%d+%d" % (self.parent.winfo_rootx()+50,
                                  self.parent.winfo_rooty()+50))
        self.parent.resizable(False, False)
        self.initial_focus.focus_set()
        self.wait_window(self)


    def ok(self, event=None):
        if not self.validate():
            self.initial_focus.focus_set() # put focus back
            return

        self.withdraw()
        self.update_idletasks()
        self.apply()
        self.cancel()


    def cancel(self, event=None):
        self.parent.focus_set()
        self.destroy()


    # command hooks
    def set_choices(self, set_to):
        self.choices = set_to


    def close_everything(self):
        """
        Closes every window if user closes the window to choose
        Who they want to play against
        """
        try:
            self.cancel()
            self.screen.close_window()
            self.result = 'closed'
        except:
            pass





if __name__ == '__main__':
    screen = Screen(height='1500', width='700')
    #screen.canvas.bind("<Button-1>", screen.paint)

    screen.show_canvas(screen.root, 'white')
    screen.canvas.bind("<B1-Motion>", screen.paint)
    screen.canvas.bind("<Button-1>", screen.paint)
    # screen.canvas.bind("<B1-Motion>", screen.erase)
    # screen.canvas.bind("<Button-1>", screen.erase)


    #Drawing code
    screen.show_buttons({
                        'Pencil':lambda: screen.change_instrument(1),
                        'Pen':lambda: screen.change_instrument(2),
                        'Marker':lambda: screen.change_instrument(10),
                        'Fill':lambda: screen.fill(),
                        'Eraser':lambda: screen.eraser_bind(screen.erase),
                        'Color':screen.get_color,
                        })
    screen.show_cascade_menu()


    screen.start_mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
