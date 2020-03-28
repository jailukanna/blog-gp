---
title: tkinter example CHAT.merged (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'CHAT.merged'


Modules used in program: 
* `import tkinter.messagebox`
* `import tkinter`

## python CHAT.merged

Python tkinter example: CHAT.merged

```python
from socket import AF_INET, socket, SOCK_STREAM
from threading import Thread
import tkinter
import tkinter.messagebox

class Server:
    def run_server(self):        
        clients = {}
        addresses = {}
        HOST = ''
        PORT = 33000
        BUFSIZ = 1024
        ADDR = (HOST, PORT)
        SERVER = socket(AF_INET, SOCK_STREAM)
        SERVER.bind(ADDR)
        if __name__ == "__main__":
            SERVER.listen(5)
            print("Waiting for connection...")
            ACCEPT_THREAD = Thread(target=accept_incoming_connections)
            ACCEPT_THREAD.start()
            ACCEPT_THREAD.join()
            SERVER.close()
            
    def accept_incoming_connections():
        while True:
            client, client_address = SERVER.accept()
            print("%s:%s has connected." % client_address)
            client.send(bytes("Welcome to the chat! Enter your name to begine chatting.", "utf8"))
            addresses[client] = client_address
            Thread(target=handle_client, args=(client,)).start()


    def handle_client(self, client):  # Takes client socket as argument.
        name = client.recv(BUFSIZ).decode("utf8")
        welcome = 'Welcome %s! If you ever want to quit, type {quit} to exit.' % name
        client.send(bytes(welcome, "utf8"))
        msg = "%s has joined the chat!" % name
        broadcast(bytes(msg, "utf8"))
        clients[client] = name
    
        while True:
            msg = client.recv(BUFSIZ)
            if msg != bytes("{quit}", "utf8"):
                broadcast(msg, name+": ")
            else:
                client.send(bytes("{quit}", "utf8"))
                client.close()
                del clients[client]
                broadcast(bytes("%s has left the chat." % name, "utf8"))
                break
    
    
    def broadcast(self, msg, prefix=""):  # prefix is for name identification.
        for sock in clients:
            sock.send(bytes(prefix, "utf8")+msg)

class Client:   
    def receive(self):
        while True:
            try:
                msg = client_socket.recv(BUFSIZ).decode("utf8")
                msg_list.insert(tkinter.END, msg)
            except OSError:  # Possibly client has left the chat.
                break
    
    
    def send(self, event=None):  # event is passed by binders.
        msg = my_msg.get()
        my_msg.set("")  # Clears input field.
        client_socket.send(bytes(msg, "utf8"))
        if msg == "{quit}":
            client_socket.close()
            top.quit()
    
    
    def on_closing(self, event=None):
        my_msg.set("{quit}")
        send()
    
    def run_client(self):  
        top = tkinter.Tk()
        top.title("Chat4u")
        
        messages_frame = tkinter.Frame(top)
        my_msg = tkinter.StringVar()  # For the messages to be sent.
        my_msg.set("Type your messages here.")
        scrollbar = tkinter.Scrollbar(messages_frame)  # To navigate through past messages.
        # Following will contain the messages.
        msg_list = tkinter.Listbox(messages_frame, height=15, width=50, yscrollcommand=scrollbar.set)
        scrollbar.pack(side=tkinter.RIGHT, fill=tkinter.Y)
        msg_list.pack(side=tkinter.LEFT, fill=tkinter.BOTH)
        msg_list.pack()
        messages_frame.pack()
        
        entry_field = tkinter.Entry(top, textvariable=my_msg)
        entry_field.bind("<Return>", send) #Makes "send" work with Enter
        entry_field.pack()
        send_button = tkinter.Button(top, text="Send", command=send)
        send_button.pack()
        
        top.protocol("WM_DELETE_WINDOW", on_closing)
        
        #----Now comes the sockets part----
        HOST = input('Enter an IP host: ')
        PORT = input('Enter a port that is free (e.g.: 33000): ')
        if not PORT:
            PORT = 33000
        else:
            PORT = int(PORT)
            
        BUFSIZ = 1024
        ADDR = (HOST, PORT)
        
        client_socket = socket(AF_INET, SOCK_STREAM)
        client_socket.connect(ADDR)
        
        receive_thread = Thread(target=receive)
        receive_thread.start()
        tkinter.mainloop()  # Starts GUI execution.
        
answer=tkinter.messagebox.askquestion("Finger Chess", "Are you the host?")

if answer=="yes": #server code
    s=Server()
    s.run_server()
else:
    c=Client()
    c.run_client()
   
    

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
