---
title: tkinter example pidgin all dbus messages (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'pidgin all dbus messages'

Functions in program: 
* `def display_msg(sender, message, conversation, prompt_colour, msg_colour):    `
* `def sent_msg(account, conversation, message):    `
* `def received_msg(account, sender, message, conversation, flags):   `
* `def messages_update():`

Modules used in program: 
* `import tkinter.scrolledtext as tkst`
* `import tkinter as tk`

## python pidgin all dbus messages

Python tkinter example: pidgin all dbus messages

```python
#!/usr/bin/env python

# to debug dbus, try dbus-monitor "type='signal',sender='im.pidgin.purple.PurpleService',interface='im.pidgin.purple.PurpleInterface',member='ReceivedImMsg'"

import tkinter as tk
import tkinter.scrolledtext as tkst
from pydbus import SessionBus
from gi.repository import GObject
from gi.repository import GLib

SENT_PROMPT = 'prompt_sent'
SENT = 'msg_sent'
RECEIVED = 'msg_received'
RECEIVED_PROMPT = 'prompt_received'
RECEIVED_CHAT = 'msg_chat_received'
RECEIVED_CHAT_PROMPT = 'prompt_chat_received'

root = tk.Tk()
frame = tk.Frame(master = root)
frame.pack(fill='both', expand='yes')
messages = tkst.ScrolledText(master = frame, wrap = tk.WORD, width  = 100, height = 20)

messages.pack(padx=10, pady=10, fill=tk.BOTH, expand=True)
messages.tag_config(RECEIVED_PROMPT, foreground='red')
messages.tag_config(RECEIVED, foreground='black')
messages.tag_config(RECEIVED_CHAT, foreground='brown')
messages.tag_config(RECEIVED_CHAT_PROMPT, foreground='orange')
messages.tag_config(SENT_PROMPT, foreground='blue')
messages.tag_config(SENT, foreground='green')
messages.insert(tk.END, 'start of messages\n', SENT)
messages.update()

bus = SessionBus()
purple = bus.get('im.pidgin.purple.PurpleService', '/im/pidgin/purple/PurpleObject')

def messages_update():
    messages.update()
    GLib.timeout_add(100, messages_update)

def received_msg(account, sender, message, conversation, flags):   
    msg_colour = RECEIVED
    prompt_colour = RECEIVED_PROMPT
    try:
        if purple.PurpleConversationGetType(conversation) == 2:
            msg_colour = RECEIVED_CHAT
            prompt_colour = RECEIVED_CHAT_PROMPT
    except:
        pass
                
    display_msg(sender, message, conversation, prompt_colour, msg_colour)
    
def sent_msg(account, conversation, message):    
    sender = purple.PurpleAccountGetUsername(account) 
    display_msg(sender, message, None, SENT_PROMPT, SENT)

def display_msg(sender, message, conversation, prompt_colour, msg_colour):    
    print(sender, 'said:', message)
    if conversation:    
        try:
            title = purple.PurpleConversationGetTitle(conversation)
        except:           
            title = '#'    
    else:
        title = '*'  
    messages.configure(state='normal')
    messages.insert(tk.END, title + '>' + sender.split('@')[0] + ": ", prompt_colour)
    messages.insert(tk.END, message + '\n', msg_colour)
    messages.see(tk.END)
    messages.configure(state='disabled')

purple.ReceivedImMsg.connect(received_msg)
purple.ReceivedChatMsg.connect(received_msg)
purple.SentImMsg.connect(sent_msg)
purple.SentChatMsg.connect(sent_msg)

GLib.timeout_add(100, messages_update)
GObject.MainLoop().run()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
