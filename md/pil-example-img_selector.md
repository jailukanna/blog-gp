---
title: pil example img selector (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'img selector'

Functions in program: 
* `def main():`
* `def inter_evo(solutions, geometry=None, load=False, to_pil=False):`

Modules used in program: 
* `import tkinter`
* `import cv2`

## python img selector

Python pil example: img selector

```python
import cv2
import tkinter
from PIL import Image, ImageTk


def inter_evo(solutions, geometry=None, load=False, to_pil=False):
    """Ask user to select one image among the set.

    Will open a window with some buttons and images over those buttons,
    returns the index of the clicked image (button).
    solutions should be a list of PIL Images, but they can be paths or numpy
    arrays. If paths are given, use load=True. if numpy array are given, use
    to_pil=True. If you do not want full screen window, use geometry=(w, h).
    """
    selected = [None]
    root = tkinter.Tk()

    def mkclk(n):
        def _click():
            selected[0] = n
            root.destroy()
        return _click

    images = []
    # If necessary, read the images
    if load:
        # Must convert to PIL anyway
        to_pil = True
        # Load paths to cv2 images
        for i, sol in enumerate(solutions):
            sol = cv2.imread(sol, cv2.IMREAD_UNCHANGED)
            # RGB images are load as BGR and must be converted
            if len(sol.shape) == 3 and sol.shape[2] == 3:
                sol = cv2.cvtColor(sol, cv2.COLOR_BGR2RGB)
            # Replace old image
            images.append(sol)
    else:
        images = [i for i in solutions]

    # Convert to pil if necessary
    if to_pil:
        for i, img in enumerate(images):
            images[i] = Image.fromarray(img)

    # Set geometry and get size
    if geometry is not None:
        w, h = geometry
        root.geometry(f'{geometry[0]}x{geometry[1]}')
    else:
        w, h = root.winfo_screenwidth(), root.winfo_screenheight()
        root.attributes('-fullscreen', True)

    # Compute best image size
    wf = w / len(images)
    # Resize images to fit
    for img in images:
        img.thumbnail((wf, h), Image.ANTIALIAS)

    btns = []
    for i, sol in enumerate(images):
        img = ImageTk.PhotoImage(sol)
        btn = tkinter.Button(root,
                             image=img,
                             command=mkclk(i),
                             borderwidth=0,
                             highlightthickness=0)
        btn.photo = img
        btn.grid(row=0, column=i)
        btns.append(btn)

    root.mainloop()
    print('Step complete, selected image', selected[0])
    return selected[0]


def main():
    p = 'someimage.jpg'
    pics = [p, p, p]
    for _ in range(2):
        print(inter_evo(pics, load=True, to_pil=True, geometry=(300, 200)))


if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
