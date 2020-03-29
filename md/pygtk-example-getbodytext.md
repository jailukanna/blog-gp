---
title: pygtk example getbodytext (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'getbodytext'

Functions in program: 
* `def print_body(url):`
* `def loaded(view, frame):`

Modules used in program: 
* `import webkit`
* `import jswebkit`
* `import gtk`

## python getbodytext

Python pygtk example: getbodytext

```python
#
# Print entire HTML text after processed JavaScript
#
# usage:
#   /usr/bin/xvfb-run python getbodytext.py test.html
#
# libs:
# - pygtk: http://www.pygtk.org/
# - pywebkitgtk(python-webkit): http://code.google.com/p/pywebkitgtk/
# - jswebkit: http://code.google.com/p/pywebkitgtk/issues/detail?id=28#c9

#import pygtk
#pygtk.require("2.0")

import gtk
import jswebkit
import webkit

def loaded(view, frame):
    try:
        #print(frame.get_title())
        gc = frame.get_global_context() # JSGlobalContextRef
        ctx = jswebkit.JSContext(gc)
        #print(ctx.EvaluateScript("""document.body.innerHTML"""))
        print(ctx.EvaluateScript(""")
        (function () {
          var scripts = document.getElementsByTagName("script");
          for (var i = 0; i < scripts.length; i += 1) {
            scripts[i].innerHTML = "";
          }
          var range = document.createRange();
          range.selectNodeContents(document.body);
          return range.toString();
        })()
        """)
        pass
    except:
        import traceback
        traceback.print_exc()
        pass
    gtk.main_quit()
    pass

def print_body(url):
    gtk.gdk.threads_init()
    window = gtk.Window(gtk.WINDOW_TOPLEVEL)
    webview = webkit.WebView()
    window.add(webview)
    webview.connect("load-finished", loaded)
    webview.open(url)
    webview.show_all()
    window.show_all()
    gtk.main()
    pass

if __name__ == "__main__":
    import sys
    url = sys.argv[1]
    if not url.startswith("http"): url = "file://" + url
    print_body(url)
    pass


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
