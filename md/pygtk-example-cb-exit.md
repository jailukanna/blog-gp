---
title: pygtk example cb-exit (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'cb-exit'

Functions in program: 
* `def main():`

Modules used in program: 
* `import getpass`
* `import subprocess`
* `import gtk`
* `import pygtk`

## python cb-exit

Python pygtk example: cb-exit

```python
#!/usr/bin/env python
import pygtk
pygtk.require('2.0')
import gtk
import subprocess
import getpass


class CBExit(object):
    def __init__(self):
        self.create_window()

    def create_window(self):
        self.window = gtk.Window()
        title = 'Log out %s? Choose an option' % getpass.getuser()
        self.window.set_title(title)
        self.window.set_border_width(5)
        self.window.set_size_request(500, 80)
        self.window.set_resizable(False)
        self.window.set_keep_above(True)
        self.window.stick
        self.window.set_position(1)
        self.window.connect('delete_event', gtk.main_quit)
        windowicon = self.window.render_icon(gtk.STOCK_QUIT, gtk.ICON_SIZE_MENU)
        self.window.set_icon(windowicon)

        # Create HBox for buttons
        self.button_box = gtk.HBox()
        self.button_box.show()

        # Cancel button
        self.cancel = gtk.Button(stock=gtk.STOCK_CANCEL)
        self.cancel.set_border_width(4)
        self.cancel.connect('clicked', self.cancel_action)
        self.button_box.pack_start(self.cancel)
        self.cancel.show()

        # Logout button
        self.logout = gtk.Button('_Log out')
        self.logout.set_border_width(4)
        self.logout.connect('clicked', self.logout_action)
        self.button_box.pack_start(self.logout)
        self.logout.show()

        # Suspend button
        self.suspend = gtk.Button('_Suspend')
        self.suspend.set_border_width(4)
        self.suspend.connect('clicked', self.suspend_action)
        self.button_box.pack_start(self.suspend)
        self.suspend.show()

        # Hibernate button
        self.hibernate = gtk.Button('_Hibernate')
        self.hibernate.set_border_width(4)
        self.hibernate.connect('clicked', self.hibernate_action)
        self.button_box.pack_start(self.hibernate)
        self.hibernate.show()

        # Reboot button
        self.reboot = gtk.Button('_Reboot')
        self.reboot.set_border_width(4)
        self.reboot.connect('clicked', self.reboot_action)
        self.button_box.pack_start(self.reboot)
        self.reboot.show()

        # Shutdown button
        self.shutdown = gtk.Button('_Power off')
        self.shutdown.set_border_width(4)
        self.shutdown.connect('clicked', self.shutdown_action)
        self.button_box.pack_start(self.shutdown)
        self.shutdown.show()

        # Create HBox for status label
        self.label_box = gtk.HBox()
        self.label_box.show()
        self.status = gtk.Label()
        self.status.show()
        self.label_box.pack_start(self.status)

        # Create VBox and pack the above HBox's
        self.vbox = gtk.VBox()
        self.vbox.pack_start(self.button_box)
        self.vbox.pack_start(self.label_box)
        self.vbox.show()

        # Close the windows on Escape
        accelgroup = gtk.AccelGroup()
        key, modifier = gtk.accelerator_parse('Escape')
        accelgroup.connect_group(key, modifier,
                                 gtk.ACCEL_VISIBLE, gtk.main_quit)
        self.window.add_accel_group(accelgroup)

        self.window.add(self.vbox)
        self.window.show()

    def cancel_action(self, btn):
        self.disable_buttons()
        gtk.main_quit()

    def logout_action(self, btn):
        self.disable_buttons()
        self.status.set_label('Exiting Openbox, please standby...')
        subprocess.call(['openbox', '--exit'])

    def suspend_action(self, btn):
        self.disable_buttons()
        self.status.set_label('Suspending, please standby...')
        subprocess.call(['cb-lock'])
        subprocess.call(['dbus-send', '--system', '--print-reply',
                         '--dest=org.freedesktop.UPower',
                         '/org/freedesktop/UPower',
                         'org.freedesktop.UPower.Suspend'])
        gtk.main_quit()

    def hibernate_action(self, btn):
        self.disable_buttons()
        self.status.set_label('Hibernating, please standby...')
        subprocess.call(['cb-lock'])
        subprocess.call(['dbus-send', '--system', '--print-reply',
                         '--dest=org.freedesktop.UPower',
                         '/org/freedesktop/UPower',
                         'org.freedesktop.UPower.Hibernate'])
        gtk.main_quit()

    def reboot_action(self, btn):
        self.disable_buttons()
        self.status.set_label('Rebooting, please standby...')
        subprocess.call(['dbus-send', '--system', '--print-reply',
                         '--dest=org.freedesktop.ConsoleKit',
                         '/org/freedesktop/ConsoleKit/Manager',
                         'org.freedesktop.ConsoleKit.Manager.Restart'])

    def shutdown_action(self, btn):
        self.disable_buttons()
        self.status.set_label('Shutting down, please standby...')
        subprocess.call(['dbus-send', '--system', '--print-reply',
                         '--dest=org.freedesktop.ConsoleKit',
                         '/org/freedesktop/ConsoleKit/Manager',
                         'org.freedesktop.ConsoleKit.Manager.Stop'])

    def disable_buttons(self):
        self.cancel.set_sensitive(False)
        self.logout.set_sensitive(False)
        self.suspend.set_sensitive(False)
        self.hibernate.set_sensitive(False)
        self.reboot.set_sensitive(False)
        self.shutdown.set_sensitive(False)


def main():
    gtk.main()

if __name__ == '__main__':
    go = CBExit()
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
