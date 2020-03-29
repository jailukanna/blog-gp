---
title: pygtk example pygtk compatibility (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'pygtk compatibility'

Functions in program: 
* `def click(button):`

Modules used in program: 
* `import gi`

## python pygtk compatibility

Python pygtk example: pygtk compatibility

```python
GTK_VER = '2.0'
USE_STOCK = False

# Descomente abaixo para PyGtk 2.0 com Python 2.x e comente o PyGObject

# Aqui usa-se PyGtk e troca-se de "gtk" para "Gtk" "compatibilizando" o código
# import pygtk
# pygtk.require(GTK_VER)
# import gtk as Gtk
# print(Gtk.ver)

# Aqui usa-se PyGObject e pode-se varia a versão do GTK+ em 2.x, 3.x ou 4.x
import gi
gi.require_version('Gtk', GTK_VER)
from gi.repository import Gtk
print(Gtk.MAJOR_VERSION, Gtk.MINOR_VERSION, Gtk.MICRO_VERSION)

# Mostrar as imagens nos botões com PyGtk ou PyGObject, não rola na 4.x
if 'settings_get_default' in Gtk.__dict__:
    Gtk.settings_get_default().props.gtk_button_images = True
elif GTK_VER in ('2.0', '3.0'):
    Gtk.Settings.get_default().props.gtk_button_images = True


# Função executada ao clicar no botão "Fechar"
def click(button):
    window = button.get_toplevel()
    window.destroy()
    Gtk.main_quit()


# Construção básica de exemplo de uma janela
window = Gtk.Window()
window.set_size_request(400, 400)
window.props.title = "Teste"
window.connect('delete-event', Gtk.main_quit)
window.show()

# Construção básica de um botão
# Muita coisa retirada no 4.x, stock é uma, então...
if USE_STOCK:
    print("Usando stock...")
    button = Gtk.Button('gtk-close')
    button.props.use_stock = True
    button.props.border_width = 100
else:
    print("Sem stock...")
    if GTK_VER == '2.0':
        align = Gtk.Alignment()
        align.props.xscale = 0
        align.props.yscale = 0
        align.props.xalign = 0.5
        align.props.yalign = 0.5
        align.show()
        box = Gtk.HBox(spacing=2)
        button = Gtk.Button()
        button.props.border_width = 100
        align.add(box)
        button.add(align)
    else:
        box = Gtk.Box(orientation=Gtk.Orientation.HORIZONTAL, spacing=2,
                      halign=Gtk.Align.CENTER)
        button = Gtk.Button(margin=100)
        button.add(box)
    image = Gtk.Image()
    image.props.icon_name = 'window-close'
    box.add(image)
    label = Gtk.Label('_Fechar')
    label.props.use_underline = True
    box.add(label)
    if GTK_VER in ('2.0', '3.0'):  # 2 e 3 com show_all, show_all retirado na 4
        box.show_all()
button.connect('clicked', click)
button.show()
window.add(button)

# Executa o "main loop" para integragir com o usuário e rederizar os widgets
Gtk.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
