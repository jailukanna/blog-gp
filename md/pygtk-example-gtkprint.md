---
title: pygtk example gtkprint (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'gtkprint'

Functions in program: 
* `def _UNSAFE_pycairo_context_to_cairocffi(pycairo_context):`
* `def print_html(document):`

Modules used in program: 
* `import gtk`
* `import pygtk`
* `import cairocffi`
* `import cairo`

## python gtkprint

Python pygtk example: gtkprint

```python
import cairo
import cairocffi
import pygtk
pygtk.require('2.0')
import gtk

from weasyprint(import HTML)


def print_html(document):
    """:param document: A :class:`~weasyprint.document.Document` object."""

    def draw_page(_, print_context, page_no):
        pycairo_context = print_context.get_cairo_context()
        cairocffi_context = _UNSAFE_pycairo_context_to_cairocffi(pycairo_context)
        # 72 dpi (default resolution in GTK Print) / 96 dpi (default in WeasyPrint)
        document.pages[page_no].paint(cairocffi_context, scale=72. / 96)

    operation = gtk.PrintOperation()
    operation.set_n_pages(len(document.pages))
    operation.connect('draw-page', draw_page)
    operation.run(gtk.PRINT_OPERATION_ACTION_PREVIEW)


# http://pythonhosted.org/cairocffi/cffi_api.html#converting-pycairo-wrappers-to-cairocffi
def _UNSAFE_pycairo_context_to_cairocffi(pycairo_context):
    if not isinstance(pycairo_context, cairo.Context):
        raise TypeError('Expected a cairo.Context, got %r' % pycairo_context)
    return cairocffi.Context._from_pointer(
        cairocffi.ffi.cast('cairo_t **',
                           id(pycairo_context) + object.__basicsize__)[0],
        incref=True)


if __name__ == '__main__':
    print_html(HTML('http://www.w3.org/TR/CSS21/intro.html').render(
        stylesheets=['http://weasyprint.org/samples/CSS21-print.css'],
        enable_hinting=False))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
