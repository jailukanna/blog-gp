---
title: tkinter example python decode encode tkinter (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'python decode encode tkinter'

Functions in program: 
* `def clicked_encrypt():`
* `def clicked_decrypt():`
* `def center(toplevel):`

Modules used in program: 
* `import tkinter as tk`
* `import tkinter as  tk`
* `import hashlib`
* `import base64`

## python python decode encode tkinter

Python tkinter example: python decode encode tkinter

```python
"""
If you have problems importing PyQt5, just comment:

# from PyQt5.QtWidgets import QApplication
# all def center(): ...
# center(rootWin)

It just centers the window on screen
"""

import base64
import hashlib

from Crypto import Random
from Crypto.Cipher import AES


class AESCipher:
	bs = AES.block_size

	def __init__(self, key):
		self.key = hashlib.sha256(key.encode()).digest()

	def encrypt(self, message):
		message = self._pad(message).encode()
		iv = Random.new().read(AES.block_size)
		cipher = AES.new(self.key, AES.MODE_CBC, iv)
		return base64.b64encode(iv + cipher.encrypt(message)).decode('utf-8')

	def decrypt(self, enc):
		enc = base64.b64decode(enc)
		iv = enc[:AES.block_size]
		cipher = AES.new(self.key, AES.MODE_CBC, iv)
		return self._unpad(cipher.decrypt(enc[AES.block_size:])).decode('utf-8')

	def _pad(self, s):
		return s + (self.bs - len(s) % self.bs) * chr(self.bs - len(s) % self.bs)

	@staticmethod
	def _unpad(s):
		return s[:-ord(s[len(s) - 1:])]

secret_key = "123"

import tkinter as  tk
from tkinter.scrolledtext import ScrolledText
from tkinter import Button
from tkinter import BOTH, END, LEFT, RIGHT



import tkinter as tk
from PyQt5.QtWidgets import QApplication

def center(toplevel):
	toplevel.update_idletasks()
	app = QApplication([])
	screen_width = app.desktop().screenGeometry().width()
	screen_height = app.desktop().screenGeometry().height()
	size = tuple(int(_) for _ in toplevel.geometry().split('+')[0].split('x'))
	x = screen_width/2 - size[0]/2
	y = screen_height/2 - size[1]/2
	toplevel.geometry("+%d+%d" % (x, y))
	toplevel.title("Text Encryption / Decryption")

rootWin = tk.Tk()
rootWin.geometry("800x500")
def clicked_decrypt():

	eText = textfield.get('1.0', END)
	textfield.delete('1.0', END)
	textfield2.delete('1.0', END)
	sec = e_password.get()
	aes_cipher = AESCipher(sec)
	decrypted = aes_cipher.decrypt(eText)
	textfield2.insert('insert', decrypted)


def clicked_encrypt():
	eText = textfield2.get('1.0', END)
	textfield.delete('1.0', END)
	textfield2.delete('1.0', END)
	sec = e_password.get()
	aes_cipher = AESCipher(sec)
	encrypted = aes_cipher.encrypt(eText)
	textfield.insert('insert', encrypted)



textfield2 = ScrolledText(width=100, height=12)
textfield2.pack()
textfield2.delete('1.0', END)

button2 = Button(rootWin, text="Encrypt", command=clicked_encrypt)
button2.pack()



textfield = ScrolledText(width=100, height=12)
textfield.pack()
textfield.delete('1.0', END)
button = Button(rootWin, text="Decrypt", command=clicked_decrypt)
button.pack()


l1 = tk.Label(rootWin, text="Password")
e_password = tk.Entry(rootWin, show="*", width=25)
e_password.insert("insert", secret_key )

l1.pack()
l1.place(x=10, y=450)
e_password.pack()
e_password.place(x=75, y=450)


center(rootWin)
rootWin.resizable(False, False)
rootWin.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
