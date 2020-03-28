---
title: tkinter example gistfile2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'gistfile2'

Functions in program: 
* `def main():`
* `def getValidProxy():`
* `def getProxy():`
* `def consume(infile):`
* `def start_thread(func, *args):`

Modules used in program: 
* `import socket`
* `import random`
* `import subprocess, sys, shlex, os`

## python gistfile2

Python tkinter example: gistfile2

```python
#!usr/bin/env python3.4.2
# -*- coding:utf-8 -*-
programname = 'BBCproxyv1.py'
programdate = '18-05-2015'
sub = 'get_iplayer'
# file location for video-recording
# for Windows
recordirw = 'D:/Video-lib/'
# for Linux
recordirl = '/home/$USER/Video/'
import subprocess, sys, shlex, os
import random
try:
    import urllib.request as urllib2
except:
    import urllib2
try:
    import bs4 as BeautifulSoup
except:
    import bs4 as beautifulsoup
import socket
from threading   import Thread
si=None
ON_POSIX = 'posix' in sys.builtin_module_names
Finished = False
listeUserAgents = [ 'Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_5_5; fr-fr) AppleWebKit/525.18 (KHTML, like Gecko) Version/3.1.2 Safari/525.20.1',
                                                'Mozilla/5.0 (Windows NT 6.1) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.186 Safari/535.1',
                                                'Mozilla/5.0 (Windows; U; Windows NT 6.0; en-US) AppleWebKit/525.13 (KHTML, like Gecko) Chrome/0.2.149.27 Safari/525.13\
',
                                                'Mozilla/5.0 (X11; U; Linux x86_64; en-us) AppleWebKit/528.5+ (KHTML, like Gecko, Safari/528.5+) midori',
                                                'Mozilla/5.0 (Windows NT 6.0) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/13.0.782.107 Safari/535.1',
                                                'Mozilla/5.0 (Macintosh; U; PPC Mac OS X; en-us) AppleWebKit/312.1 (KHTML, like Gecko) Safari/312',
                                                'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.12 Safari/535.11',
                                                'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.8 (KHTML, like Gecko) Chrome/17.0.940.0 Safari/535.8' ]
allTimeouts = (3, 10, 15, 20)
if sys.platform != 'linux':
    progloc = (os.environ['PROGRAMFILES'])
class FlushFile(object):
    """Write-only flushing wrapper for file-type objects."""
    def __init__(self, f):       
        self.f = f
    def write(self, x):
        self.f.write(x)
        self.f.flush()
def start_thread(func, *args):
    t = Thread(target=func, args=args)
    t.daemon = True
    t.start()
    return t
def consume(infile):
    global Finished
    for line in iter(infile.readline, ''):
        if 'Downloaded Thumbnail to ' in line:
            print(('!@!@!@if1'))
            Finished = True 
        if 'Recorded ' in line:
            print(('!@!@!@!if2'))
            Finished = True
        if 'Finished writing to temp file.' in line:
            print(('!@!@!@if3')    )
            Finished = True
        if 'Already in history' in line:
            print(('!@!@!@if4'))
            Finished = True   
        print((line,end=""))
    infile.close()
def getProxy():
    opener = urllib2.build_opener()
    opener.addheaders = [('User-agent', random.choice(listeUserAgents))]
    data = opener.open('http://free-proxy-list.net/uk-proxy.html').read()
    opener.close()
    soup = BeautifulSoup.BeautifulSoup(data)
    lst = []
    for tr in soup.tbody.findAll('tr'):
        i = 0
        slst = []
        for td in tr.find_all('td'):
            i += 1
            if i in (1, 2, 5):
                slst.append(td.contents[0])
            elif i == 8:
                i = 0
                lst.append(slst)
                slst = []
    for href in lst:
        yield href
def getValidProxy():
    for timeout in allTimeouts:
        print(('Timeout =', timeout))
        socket.setdefaulttimeout(timeout)
        for host, port, typeproxy in getProxy():
            try:
                print(('Trying %s:%s' % (host, port)))
                params = (host, int(port))
#                buffer_size = 1024
                s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                s.connect(params)
                s.close()
                yield host, port, typeproxy
            except socket.timeout:
                pass
            except socket.error:
                pass
def main():
    global Finished
#    # Replace stdout with an automatically flushing version
    sys.stdout = FlushFile(sys.__stdout__)
    idvideo = sys.argv[1]
    for host, port, typeproxy in getValidProxy():
        print((host, port, typeproxy))
        idvideo = sys.argv[1]
        tstforce = sys.argv[2]
        tstradio = sys.argv[3]
        if tstradio == "1":
            #modus = " --type=radio "
            #modus = " --mode=best"
            #modus = " --modes=wma1"
            modus = " --aactomp3"
        else:
            modus = " --subtitles --mode=best"
        if sys.platform == 'linux':
            if len(sys.argv) > 1 and sys.argv[2] == '1':
                cmds = "get_iplayer --get "+idvideo + modus + \
                       "  --attempt=99 --output="+ recordirl + \
                       " --nopurge  --force --proxy=http://"+host+":"+port
            else:
                cmds = "get_iplayer --get "+idvideo + modus + \
                       "  --attempt=99 --output="+ recordirl + \
                       " --nopurge  --proxy=http://"+host+":"+port
           # argments = cmds.split
            arguments = shlex.split(cmds)
            Finished = False
#            print(('cmds=',cmds))
            print(('arguments = ',arguments)                                                         )
            process = subprocess.Popen(args=arguments, stdout=subprocess.PIPE, stderr=subprocess.PIPE,
                                       universal_newlines=True)
        else:
            #print(("loc = ", progloc))
            if len(sys.argv) > 1 and tstforce == '1':
                cmds = "\"" + progloc + "/get_iplayer/get_iplayer.cmd \"" + " --get  " + idvideo + modus + \
                       " --check duration  --attempt=99 --output=" +\
                recordirw + " --nopurge --force --proxy=http://" + host + ":" + port
            else:
                cmds = "\"" + progloc + "/get_iplayer/get_iplayer.cmd \"" + " --get  " + idvideo + modus + \
                       " --check duration --attempt=99  --output=" + \
                recordirw + " --nopurge  --proxy=http://" + host + ":" + port
                #cmds = "\"C:/Program Files/get_iplayer/get_iplayer.cmd \""                 #test syntax
                #cmds = "\"" + progloc + "/get_iplayer/get_iplayer.cmd \"" + " -help"
            Finished = False
            print(('cmds=',cmds))
            si=subprocess.STARTUPINFO()                                             # Hide Windows Command Window!
            si.dwFlags |= subprocess.STARTF_USESHOWWINDOW
            si.wShowWindow = subprocess.SW_HIDE
            process = subprocess.Popen(args=cmds, stdout=subprocess.PIPE, stderr=subprocess.PIPE,
                                       startupinfo=si, universal_newlines=True)
            process.communicate
        #(stdout, stderr ) = process.communicate()    
        pid = process.pid
        print(('le pid de get_iplayer est ', pid))
        thread1 = start_thread(consume, process.stdout)
        thread2 = start_thread(consume, process.stderr)
        thread1.join() # wait for IO completion
        thread2.join() # wait for IO completion
        if Finished:
                for n in range(1,1):
                    print(('******Download Finished, Make another Choice, or Quit the program, or Select AGAIN and mark FORCE to repeat a download'))
                    print(('******if you repeat the download make sure the (partial)file is deleted first'))
                print((sub + '******Download Finished, Make another Choice'))
                exitmessage = 'Download Finished, Make another Choice'
                exitmessage.set('the file is downloaded now') 
                root.update()
                thread1.close()
                thread2.close()
                break;                                          
if __name__ == "__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
