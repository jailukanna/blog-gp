---
title: pygtk example limpeza de gtk entrys por hierarquia (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'limpeza de gtk entrys por hierarquia'

Functions in program: 
* `def resposta_ao_clique(button):`
* `def limpar_entrys_de(widget, default='', nivel=0):`

## python limpeza de gtk entrys por hierarquia

Python pygtk example: limpeza de gtk entrys por hierarquia

```python
#!/bin/env python
# -*- coding: utf-8 -*-

# Para o print(funcionar no Python 2 e 3)
from __future__ import print_function

# Importa Gtk versão 3, senão possível tenta versão 2
try:
    import gi
    gi.require_version('Gtk', '3.0')
    from gi.repository import Gtk as gtk
    print('Gtk 3')
    settings = gtk.Settings.get_default()
except Exception:
    import pygtk
    pygtk.require('2.0')
    import gtk
    print('Gtk 2')
    settings = gtk.settings_get_default()


# Função para limpar entrys filhos de um widget
def limpar_entrys_de(widget, default='', nivel=0):
    # Bloco para mostrar a hierarquia da janela
    deslocamento = '  ' * nivel
    print(deslocamento + widget.get_name(), '=>',
          'Entry:', isinstance(widget, gtk.Entry), '|',
          'Bin:', isinstance(widget, gtk.Bin), '|',
          'Container:', isinstance(widget, gtk.Container))

    # Se for um Entry, limpa o mesmo
    if isinstance(widget, gtk.Entry):
        widget.set_text(default)
    # Se for um widget com filhos, pede para limpar os filhos
    elif isinstance(widget, (gtk.Bin, gtk.Container)):
        for child in widget.get_children():
            limpar_entrys_de(child, default, nivel + 1)


# Função chamada pelo clique do botão
def resposta_ao_clique(button):
    # Limpar os entrys da janela (toplevel) deste botão
    limpar_entrys_de(button.get_toplevel())


# Widgets
w = gtk.Window()
v = gtk.VBox()
e1 = gtk.Entry()
e2 = gtk.Entry()
h = gtk.HBox()
l = gtk.Label('Entrys:')
e3 = gtk.Entry()
e4 = gtk.Entry()
a = gtk.Alignment()
e5 = gtk.Entry()
e6 = gtk.Entry()
b = gtk.Button()

# Colocando textos de teste
for i, e in enumerate([e1, e2, e3, e4, e5, e6]):
    e.set_text('Teste %i' % i)

# Configurando o botão e saída [X] da janela
settings.props.gtk_button_images = True
b.set_label('gtk-clear')
b.set_use_stock(True)
b.connect('clicked', resposta_ao_clique)
w.connect('delete-event', gtk.main_quit)

# Montando a hierarquia da janela
w.add(v)
v.pack_start(e1, True, True, True)
v.pack_start(e2, True, True, True)
v.pack_start(h, True, True, True)
h.pack_start(l, True, True, True)
h.pack_start(e3, True, True, True)
h.pack_start(e4, True, True, True)
h.pack_start(a, True, True, True)
a.add(e5)
h.pack_end(e6, True, True, True)
v.pack_end(b, True, True, True)

# Mostrando tudo
w.show_all()

# Iniciando o loop do gtk
gtk.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
