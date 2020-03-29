---
title: pygtk example simple-taskbar (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'simple-taskbar'


Modules used in program: 
* `import pygtk`
* `import gtk`
* `import cairo`
* `import wnck`

## python simple-taskbar

Python pygtk example: simple-taskbar

```python
#!/usr/bin/python2.7
# coding=utf-8

import wnck

import cairo
import gtk
import pygtk

pygtk.require('2.0')


class MyTaskbar:
    @staticmethod
    def sort_windows(arg):
        group_name = arg.get_class_group().get_name()
        return group_name

    def area_draw(self, widget, *event):
        cr = widget.get_window().cairo_create()
        r, g, b, a = event[1]
        cr.set_source_rgba(r, g, b, a)
        cr.set_operator(cairo.OPERATOR_SOURCE)
        cr.paint()
        cr.set_operator(cairo.OPERATOR_OVER)
        return False

    def __init__(self):
        self.color_std = (.0, .0, .0, 0.8)  # цвет фона значка
        self.color_active_std = (.8, .8, .8, 1)  # цвет индикатора активного значка

        self.margin_y = 28  # 24 - стандарт, 28 - для MATE
        self.padding_y = 5
        self.footer_size = 15

        self.window = None
        self.table = None
        self.colormap = None
        self.panel_size = 30  # размер панели
        self.indicator_width = 1
        self.icon_size = self.panel_size - self.indicator_width - 4

        self.win_width = self.panel_size

        self.screen = wnck.screen_get_default()
        self.win_height = self.screen.get_height()

        self.colormap = gtk.Window().get_screen().get_rgba_colormap()

        self.window = self.create_window()

        self.update_bar()

        self.screen.connect("active-window-changed", self.event_update)
        self.screen.connect("window-closed", self.event_update)
        self.screen.connect("window-opened", self.event_update)

    def event_update(self, screen, window):
        self.update_bar()
        return

    def create_window(self):
        window = gtk.Window(gtk.WINDOW_TOPLEVEL)
        window.set_colormap(self.colormap)
        window.set_app_paintable(True)
        window.connect("expose-event", self.area_draw, self.color_std)

        window.set_type_hint(gtk.gdk.WINDOW_TYPE_HINT_DOCK)
        window.set_border_width(0)
        window.resize(self.win_width, self.win_height - self.margin_y)
        window.move(0, self.margin_y)
        window.set_has_frame(False)
        window.show()
        window.window.property_change("_NET_WM_STRUT", "CARDINAL", 32, gtk.gdk.PROP_MODE_REPLACE, [self.win_width, 0, 0, 0])
        return window

    def create_eventbox(self, colour, width, height):
        box = gtk.EventBox()
        box.set_colormap(self.colormap)
        box.set_app_paintable(True)
        box.set_size_request(width, height)
        box.connect("expose-event", self.area_draw, colour)
        return box

    def update_bar(self):
        self.screen.force_update()
        windows = self.screen.get_windows()
        windows_normal = []
        for w in windows:
            if w.get_window_type() == wnck.WINDOW_NORMAL:
                windows_normal.append(w)

        windows_normal = sorted(windows_normal, key=MyTaskbar.sort_windows, reverse=False)
        cnt = len(windows_normal)
        if cnt == 0:
            cnt = 1

        _table = gtk.Table(rows=cnt, columns=3, homogeneous=False)
        _table.set_row_spacings(self.padding_y)
        _table.set_col_spacings(0)

        if self.table is not None:
            self.window.remove(self.table)
        self.window.add(_table)
        idx = 0
        for w in windows_normal:
            if w.get_window_type() == wnck.WINDOW_NORMAL:
                pixbuf = w.get_icon()
                img = gtk.Image()
                box = self.create_eventbox(self.color_std, self.panel_size, self.panel_size)
                box.add(img)

                if not w.is_active():
                    active_box = self.create_eventbox(self.color_std, self.indicator_width, self.icon_size)
                else:
                    active_box = self.create_eventbox(self.color_active_std, self.indicator_width, self.icon_size)

                scaled_buf = pixbuf.scale_simple(self.icon_size, self.icon_size, gtk.gdk.INTERP_BILINEAR)
                img.set_from_pixbuf(scaled_buf)
                tooltip = gtk.Tooltips()
                tooltip.set_tip(img, w.get_name())
                img.show()
                idx += 1

                box.connect("button-press-event", self.clicked, w)
                _table.attach(active_box, 1, 2, idx, idx + 1, gtk.SHRINK, gtk.SHRINK)
                _table.attach(box, 2, 3, idx, idx + 1, gtk.SHRINK, gtk.SHRINK)
        _table.show_all()
        self.table = _table

    def clicked(self, widget, *data):
        window = data[1]
        event = data[0]
        if event.button == 1:
            print("Clicked on " + window.get_class_group().get_name())

            ts = gtk.gdk.x11_get_server_time(gtk.gdk.get_default_root_window())
            window.unshade()
            window.unminimize(ts)
            window.activate(ts)

            self.update_bar()

    @staticmethod
    def main():
        try:
            gtk.main()
        except KeyboardInterrupt:
            print("Bye")


if __name__ == "__main__":
    base = MyTaskbar()
    base.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
