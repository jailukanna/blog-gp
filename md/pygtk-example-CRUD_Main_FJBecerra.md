---
title: pygtk example CRUD Main FJBecerra (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'CRUD Main FJBecerra'


Modules used in program: 
* `import gtk`
* `import pygtk`

## python CRUD Main FJBecerra

Python pygtk example: CRUD Main FJBecerra

```python
import pygtk
pygtk.require("2.0")
import gtk

mainVersion = 0.5 # Versión Activa

class GUI(object):       
        def __init__(self):
                self.gtkBuilder = gtk.Builder() # Creamos
                self.gtkBuilder.add_from_file("CRUD_Main_FJBecerra.glade") # Cargamos del interfaz
                # Asignamos los handlers que utilizaremos en el código
                self.handlers = self.gtkBuilder.connect_signals({"on_bAcercaDe_clicked" : self.on_bAcercaDe_clicked})
                # Conectamos los handlers asignados
                self.gtkBuilder.connect_signals(self.handlers)
                self.window = self.gtkBuilder.get_object("main_window") # Accedemos a la ventana principal
                self.window.connect('destroy', self.destroy)
                self.windowsTemp = self.gtkBuilder.get_object("wAcercaDe")
                self.window.show() # Visualizar la ventana principal


        def destroy(self,window):
                gtk.main_quit()


        # Función: on_bAcercaDe
        # Descripcion: Muetra la ventana "Acerca de" -> wAcercaDe 
  # Asociada al handler: on_bAcercaDe_clicked
        # Parámetros: button    -> GtkButton
        def on_bAcercaDe_clicked(self,button):                
                self.response = self.windowsTemp.run()
                self.windowsTemp.hide()
        
               
        def main(self):
                gtk.main()

if __name__ == "__main__":
        app = GUI()
        app.main()
        




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
