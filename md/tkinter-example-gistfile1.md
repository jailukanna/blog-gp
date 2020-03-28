---
title: tkinter example gistfile1 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'gistfile1'

Functions in program: 
* `def main():`
* `def programend():`
* `def leave():`
* `def consume(infile):`
* `def start_thread(func, *args):`

Modules used in program: 
* `import glob`
* `import locale`
* `import time`
* `import re`
* `import sys`
* `import queue`
* `import subprocess`
* `import distutils.dir_util`
* `import tkinter.simpledialog`
* `import tkinter.filedialog as tk_FileDialog`
* `import tkinter as tk    `
* `import os`

## python gistfile1

Python tkinter example: gistfile1

```python
#!/usr/bin/python3.4.2
#-*- coding:utf-8 -*-
programname = 'BBCrecordv1.py'
programdate = '18-05-2015'
# A USER FRIENDLY INTERFACE TO DOWNLOAD AND RECORD BBC RADIO AND TV PROGRAMS USING GET_IPLAYER in Python 3
# especially for use out of the UK by using UK Proxy servers
# tested on windows 8.1 pro 64 bit, Windows Vista 32  and Linux Mint 17 32 bit with get_iplayer 2.90 (november 2014)
#
# this script is based on scripts found on Ubuntu Users Forum France with some modifications by Bob
# main differences are: it works on Linux as well on Windows platforms
#                       a few buttons  added
#                       separate screens foreseen for search and for downloading
#                       status display and filesizes for data and subtitles 
#                       is written in Python3.4 in stead of Python2
#                       is written by a beginning Python programmer.., (corrections and suggestions welcome)
#
# the two program PYTHON scripts  are BBCrecordv1.py and BBCproxyv1.py as well for Linux as for Windows
#
#
# version v1 18-05-2015 changes
#   ProgressBar added for watching evolution of Download process
#   print(suppressed of certain messages (the % Download  messages, unless new checkbutton print(all is checked)
#   some errors corrected: in quit-button action,...
#   older  improvements:    auto scroll and window resizing
#                           display of recent  used proxy
#                           hide cmd window in Windows
#                           even if only 1 program found, download possible 
#                           scroll and resize windows possible
#                           files now found in  windows 32 Program File directory and 64 bits Program File (x86) directory
#
subl = 'BBCproxyv1.py'  # linux subroutine
subw = 'BBCproxyv1.py'  # windows subroutine  (now identical on linux subroutine)
sub = ' '
installdirw = 'C:/Program Files (x86)/get_iplayer/'
# for windows
recordirw = 'D:/Video-lib/' #attention this variable used in subprogram (BBCproxyv1.py) too!!
# for linux
recordirl = '/home/$USER/Video/'
recordir = ' '
#
# POSSIBLE ECECUTION ERRORS:
# TRICK !!  sometimes the program is not found although it is indicated in the website
#       of the BBC that it is available # for some days (mostly 7 or 28 days)
# Simply specify spaces in the search field and than all available programs are listed
#       in alphabetical order and you can also specify the first 3 or 4 digits (PID)  in the search field
# It is better to refresh the BBC-cache regulary (nothing in search fields and refresh!
#       refresh sometimes can take minutes!!!
# The WAITING state can take a while (looking for a good Proxy server) BE PATIENT!
#       difference between wait and Hang-up status can be seen if the date and time display is working!!!
#######################################################################################################
#****GENERAL INSTALLATION REMARKS****
# the output directory for the mp4 and srt files (tv) and wma files (radio) can be specified
#       in the beginning lines of the scripts
# VLC can later be used for PC display of the mp4 files and the srt file can be used for subtitles(English)
# If a TV set is capable of reading mp4 the files can be put on an usb-stick or transferred by network
# 
# Pay attention on the ABOUT-key !!!!
#
# known problems:   - in some cases filesizecounters are not started up (for specific filenames)                 
#                   - there can be a small difference in the BBC-program names found in Linux and found in
#                       Windows (special characters) up to now reason is not clear to me
#                   - because of a synchronization problem a WARNING01 message can be printed 
#******LINUX installation****
# use Python 3
# for linux use this program and the subprogram BBCproxyv1.py
# first install get_iplayer from the Linux repository (all modules are installed at once)
# remark: pay attention you install version 2.90 of get_iplayer and not earlier versions!!
# as long as 2.90 is not foreseen in the normal repositories, instructions for loading from PPA
# can be found in https://github.com/dinkypumpkin/get_iplayer/wiki/ubuntu
#
#****WINDOWS installation****
# Python installation
# On my Windows system I installed Python 3.4.x in the directory C:\Python34 
# For Windows  get-iplayer and all extra modules (these are
#   get_iplayer,Mplayer,LAME,FFmpeg,VLC, RTMPDump,Atomic Parsley) are installed by the
#   get_iplayer-setup_latest.exe from http://www.infradead.org/get_iplayer(run as administrator!!, change in properties).
# I choosed for destination directory the default directory (get_iplayer in Program Files) and another directory for the recording files
# After installation check in administrator command window if get_iplayer command works!!!
#   sometimes persisting errors wth RTMDump, demanding a newer cersion than 1.8 although 2.4 is used
# Check Path parameters maybe REVO uninstall and re-install completely get-iplayer can help
# Download bs4 (beautiful soup) and copy bs and bs4 directory to the Python directory
# If pip is not foreseen in python 3.4 go to https://pip.pypa.io/en/latest/installing.html to install pip
#   in cmd window: py -m pip install beautifulsoup4
# To test if it works run alone BBCproxyv1 and result must be IndexError: list index out of range
# The scripts BBCrecordv1.py and BBCproxyv1.py are installed in the Python directory 
#
# Take care to install get_iplayer in C:\Program Files\get_iplayer or in C:\Program Files (x86)\get_iplayer for 64 bit windows
#   (get-iplayer installation asks for its destination, also a destination for recorded videofiles)
# If you choose other locations 
#   Adapt after installation the PATH if neccesary
#       (control panel, system, advanced,environment variable,system variable,PATH...)
# IMPORTANT for WINDOWS
#   Make with a txt-editor a small file named get_iplayer.cmd and copy this file into the directory of get_iplayer!!
#   this file can contain the following:
#
#       @echo off
#       cd C:/Program Files (x86)/get_iplayer (or cd C:/Program Files/get_iplayer)
#       perl.exe get_iplayer.pl %*
#
# BBCrecordv1.py can be run through the idle.bat file or from a command window (correct PATH)
# Optional:from strawberryperl.com download and install Strawberry.perl (32 or 64 bit version)(in C:\Strawberry
#################################################################################################################
# POSSIBLE INSTALLATION ERRORS:
#   if you get message  can t open perl script get_iplayer.pl   
#       check if get_iplayer.pl is in the right directory (Program Files/get_iplayer?)
#       adapt if needed the get_iplayer.cmd file 
#   if you get message can t locate Env.pm in @INC (@INC contains .}
#       check  if getplayer.cmd is in right directory C:/Program Files (x86)/get_iplayer]
#########################################################################################################
# 
# GENAERAL installation remarks
#   How to install a not standard module?
# windows:
#   in cmd-window           cd pythondirectory  (in my case cd c:\Python342)
#                           cd Scripts
#                           pip install modulename
# linux (mint):
#   if pip not yet installed:
#       sudo apt-get install python3-pip
#       sudo python3 -m pip install modulename
#   or in some cases: (on community.linuxmint.com) install modulename
#################################################################################################################################
#################################################################################################################################

import os
try:
    from tkinter import *
except:
    from Tkinter import *
import tkinter as tk    
try:
    from ScrolledText import ScrolledText
except ImportError:
    import tkinter.scrolledtext
    from tkinter.scrolledtext import ScrolledText
import tkinter.filedialog as tk_FileDialog
import tkinter.simpledialog
try:
    import tkMessageBox
except ImportError:
    import tkinter.messagebox
from tkinter.messagebox import showinfo
from tkinter.simpledialog import askinteger
try:
    from tkColorChooser import askcolor
except ImportError:
    import tkinter.colorchooser
try:
    import tkFileDialog
except ImportError:
    import tkinter.filedialog
try:
    import ttk
except ImportError:
    import tkinter.ttk as ttk
try:
    from io import StringIO
except ImportError:
    from cStringIO import StringIO
try:
    import ConfigParser
except ImportError:
    import configparser
import distutils.dir_util
from queue import Queue, Empty
import subprocess
import queue
import sys
import re
from threading import Thread

import time
from time import ctime
import locale
import glob
ON_POSIX = 'posix' in sys.builtin_module_names
Finished = None
thread1 = None
force = None
tvtype=None
root = None
process = None
queue = Queue()
manager = None
df = None
nom = None
snom = None
nom2 = None
filesSize = None
srtfilesSize = None
exitmessage = None
mpfile = None
srtfile = None
exitmsg = None
tstf0 = None
tstsubtit = None
sz = None
linebuf = None
si = None
perc1 = None
tstprint(= None)
if sys.platform != 'linux':
    progloc = (os.environ['PROGRAMFILES'])
class IODirector(object):
#       A general class for redirecting I/O to this Text widget.
        def __init__(self,text_area):
                self.text_area = text_area
class StdoutDirector(IODirector):
#       A class for redirecting stdout to this Text widget.
        def write(self, msg):
                self.text_area.insert(END,msg)                
        def flush(self):
                pass
def start_thread(func, *args):
                global  nom, snom, nom2
                t = Thread(target=func, args=args)
                t.daemon = True
                t.start()
                return t
def consume(infile):
                global tstf0, tstsubtit,linebuf,perc1,tstprint
                for line in iter(infile.readline , ''):
                #for line in iter(infile.readline ,b''):
                        #line = line.decode(sys.stdout.encoding)
                        queue.put(line)
                        if line.startswith  ('!@!@!@if1') or line.startswith ('Downloaded Thumbnail to '): 
                                tstf0= '1'
                                Finished = True
                        if line.startswith  ('!@!@!@if2') or line.startswith ('INFO: Recorded '):
                                tstf0 = '2'
                                Finished = True
                        if line.startswith  ('!@!@!@if3') or 'Finished writing to temp file.' in line:      
                                tstf0 = '3'
                                Finished = True
                        if line.startswith  ('!@!@!@if4') or 'Already in history' in line:
                                tstf0 = '4'
                                Finished = True
                        if line.startswith  ('!@!@!@if5') or  'Starting download at' in line:
                                tstf0 = '5'
                        if line.startswith ('INFO: Subtitles not available'):
                                tstsubtit = 'N'
                        if line.startswith ('INFO: Using Proxy'):
                                tstf0 = '6'
                                linebuf = line
                        if ('kB / ' in line) or ('Progress:' in line):
                                tstprint(= "N"                                             )
                                if 'kB / 'in line:
                                    perc1 = line[line.find("(")+1:line.find("%")]
                        else:
                                tstprint(= "Y"                               )
                infile.close()
def leave():
        programend()
        return
def programend():
        global root
        showinfo('info','end of main program')
        root.destroy()
class Application(tk.Frame):             
    def __init__(self, master=None):   
        Frame.__init__(self, master)   
        self.createWidgets()
        self.after(100,self.on_after)
        self.after(1000, self.on_after2)
    def createWidgets(self):        
        self.dirfilms = StringVar()
        self.tvtype = IntVar()
        self.refresh = IntVar()
        self.force = IntVar()
        self.printall = IntVar()
        self.filesSize = StringVar()
        self.srtfilesSize = StringVar()
        self.perc1 = StringVar()
        self.filenaam = StringVar()
        self.exitmessage = StringVar()
        self.exitmsg = StringVar()
        self.hdrfs='Filesizes media/subtitle:'
        self.dtm=StringVar()
        self.hdrout='Outputfiles are recorded in: '
        self.outfile=StringVar()
        self.hdrproxy='Proxy:'
        self.proxy=StringVar()       
#               upper_frame
        self.upper_frame = Frame(background='green', borderwidth=5)
        self.upper_frame.grid(row=0,column=0,columnspan=3,sticky=N+S+E+W)
        self.df = Entry( textvariable=self.dirfilms,width=80)
        self.df.bind('<Return>', (lambda event: self.entry_window()))                                   # enter=button search
        self.df.focus_set()                                                                             # activate the Entrywindow
        self.df.grid(column=1,row=0)                                                                    # location of the Entrywindow
        self.bt1 = Button( text = 'SEARCH',justify=RIGHT,  command = self.searchcall,bd=4,bg='lightgreen')
        self.bt1.grid(row =0,column=1,sticky=E)                                                         #location of ****BUTTON SEARCH
        self.cb0 = Checkbutton(text='RADIO',variable=self.tvtype)                                       # for cache tvtype
        self.cb0.grid(row=0,column=0,sticky=W)
        self.cbd = Button(text = '   X   ',command = self.deletefields,bd=4,bg='lightgreen')
        self.cbd.grid(row=0,column=2)
#                 location of checkbutton tvtype
        self.cb1 = Checkbutton(text='refresh BBC-cache',variable=self.refresh)                          # for cache refresh
        self.cb1.grid(row=0,column=3,sticky=E)                                                          #location of checkbutton refresh
#                  listbox  with scrollbar under upper_frame
        self.lsb = Listbox(background = 'yellow',width=55,height=10)
        self.lsb.grid(column=0,columnspan=3,sticky=N+S+E+W,ipadx=200)                                   #location of listbox
        self.yscrollbar = Scrollbar()
        self.yscrollbar.grid(row=1, column=2, sticky=N+S+E)
        self.yscrollbar.config(command=self.lsb.yview)
        self.lsb.configure(yscrollcommand=self.yscrollbar.set)                                          # for autoscrolllling
#                 General Button_frame part 1
        self.btn_frame = Frame(background='green', borderwidth=5)                                       # BUTTONS
        self.btn_frame.grid(column=0,columnspan=3,sticky=N+S+E+W)
        self.bt2 = Button(text = 'DOWNLOAD',justify=RIGHT,  command = self.download,bd=4,bg='lightgreen')
        self.bt2.grid(row=2,column=2,sticky=E)                                                          #location of ****BUTTON DOWNLOAD
        self.cb2 = Checkbutton(text='Force',variable=self.force)                                        # for second download of identical file
        self.cb2.grid(row=2,column=3,sticky=W)                                                          #location of checkbutton
        self.lblfs=Label(self.btn_frame,text=self.hdrfs, justify=LEFT,anchor=W,width=18)
        self.lblfs.grid(row=2,column=0,ipady=1,padx=5)                                                  #location of label Filesize
        self.lblfs2=Label(self.btn_frame,textvariable=self.filesSize, justify=LEFT,width=10,anchor=W)
        self.lblfs2.grid(row=2,column=1,ipady=1,padx=1)                                                 #location of label Filesize
        self.lblfs3=Label(self.btn_frame,textvariable=self.srtfilesSize, justify=RIGHT,width=5)
        self.lblfs3.grid(row=2,column=2,ipady=1,padx=10)                                                #location of label srtFilesize
        self.lblfs4=Label(self.btn_frame,textvariable=self.exitmessage, justify=LEFT,fg='red',width=28)
        self.lblfs4.grid(row=2,column=3,ipady=1)                                                        #location of label exitmessage
        self.lblfs5=Label(self.btn_frame,textvariable=self.filenaam, justify=LEFT,width=50)
        self.lblfs5.grid(row=2,column=4,ipady=1,padx=30,)                                               #location of label filename
        self.filesSize.set('0000')
        self.srtfilesSize.set ('000')
        self.perc1.set ('00.0')
        self.exitmessage.set(' ****** ')
        self.filenaam.set('---------------------------')        
#                   ProgressBarFrames 
        self.pb1_frame = ttk.Frame()
        self.pb1_frame.grid(row=3,column=0,columnspan=3,sticky=W)                                                    #location of progressbar                                                                                                                   
        self.pbpr1  = ttk.Progressbar(self.pb1_frame, orient='horizontal', mode='determinate',length=490,variable=self.perc1)
        self.pbpr1.grid(row=3,column=0,sticky=W) 
        self.lblpr1v=Label(textvariable=self.perc1,justify=RIGHT,padx=1)                                #location of procentvalue
        self.lblpr1v.grid(row=3,column=1,sticky=W,padx=390)
        self.lblpr1f=Label(text='%-download',padx=3)
        self.lblpr1f.grid(row=3,column=1,sticky=W,padx=420)                                                                            
#                    Check Button for complete print
        self.cb3 = Checkbutton(text='print(all output',variable=self.printall)                          # complete print)
        self.cb3.grid(row=3,column=3,sticky=W)                                                          #location of checkbutton print(all)
#                   Text_area under upper frame
        self.text_area = Listbox(width = 55,height=22,bg='light cyan')
        top=self.winfo_toplevel()                #1
        top.rowconfigure(5, weight=1)            #2
        top.columnconfigure(0, weight=1)         #3
        self.rowconfigure(5, weight=1)           #4
        self.columnconfigure(0, weight=1)        #5
        self.rowconfigure( 1, weight=100 )
        self.columnconfigure( 0, weight=1 )
        self.text_area.grid(row=5, column=0,columnspan=3,sticky=N+S+E+W,ipadx=200)
        self.tayscrollbar= Scrollbar()
        self.tayscrollbar.grid(row=5, column=2, sticky=N+S+E)
        self.tayscrollbar.config(command=self.text_area.yview)
        self.text_area.configure(yscrollcommand=self.tayscrollbar.set)
#                       *****Date and Time
        self.lbldtm=Label(textvariable=self.dtm)
        self.lbldtm.grid(row=6,column=0)                                                                #location of date and time
        self.lblhdrout=Label(text=self.hdrout)
        self.lblhdrout.grid(row=6,column=1,sticky=W)                                                    #location of header outputfiles
        self.lbloutputfile=Label(textvariable=self.outfile,fg='red')
        self.lbloutputfile.grid(row=6,column=1,sticky=W,padx=170)                                       #location of outputfiles
        self.lblhdrproxy=Label(text=self.hdrproxy)
        self.lblhdrproxy.grid(row=6,column=1,sticky=W,padx=295)                                         #location of proxy-header
        self.lblproxy=Label(textvariable=self.proxy,width= 20,fg='red')                 
        self.lblproxy.grid(row=6,column=1,sticky=W,padx=350)                                            #location of proxy
#                 General Button_frame part 2
        self.bt3 = Button(text='ABOUT..', command=self.about,justify=LEFT,fg='red',bd=4,bg='white')
        self.bt3.grid(row=6,column=3,sticky=W,padx=0)                                                   #location of ****BUTTON ABOUT
        self.bt4 = Button(text='QUIT  ',command=leave, justify=RIGHT,bd=4,bg='white')
        self.bt4.grid(row=6,column=3,sticky=E)                                                          #location of ****BUTTON QUIT                            
    def txtprint(self,ligne):
        self.text_area.insert(END,ligne)
        self.text_area.select_clear(self.text_area.size() - 2)                                          #Clear the current selected line
        self.text_area.select_set(END)                                                                  #Select the new line
        self.text_area.yview(END)                                                                       #Set the scrollbar to the end of the listbox         
    def on_after(self):
            global queue,root,app,text_area,tstf0,tstprint
            while True:
                try:
                    v = queue.get_nowait()
                except Empty:
                    break;          
                ligne = (v)              
                if v:
                    if self.printall.get() or tstprint(== "Y" :)
                        self.txtprint(ligne)
            tim=ctime()
            self.dtm.set(tim)
            self.after(200,self.on_after)          
    def getsize(self,filename):
            sz =None
            for fn in glob.glob(filename):
                 try:
                    sz = os.path.getsize(str(fn))
                 except OSError:
                    print(("WARNING01")    )
            return sz
    def on_after2(self):
            global  tstf0,  mpfile, srtfile,recordir,nom,sz1,sz2
            if snom is not None:
                    msgsdict=dict([(1,'Finishing Download...'),
                                   (2,'finishing Download...'),
                                   (3,'FINISHED WRITING the media file !!'),
                                   (4,'This FILE is already in history !!!'),
                                   (5,'Now Downloading:'),
                                   (6,'Waiting...'),
                                   (7,'reserve')])
                    if  tstf0  is not None:
                            idx = int(tstf0)
                            exitmsg = msgsdict[idx]
                            self.exitmessage.set((exitmsg))
                            root.update()
                    if tstf0 ==  '3':
                        x=self.perc1.get()
                        if x == '99.9':                                  # esthetic correction  correct 99.9 value to 100                     
                                    self.perc1.set('100.0')
                                    root.update()
                                    
                    if tstf0 == '4':           
                            self.filesSize.set('0000')
                            self.srtfilesSize.set('000')
                            root.update()
                    if tstf0 == '6':
                            self.proxy.set(str.strip(linebuf[18:46]))
                    if tstf0 == '5' or tstf0 == None:
                            self.filenaam.set((nom2))
                            self.filesSize.set('0000')
                            self.srtfilesSize.set('000')
                            root.update()
                            if tstf0 == '5':
                                    if self.tvtype.get():
                                        #mpfile = str(recordir + nom + '*part*wma')
                                        mpfile = str(recordir + nom + '*wma')
                                    else:    
                                        #mpfile = str(recordir + nom + '*partial.m*') 
                                        mpfile = str(recordir + nom + '*.m*') 
                                    #srtfile = str(recordir + nom + '*partial.srt*')
                                    srtfile = str(recordir + nom + '*.srt*')
                                    filename = mpfile
                                    sz = '000000'
                                    sz = self.getsize(filename)
                                    sz1 = sz
                                    self.filesSize.set(str(sz1))
                                    self.perc1.set(perc1)
                                    root.update()         
                                    if tstsubtit != 'N' :        
                                            for fn2 in glob.glob(srtfile):
                                                    sz2=os.path.getsize(str(fn2))
                                                    self.srtfilesSize.set(str(sz2))
                                    else:
                                            self.srtfilesSize.set(' *** ')                                                        
                                    root.update()                           
            ##self.after(1000, self.on_after2)
            self.after(500, self.on_after2)                        
    def busy(self):
                root.config(cursor='watch')
                self.text_area.config(cursor="watch")
                root.update()
    def notbusy(self):
                global manager
                root.config(cursor='')
                self.text_area.config(cursor='')
                root.update()
    def searchcall(self):
                    global root
                    self.busy()
                    root.after_idle(self.entry_window)
    def entry_window(self):
                    if self.tvtype.get():
                            tvtypetxt = '--type=radio'
                    else:
                            tvtypetxt = '--type=tv'
                    if sys.platform == 'linux':
                            args = ["get_iplayer", tvtypetxt, self.dirfilms.get()]
                            if self.refresh.get():
                                args.append('-f')
                            app = subprocess.Popen(args=args, stdout=subprocess.PIPE,   
                                        stderr=subprocess.STDOUT)    
                    else:
                            si=None                                                                     # Hide Windows Command Window!
                            si=subprocess.STARTUPINFO()
                            si.dwFlags |= subprocess.STARTF_USESHOWWINDOW
                            si.wShowWindow = subprocess.SW_HIDE                                                   
                            args = [progloc + "/get_iplayer/get_iplayer.cmd", tvtypetxt, self.dirfilms.get()]
                            print((args))
                            if self.refresh.get():
                                args.append('-f')
                            app = subprocess.Popen(args=args, stdout=subprocess.PIPE,
                                    startupinfo=si,   
                                    stderr=subprocess.STDOUT)                    
                    (stdout, stderr) = app.communicate()
                    self.notbusy()
                    if app.returncode == 1:                                                                 # sortie en erreur →
                                ligne=('get_iplayer failed\n')
                                self.txtprint(ligne)
                    else:
                            encoding = locale.getdefaultlocale()[1]
                            lines = stdout.decode('latin').split('\n')
                            for line in lines:
                                    ligne=(line)
                                    self.txtprint(ligne)                   
                            n = 1
                            for line in lines:
                                    if line.startswith('Matches:'):
                                            break
                                    n += 1
                            lines = lines[n:-3] if n <= len(lines)  else []
                            n = 0
                            for line in lines:
                                    line = line.replace('\t', ' ')
                                    line = line.replace('Added: ','')
                                    n += 1
                                    self.lsb.insert(END, line)                                              #Insert a new line at the end of the list
                                    self.lsb.select_clear(self.lsb.size() -2)                               #Clear the current selected line
                                    self.lsb.select_set(END)                                                #Select the new line
                                    self.lsb.yview(END)                                                     #Set the scrollbar to the end of the listbox
                                    ligne=('get_iplayer success, %s founds' % (n,))
                                    self.txtprint(ligne)
    def deletefields(self):
        self.dirfilms.set("")                                                                               #Clear the entry-field
        self.lsb.delete(0,END)                                                                              #Clear the yellow listbox
        self.force.set("0")
        self.refresh.set("0")
    def download(self):
                    global process, snom, nom2,  nom, x, tstf0,recordir,tstprint
                                                                                                            # initialize some values
                    tstf0 = None
                    self.filesSize.set('0000')
                    self.srtfilesSize.set('000')
                    self.exitmessage.set('***************')
                    self.filenaam.set('---------------------------')
                    self.outfile.set(str(recordir))
                    self.perc1.set('00.0')
                    root.update()

                    sel = self.lsb.curselection()
                    if len (sel) == 0:                                                                      
                        sel = None                                                                          
                        exitmsg = 'Please Select a program to Download..'
                        self.exitmessage.set((exitmsg))
                        root.update()
                    else:                                                                                                          
                            nom =   self.lsb.get(sel).split(':')[1].split(',')[0].strip().replace(' ', '_').replace('/', '_').replace("'",'')
                            nom2 = self.lsb.get(sel).split(',')[0]                                          # modified
                            snom = './'+nom+"*"
                            if sys.platform == 'linux':
                                    sub = subl
                                    recordir = recordirl
                                    args = ["python3", r"./" + sub] + [str(self.lsb.get(sel)).split(':')[0]]+ [str(self.force.get())] +[str(self.tvtype.get())]
                                    print('args = ',args)
                                    process = subprocess.Popen(args=args, stdout=subprocess.PIPE, universal_newlines=True,
                                               stderr=subprocess.PIPE,close_fds=ON_POSIX)
                            else:
                                    sub = subw
                                    recordir = recordirw
                                    args = [sys.executable, r"./" +sub] + [str(self.lsb.get(sel)).split(":")[0]] + [str(self.force.get())]+[str(self.tvtype.get())]
                                    print('args = ',args)
                                    si=None                                                                         # Hide Windows Command Window!
                                    si=subprocess.STARTUPINFO()
                                    si.dwFlags |= subprocess.STARTF_USESHOWWINDOW
                                    si.wShowWindow = subprocess.SW_HIDE
                                    process = subprocess.Popen(args=args,startupinfo=si, stdout=subprocess.PIPE, universal_newlines=True,
                                               stderr=subprocess.PIPE,close_fds=ON_POSIX)
                            if process.returncode == 1: 
                                self.text_area.insert(END, 'call to subroutine failed!!!\n')
                                print(('call to subroutine failed!!!'))
                            else:
                                thread1 = start_thread(consume, process.stdout)
#                   #else:                                                                        # corrected
#                         #   print(                                                              # corrected       )
    def about(self):
        for n in range (1,4):
            ligne = '*************************************************************************************'
            ligne = ligne.center(90,"*")
            self.txtprint((ligne))
        ligne = "   System Info   "
        ligne = ligne.center(90," ")
        self.txtprint(ligne)
        ligne = ('  OS = ' + sys.platform + '  Python Version = ' + sys.version.split()[0]+'  ')
        ligne = ligne.center(90,"-")
        self.txtprint(ligne)
        programname= (sys.argv)       
        ligne = (' scriptname = ' + str(programname)+ ' '+ str(programdate))
        ligne = ligne.center(90,"-")
        self.txtprint(ligne)
        ligne = ("  STARTING with  this version the detailed printlines for the percentage of download ")
        ligne = ligne.center(90,"-")
        self.txtprint(ligne)
        ligne = ("  are changed in a in a GRAPHIC PROGRESS BAR,  showed while downloading...   ")
        ligne = ligne.center(90,"-")
        self.txtprint(ligne)
        ligne = ('  subprocesses are get_iplayer and ' + sub + '   ')
        ligne = ligne.center(90,"-")
        self.txtprint(ligne)
        if os.path.isfile(sub)  is True:
                ligne = ('   subroutine' + sub + '   is present   ')
                ligne = ligne.center(90,"-")
                self.txtprint(ligne)                          
        else:
                fileOK = None
                ligne = ('  subroutine   ' + sub + '   file is not installed!!!     ')
                ligne = ligne.center(90,"-")
                self.txtprint((ligne)             )
        if sys.platform != 'linux':
            ligne = ('   Location of ProgramFiles =  '+ progloc + '   ' )
            ligne = ligne.center(90,"-")
            self.txtprint((ligne) )
        for n in range (1,2):
            ligne = '**************************************************************************************'
            self.txtprint((ligne))
        ligne= ('    If you download another program before the first download is finished   ')
        ligne = ligne.center(90,"-")
        self.txtprint(ligne)
        ligne = ('   downloads will succeed simultaneously but filecounters will be incorrect!    ')
        ligne = ligne.center(90,"-")
        self.txtprint(ligne)
        for n in range (1,1):
             ligne = '*************************************************************************************'
             ligne = ligne.center(90,"-")
             self.txtprint((ligne))
        for n in range (1,3):
            ligne = '**************************************************************************************'
            ligne = ligne.center(90,"-")
            self.txtprint((ligne))
        ligne = ('    Default are TV-programs, for Radio programs mark RADIO    ')
        ligne = ligne.center(90,"-")
        self.txtprint(ligne)  
        ligne = ('   Subtitles will be loaded, if present in a SRT-file  ')
        ligne = ligne.center(90,"-")
        self.txtprint(ligne)    
        ligne =  ('   Enter now a recent BBC TV program-title or the PID-number in the search field   ')
        ligne = ligne.center(90,"-")
        self.txtprint(ligne)       
        for n in range (1,4):
            ligne = '*************************************************************************************'
            ligne = ligne.center(90,"*")
            self.txtprint((ligne))
def main():
        global app, root, dirfilms, refresh, force, tvtype, dtm, snom, nom2, filesSize, srtfilesSize, text_area, lsb, nom, ScrolledText,exitmessage,tstfin,filenaam,recordir,sub,sz,sz1,sz2
        if sys.platform == 'linux':
                                sub = subl
                                recordir = recordirl
        else:
                                sub = subw
                                recordir = recordirw
        print(('downloaded files are recorded in ',recordir)                               )
        root = Tk()
        root.minsize(1200,400)
        app=Application()
        app.master.title(programname+' and  '+sub + '  using get_iplayer to download BBC''\'s TV and Radio programs')
        root.mainloop()
if __name__ == '__main__':
        main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
