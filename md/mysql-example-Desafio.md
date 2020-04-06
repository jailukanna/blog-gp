---
title: mysql example Desafio (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Desafio'


Modules used in program: 
* `import _mysql`
* `import gtk`
* `import pygtk`

## python Desafio

Python mysql example: Desafio

```python
#!/usr/bin/env python
# Coded By MMxM

import pygtk
pygtk.require('2.0')
import gtk
import _mysql
from hashlib import sha1

class Gui:
	def usuario(self, abc):
		nome = self.edit4.get_text()
		login = self.edit5.get_text()
		senha = self.edit6.get_text()
		crypt = sha1(senha).hexdigest()
		email = self.edit7.get_text()

		try:
			con = _mysql.connect(self.edit1.get_text(),self.edit2.get_text(),self.edit3.get_text(),"desafio")
			d = {'"':'\\"', "'":"\\'", "\0":"\\\0", "\\":"\\\\"}
			nome = ''.join(d.get(c, c) for c in nome)
			login = ''.join(d.get(c, c) for c in login)
			email = ''.join(d.get(c, c) for c in email)
			cmd = "INSERT INTO `usuarios` VALUES  (NULL, '%s', '%s', '%s', '%s')" %(nome,login,crypt,email)
			con.query(str(cmd))
			result = con.use_result()
			ok = gtk.MessageDialog(None ,gtk.DIALOG_DESTROY_WITH_PARENT, gtk.MESSAGE_INFO, gtk.BUTTONS_CLOSE, "Usuario Registrado com Sucesso !!!")
			ok.run()
			ok.destroy()

		except:
			fail = gtk.MessageDialog(None , gtk.DIALOG_DESTROY_WITH_PARENT, gtk.MESSAGE_ERROR, gtk.BUTTONS_CLOSE, "Erro ao Registrar Usuario !!!")
			fail.run()
			fail.destroy()		

	def hide(self):

		self.window.hide()

		self.new = gtk.Window(gtk.WINDOW_TOPLEVEL)
		self.new.set_size_request(300, 210)
		self.new.set_title("Desafio do C0de")
                self.new.set_position(gtk.WIN_POS_CENTER)
		self.new.set_resizable(False)

		fix = gtk.Fixed()
		
		self.bexit = gtk.Button(stock=gtk.STOCK_CLOSE)
		self.bt1 = gtk.Button("Adicionar")
                self.lb1 = gtk.Label("Adicionar novo usuario: ")
                self.lb2 = gtk.Label("Nome:")
                self.lb3 = gtk.Label("Login:")
		self.lb4 = gtk.Label("Senha:")
		self.lb5 = gtk.Label("Email:")
		self.edit4 = gtk.Entry()
		self.edit5 = gtk.Entry()
		self.edit6 = gtk.Entry()
		self.edit7 = gtk.Entry()

		self.bt1.set_tooltip_text("Adicionar novo usuario")

                self.bt1.connect("clicked", self.usuario)
                self.bexit.connect("clicked", gtk.main_quit)
                self.edit4.set_size_request(200, 25)
                self.edit5.set_size_request(200, 25)
                self.edit6.set_size_request(200, 25)
		self.edit7.set_size_request(200, 25)

                fix.put(self.bexit, 180, 160)
		fix.put(self.bt1, 100, 160)
		fix.put(self.lb1, 50, 2)
                fix.put(self.lb2, 10, 25)
                fix.put(self.lb3, 10, 55)
                fix.put(self.lb4, 10, 85)
		fix.put(self.lb5, 10, 115)
                fix.put(self.edit4, 56, 25)
                fix.put(self.edit5, 56, 55)
                fix.put(self.edit6, 56, 85)
		fix.put(self.edit7, 56, 115)

                self.new.add(fix)
		self.new.show_all()
		self.new.connect("destroy",self.destroy)

	def destroy(self, widget, data=None):
		gtk.main_quit()

	def __init__(self):
        	self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
        	self.window.set_position(gtk.WIN_POS_CENTER)
		self.window.set_size_request(300, 160)
		self.window.set_title("Desafio do C0de")
		self.window.set_resizable(False)

		fix = gtk.Fixed()

		self.lb1 = gtk.Label("Host:")
		self.lb2 = gtk.Label("User:")
		self.lb3 = gtk.Label("Senha:")
		self.edit1 = gtk.Entry()
		self.edit2 = gtk.Entry()
		self.edit3 = gtk.Entry()
		self.bt1 = gtk.Button("Conectar")
		self.bexit = gtk.Button(stock=gtk.STOCK_CLOSE)
		
		self.bt1.set_tooltip_text("Conectar ao Banco de Dados")
		
		self.bt1.connect("clicked", self.sql)
		self.bexit.connect("clicked", gtk.main_quit)
		self.edit1.set_size_request(200, 25)
		self.edit2.set_size_request(200, 25)
		self.edit3.set_size_request(200, 25)

		fix.put(self.bt1, 100, 120)
		fix.put(self.bexit, 180, 120)
		fix.put(self.lb1, 10, 15)
		fix.put(self.lb2, 10, 45)
		fix.put(self.lb3, 10, 75)		
		fix.put(self.edit1, 56, 15)
                fix.put(self.edit2, 56, 45)
                fix.put(self.edit3, 56, 75)

		self.window.add(fix)
		self.window.show_all()
		self.window.connect("destroy",self.destroy)

	def sql(self, widget):
        	host = self.edit1.get_text()
        	user  = self.edit2.get_text()
		senha = self.edit3.get_text()
		if host == '': host = 'indefinido'
                if user == '': user = 'indefinido'
                if senha == '': senha = 'indefinido'

		try:
			con = _mysql.connect(host, user, senha)
			self.hide()

		except _mysql.Error, e:
			erro = gtk.MessageDialog(None , gtk.DIALOG_DESTROY_WITH_PARENT, gtk.MESSAGE_ERROR, gtk.BUTTONS_CLOSE, "Erro ao conectar ao Banco de Dados\nTente Novamente")
			erro.run()
			erro.destroy()
			
	def main(self):
        	gtk.main()

if __name__ == "__main__":
	gui = Gui()
	gui.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
