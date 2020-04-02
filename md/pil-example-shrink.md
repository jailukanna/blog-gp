---
title: pil example shrink (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'shrink'

Functions in program: 
* `def getGoodThreadValue():`

Modules used in program: 
* `import BmpImagePlugin, GifImagePlugin, JpegImagePlugin, PngImagePlugin`
* `import multiprocessing, os, PIL, Queue, shutil, string, sys, threading`

## python shrink

Python pil example: shrink

```python
"""
Author:     Matze
Date:       July 2012
Website:    https://github.com/ma4a

About functions:
*creates same recursive folder structure in parallel with resized images
 *you have two folder structures. the first untouched one with the source files and the one with resized images and copied videos
*resize any found image that's larger than needed -> EXIF data not copied
*just copy any image that's smaller than needed - no resize necessary -> EXIF data may be copied
*copy any video without doing anything with the file
*file types that are not defined as video or image are simply skipped
*supports threading per folder to speed up. i.e. using 6 threads on a quad core was more than 6 times faster than without :)
*summary at the end to see success or failure

Requirements:
*PIL (tested with 1.1.7, http://www.pythonware.com/products/pil/)
*Python (according to PIL, tested with 2.7.3 for Win 32bit, http://www.python.org/download/)
"""

import multiprocessing, os, PIL, Queue, shutil, string, sys, threading
from PIL import Image, ImageOps
import BmpImagePlugin, GifImagePlugin, JpegImagePlugin, PngImagePlugin

#* * * don't touch this * * *#
def getGoodThreadValue():
    if multiprocessing.cpu_count() > 1:
        return 2 * multiprocessing.cpu_count() - 2
    else:
        return 1


#SETTINGS *** only change values in this area
dirBig = 'D:\_shrinked\img'                                                             #source folder, absolute path
dirSmall = 'D:\_shrinked\img_small'                                                     #destination folder, absolute path
extList_img = [".jpg", ".jpeg", ".png", ".gif"]                                         #image file extensions
extList_vid = [".mp4", ".3gp", ".mov", ".avi", ".mpg", ".mpeg"]                         #video file extensions
newSize = (0,800)                                                                       #set the dimensions for destination image. set one dimension to 0!
imgQuality = 80                                                                         #quality of new image
logOnly = False                                                                         #enable or disable logging to file. disables normal output to screen
skipExisting = False                                                                    #if file exists already, just skip
debug = False                                                                           #activate to get some help for error search
numberOfThreads = getGoodThreadValue()                                                  #algo to choose a good number of threads. you can also replace it by any number but 2*core-2 seems to be best working
#end of SETTINGS *** only change values in the area above



#* * * don't touch below * * *#
directory = os.path.abspath(".")                                                        #absolute path to current directory

if logOnly: sys.stdout = open(directory + '\\log.txt', 'a')

count_img = 0
count_vid = 0
count_skip = 0
count_error = 0

class work4me(threading.Thread):
    wresult = {}
    wlock = threading.Lock()
    wqueue = Queue.Queue(maxsize=-1)
    plock = threading.Lock()
    
    def run(self):
        while True:
            args = work4me.wqueue.get()
            r = self.shrink(args)
            
            work4me.wlock.acquire()
            work4me.wresult[args] = r
            work4me.wlock.release()
            
            work4me.wqueue.task_done()
            
    def shrink(self, args):
        global count_img
        global count_vid
        global count_skip
        global count_error
        
        #split args
        dir_from = args[0]
        dir_to = args[1]
        file = args[2]
        
        file_from = dir_from + "\\" + file
        file_to = dir_to + "\\" + file

        if not os.access(file_to, os.F_OK) or not skipExisting:
            if string.lower(os.path.splitext(file)[1]) in extList_img:                  #image
                try:
                    img_in = Image.open(file_from)
                except IOError:
                    work4me.plock.acquire()
                    print("[ERROR]", file, "threw error 'cannot identify image'")
                    count_error+=1
                    work4me.plock.release()
                    return "error"
                if (img_in.size[0]<10000 and img_in.size[1]<10000):
                    if (int(newSize[0])==0):                                            #resize to a defined height
                        if (img_in.size[1]<=newSize[1]):
                            if not os.access(file_to, os.F_OK) or not skipExisting:
                                shutil.copyfile(file_from, file_to)
                            work4me.plock.acquire()
                            print("[IMG]  ", file, "already small enough. just copied")
                            count_img+=1
                            work4me.plock.release()
                        else:
                            newSize_do = int((float(newSize[1])/img_in.size[1])*img_in.size[0]) , int(newSize[1])
                            img_out = ImageOps.fit(img_in, newSize_do, Image.ANTIALIAS)
                            img_out.save(file_to, quality=int(imgQuality))
                            work4me.plock.acquire()
                            print("[IMG]  ", file, "|", img_in.size, "->", newSize_do)
                            count_img+=1
                            work4me.plock.release()
                    elif (int(newSize[1])==0):                                          #resize to a defined width
                        if (img_in.size[0]<=newSize[0]):
                            if not os.access(file_to, os.F_OK) or not skipExisting:
                                shutil.copyfile(file_from, file_to)
                            work4me.plock.acquire()
                            print("[IMG]  ", file, "already small enough. just copied")
                            count_img+=1
                            work4me.plock.release()
                        else:
                            newSize_do = int(newSize[0]), int((float(newSize[0])/img_in.size[0])*img_in.size[1])
                            img_out = ImageOps.fit(img_in, newSize_do, Image.ANTIALIAS)
                            img_out.save(file_to, quality=int(imgQuality))
                            work4me.plock.acquire()
                            print("[IMG]  ", file, "|", img_in.size, "->", newSize_do)
                            count_img+=1
                            work4me.plock.release()
                    else:
                        work4me.plock.acquire()
                        print("Error on resize! Wrong dimensions in 'newSize' setting!")
                        work4me.plock.release()
                        raw_input("Press Enter to exit...")
                        sys.exit(1)
                else:
                    work4me.plock.acquire()
                    print("[ERROR]", file, "too large", img_in.size)
                    count_error+=1
                    work4me.plock.release()
                if (debug and not logOnly): raw_input("Press Enter to continue...")
            elif string.lower(os.path.splitext(file)[1]) in extList_vid:                #video
                if not os.access(file_to, os.F_OK) or not skipExisting:
                    shutil.copyfile(file_from, file_to)
                work4me.plock.acquire()
                print("[VID]  ", file, "just copied")
                count_vid+=1
                work4me.plock.release()
            else:                                                                       #something to skip/don't do anything
                work4me.plock.acquire()
                print("[SKIP] ", file)
                count_skip+=1
                work4me.plock.release()
        else:
            work4me.plock.acquire()
            print("[SKIP] ", file, "already exists"                                     #skip file if option is set and file already exists)
            count_skip+=1
            work4me.plock.release()
        return "done"

myThreads = [work4me() for i in range(numberOfThreads)]
for t in myThreads:
    t.setDaemon(True)
    t.start()

for folder, subfolders, files in os.walk(dirBig):
    dir_from = os.path.abspath(folder)
    dir_to = os.path.abspath(folder.replace(dirBig, dirSmall))
    if not os.access(dir_to, os.F_OK):
        os.mkdir(dir_to)                                                                #create folder if it doesn't exist
        print("[MKDIR]", dir_to)
    print("[CHDIR]", dir_to)
    myThreads = [work4me() for i in range(numberOfThreads)]
    for file in files:
        args = (dir_from, dir_to, file)
        work4me.wresult[args] = "working"
        work4me.wqueue.put(args)
  
    work4me.wqueue.join()
    with work4me.wqueue.mutex: work4me.wqueue.queue.clear()
    work4me.wresult = {}
        
    if (debug and not logOnly): raw_input("Press Enter to continue...")
print("\n\nSummary:")
print("[IMG]  ", count_img)
print("[VID]  ", count_vid)
print("[SKIP] ", count_skip)
print("[ERROR]", count_error)
if logOnly: sys.stdout.close()
if (not logOnly): raw_input("Press Enter to exit...")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
