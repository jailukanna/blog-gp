---
title: tkinter example SimpleMp3Player (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'SimpleMp3Player'

Functions in program: 
* `def main():`

Modules used in program: 
* `import pygame.mixer as player`
* `import pygame`
* `import tkinter.messagebox as msgbox`
* `import tkinter.filedialog as filedialog`

## python SimpleMp3Player

Python tkinter example: SimpleMp3Player

```python
from tkinter import *
import tkinter.filedialog as filedialog
import tkinter.messagebox as msgbox
import pygame
import pygame.mixer as player
from enum import Enum

_BG_COLOR     = 'black'
_FG_COLOR     = 'red'
_FG_ALT_COLOR = 'white'

class PlayerState(Enum):
    STOP = 0
    PAUSE = STOP + 1
    PLAY = PAUSE + 1

class CustomFrame(Frame):
    def __init__(self, master, **kwargs):
        kwargs['bg'] = _BG_COLOR
        super(CustomFrame, self).__init__(master, **kwargs)

class CustomButton(Button):
    def __init__(self, master, **kwargs):
        kwargs['bg']               = _BG_COLOR
        kwargs['fg']               = _FG_COLOR
        kwargs['relief']           = FLAT
        kwargs['activebackground'] = _BG_COLOR
        kwargs['activeforeground'] = _FG_ALT_COLOR
        kwargs['bd']               = 0
        super(CustomButton, self).__init__(master, **kwargs)

class ControlBar(CustomFrame):
    def __init__(self, master):
        super(ControlBar, self).__init__(master)

        self.mp3File     = master
        self.playerState = PlayerState.STOP

        self.create_images()
        self.create_widgets()

    def create_images(self):
        self.open_img = PhotoImage(file='open.png')
        self.stop_img  = PhotoImage(file='stop.png')
        self.play_img  = PhotoImage(file='play.png')
        self.pause_img = PhotoImage(file='pause.png')

    def create_widgets(self):
        self.openButton = CustomButton(self, image=self.open_img,
                                      command=self.open)
        self.stopButton = CustomButton(self, image=self.stop_img,
                                      command=self.stop)
        self.playButton = CustomButton(self, image=self.play_img,
                                      command=self.play_pause_wrapper)
        buttons = [self.openButton, self.stopButton, self.playButton]

        for button in buttons:
            button.pack(side=LEFT)

    def open(self):
        self.mp3File = filedialog.askopenfilename(initialdir = "/",title = "Select file",filetypes = (("mp3 files","*.mp3"),("all files","*.*")))
        print((self.mp3File))
        #return root.filename

    def play_pause_wrapper(self):
        if self.playerState == PlayerState.PLAY:
            self.playButton.config(image=self.play_img)
            player.music.pause()
            self.playerState = PlayerState.PAUSE
            print("Pause")
        elif self.playerState == PlayerState.PAUSE:
            self.playButton.config(image=self.pause_img)
            player.music.unpause()
            self.playerState = PlayerState.PLAY
            print("Resume")
        else:
            self.play()

    def play(self):
        player.music.stop()
        player.music.load(self.mp3File)
        player.music.play(0, 0.0)
        self.playerState = PlayerState.PLAY
        self.playButton.config(image=self.pause_img)
        print("Play")

    def stop(self):
        player.music.stop()
        self.playerState = PlayerState.STOP
        print("Stop")
        self.playButton.config(image=self.play_img)


class SimpleMP3Player(CustomFrame):
    def __init__(self, master):
        super(SimpleMP3Player, self).__init__(master)

        self.master   = master
        self.playlist = []

        self.master.config(bg=_BG_COLOR, padx=2, pady=2)
        self.create_widgets()
        self.pack()
        pygame.init()

    def create_widgets(self):
        self.control_bar = ControlBar(self)
        self.control_bar.pack(expand=True, fill='none')

def main():
    mp3Gui = Tk()
    mp3Gui.title('Simple MP3 Player')
    mp3Gui.iconbitmap('music.ico')
    application = SimpleMP3Player(mp3Gui)
    application.mainloop()


if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
