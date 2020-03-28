---
title: tkinter example native example (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'native example'


## python native example

Python tkinter example: native example

```python
#!/usr/bin/env python3
# Copyright (c) 2012 The Chromium Authors. All rights reserved.
# Use of this source code is governed by a BSD-style license that can be
# found in the LICENSE file.
# Modified to work with python3 by Ross Jacobs
# A simple native messaging host. Shows a Tkinter dialog with incoming messages
# that also allows to send message back to the webapp.
try:
  import struct
  import sys
  import threading
  import queue as Queue
  try:
    import tkinter
    import tkinter.messagebox
  except ImportError:
    tkinter = None
  # Helper function that sends a message to the webapp.
  def send_message(message):
    # Write message size.
    sys.stdout.write(struct.pack('I', len(message)))
    # Write the message itself.
    sys.stdout.write(message)
    sys.stdout.flush()
  # Thread that reads messages from the webapp.
  def read_thread_func(queue):
    message_number = 0
    while True:
      # Read the message length (first 4 bytes).
      text_length_bytes = sys.stdin.read(4)
      if len(text_length_bytes) == 0:
        if queue:
          queue.put(None)
        sys.exit(0)
      # Unpack message length as 4 byte integer.
      text_length = struct.unpack('i', text_length_bytes)[0]
      # Read the text (JSON object) of the message.
      text = sys.stdin.read(text_length).decode('utf-8')
      if queue:
        queue.put(text)
      else:
        # In headless mode just send an echo message back.
        send_message('{"echo": %s}' % text)
  if tkinter:
    class NativeMessagingWindow(tkinter.Frame):
      def __init__(self, queue):
        self.queue = queue
        tkinter.Frame.__init__(self)
        self.pack()
        self.text = tkinter.Text(self)
        self.text.grid(row=0, column=0, padx=10, pady=10, columnspan=2)
        self.text.config(state=tkinter.DISABLED, height=10, width=40)
        self.messageContent = tkinter.StringVar()
        self.sendEntry = tkinter.Entry(self, textvariable=self.messageContent)
        self.sendEntry.grid(row=1, column=0, padx=10, pady=10)
        self.sendButton = tkinter.Button(self, text="Send", command=self.onSend)
        self.sendButton.grid(row=1, column=1, padx=10, pady=10)
        self.after(100, self.processMessages)
      def processMessages(self):
        while not self.queue.empty():
          message = self.queue.get_nowait()
          if message == None:
            self.quit()
            return
          self.log("Received %s" % message)
        self.after(100, self.processMessages)
      def onSend(self):
        text = '{"text": "' + self.messageContent.get() + '"}'
        self.log('Sending %s' % text)
        try:
          send_message(text)
        except IOError:
          tkinter.messagebox.showinfo('Native Messaging Example',
                                'Failed to send message.')
          sys.exit(1)
      def log(self, message):
        self.text.config(state=tkinter.NORMAL)
        self.text.insert(tkinter.END, message + "\n")
        self.text.config(state=tkinter.DISABLED)
  def Main():
    if not tkinter:
      send_message('"Tkinter python module wasn\'t found. Running in headless ' +
                  'mode. Please consider installing Tkinter."')
      read_thread_func(None)
      sys.exit(0)
    queue = Queue.Queue()
    main_window = NativeMessagingWindow(queue)
    main_window.master.title('Native Messaging Example')
    thread = threading.Thread(target=read_thread_func, args=(queue,))
    thread.daemon = True
    thread.start()
    main_window.mainloop()
    sys.exit(0)
  if __name__ == '__main__':
      Main()
except Exception as e:
  print(str(e))
  with open('/tmp/output', 'w') as f:
    f.write(str(e))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
