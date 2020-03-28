---
title: tkinter example motion sensor (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'motion sensor'

Functions in program: 
* `def main():`

Modules used in program: 
* `import time`
* `import serial`
* `import threading`
* `import tkinter.font as tkFont`
* `import tkinter as tk`

## python motion sensor

Python tkinter example: motion sensor

```python
#!/usr/bin/python3
import tkinter as tk
import tkinter.font as tkFont
import threading
import serial
import time

TTY_DEVICE = "/dev/ttyACM0"
BAUD_RATE = 38400

class TtyReader(threading.Thread):
    def __init__(self, app, serial):
        threading.Thread.__init__(self)
        self.daemon = True
        self.app = app
        self.serial = serial

    def run(self):
        while True:
            _ = self.serial.readline()
            self.app.change_sensor_state(True)
            time.sleep(8)
            self.app.change_sensor_state(False)

class Application(tk.Frame):
    def __init__(self, master=None, serial=None):
        super().__init__(master)
        self.master = master
        self.font = tkFont.Font(family="helvetica", size=35)
        self.text_field = None
        self.reader = TtyReader(self, serial)
        self.reader.start()
        self.pack()
        self.create_widgets()

    def create_widgets(self):
        self.winfo_toplevel().title("Czujnik ruchu")

        self.text_field = tk.Text(self, height=3, width=50, font=self.font)
        self.text_field.tag_configure("center", justify="center")
        self.text_field.pack(side="top")
        self.text_field.insert("1.0", "\nBrak ruchu", "center")

        self.quit_btn = tk.Button(self, text="Wyjdź", fg="red", command=self.master.destroy)
        self.quit_btn.pack(side="bottom")

    def change_sensor_state(self, move=True):
        if (move):
            self.text_field.delete("1.0", tk.END)
            self.text_field.insert("1.0", "\nWykryto ruch\nOdczekaj 8 sekund do kolejnego pomiaru.", "center")
        else:
            self.text_field.delete("1.0", tk.END)
            self.text_field.insert("1.0", "\nBrak ruchu", "center")


def main():
    s = serial.Serial(TTY_DEVICE, BAUD_RATE)
    s.flushInput()
    root = tk.Tk()
    app = Application(master=root, serial=s)
    app.mainloop()

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
