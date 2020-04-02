---
title: PIL example snapshot (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'snapshot'

Functions in program: 
* `def visible_window_titles():`
* `def show_image(cv_image, title='opencv image'):`
* `def window_cv_image(title):`

Modules used in program: 
* `import PIL.ImageGrab`
* `import win32gui`
* `import time`
* `import numpy`
* `import cv2`

## python snapshot

Python PIL example: snapshot

```python
import cv2
import numpy
import time
import win32gui
import PIL.ImageGrab

DOTA2_WINDOW_TITLE = 'Dota 2'
WINDOWS_10_FRAME_OFFSET = numpy.array((3, 26, -3, -3))
WINDOWS_FOREGROUND_SECONDS = 0.02  # typical delay for window to get to foreground


def window_cv_image(title):
    """
    Bring the given active window to the foreground, return an image of the window contents.

    :param title: name of the window
    :return: image
    :rtype image: cv2 image
    """
    hwnd = win32gui.FindWindow(None, title)
    win32gui.SetForegroundWindow(hwnd)
    rectangle = win32gui.GetWindowRect(hwnd)  # (left, top, right, bottom)
    rectangle_array = numpy.asarray(rectangle)
    bounding_box = rectangle_array + WINDOWS_10_FRAME_OFFSET
    time.sleep(WINDOWS_FOREGROUND_SECONDS)
    pil_image = PIL.ImageGrab.grab(bounding_box.tolist())
    cv_image = cv2.cvtColor(numpy.asarray(pil_image), cv2.COLOR_RGB2BGR)
    return cv_image


def show_image(cv_image, title='opencv image'):
    """
    Show image on screen in a window and wait for window to be closed.

    :param cv_image: an opencv image
    :param title: window title
    """
    cv2.imshow(title, cv_image)
    cv2.waitKey(0)
    cv2.destroyAllWindows()


def visible_window_titles():
    """
    Get a list of the current visible window titles on the desktop.
    Typically one of the titles is an empty string ''.

    :return titles: list of title strings
    :rtype titles: List
    """
    titles = []

    def foreach_window(window_handle, x):
        if win32gui.IsWindowVisible(window_handle):
            title = win32gui.GetWindowText(window_handle)
            titles.append(title)
        return True

    win32gui.EnumWindows(foreach_window, 0)
    return titles


if __name__ == '__main__':
    cv_image = window_cv_image(DOTA2_WINDOW_TITLE)
    show_image(cv_image, '(image) ' + DOTA2_WINDOW_TITLE)

    # print(visible_window_titles())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
