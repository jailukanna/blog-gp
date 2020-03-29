---
title: pygtk example pesquisa opiniao (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'pesquisa opiniao'


Modules used in program: 
* `import glib`
* `import gtk`
* `import pygtk`

## python pesquisa opiniao

Python pygtk example: pesquisa opiniao

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import pygtk
pygtk.require("2.0")
import gtk
import glib

class Projeto(gtk.Window):
    def __init__(self):
        super(Projeto, self).__init__()
        # Widgets
        vb = gtk.VBox()
        sw = gtk.ScrolledWindow()
        tv = gtk.TextView()
        hb = gtk.HButtonBox()
        bp = gtk.Button(stock = 'gtk-media-play')
        bq = gtk.Button(stock = 'gtk-quit')
        # Leiaute
        self.add(vb)
        vb.pack_start(sw)
        sw.add(tv)
        vb.pack_end(hb, expand = False)
        hb.pack_start(bp)
        self.set_border_width(5)
        vb.set_spacing(5)
        hb.set_spacing(5)
        sw.set_shadow_type(gtk.SHADOW_IN)
        sw.set_policy(gtk.POLICY_AUTOMATIC, gtk.POLICY_AUTOMATIC)
        tv.set_editable(False)
        self.set_title('Pesquisa de opinião - Junior Polegato')
        self.resize(800, 600)
        self.set_position(gtk.WIN_POS_CENTER)
        # Botões especiais
        for i in range(1, 6):
            b = gtk.Button('Nota %i' % i)
            hb.pack_start(b)
            b.connect('clicked', self.ao_clicar_num_botao, i)
        hb.pack_end(bq)
        # Conectar sinais/evento à funções
        self.connect('delete-event', self.sair)
        bp.connect('clicked', self.iniciar)
        bq.connect('clicked', self.sair)
        # Variáveis da instância
        self.loop = None
        self.buf = tv.get_buffer()
        # Mostrar janela com todos os widgets
        self.show_all()

    def sair(self, *args):
        d = gtk.MessageDialog(parent = self, flags = 0,
                              type = gtk.MESSAGE_QUESTION,
                              buttons = gtk.BUTTONS_YES_NO,
                              message_format = "Deseja realmente sair?")
        resposta = d.run()
        d.destroy()
        if resposta == gtk.RESPONSE_YES:
            self.escolha = 0
            if self.loop:
                self.loop.quit()
            gtk.main_quit()
            return False
        return True

    def iniciar(self, *args):
        self.buf.set_text('')
        for i in range(1, 11):
            self.buf.set_text(self.buf.get_text(*self.buf.get_bounds())+
                                'Qual sua nota para o sistema %i? ' % i)
            self.loop = glib.MainLoop()
            self.loop.run()
            self.loop = None
            if not self.escolha:
                break
            self.buf.set_text(self.buf.get_text(*self.buf.get_bounds())+ 
                                                  '%i\n' % self.escolha)
        self.buf.set_text(self.buf.get_text(*self.buf.get_bounds()) +
                                                            'Obrigado!')

    def ao_clicar_num_botao(self, botao, numero):
        print('Número:', numero)
        if self.loop:
            self.escolha = numero
            self.loop.quit()
        else:
            d = gtk.MessageDialog(parent = self, flags = 0,
                              type = gtk.MESSAGE_WARNING,
                              buttons = gtk.BUTTONS_OK,
                              message_format = "Pressione Reproduzir!")
            d.run()
            d.destroy()

if  __name__ == "__main__":
    projeto = Projeto()
    gtk.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
