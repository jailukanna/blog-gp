---
title: matplotlib example exporters init diff (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'exporters init diff'


## python exporters init diff

Python matplotlib example: exporters init diff

```python
15c15,34
< for mod in importModules('', globals(), locals(), excludes=['Exporter']).values():
---
> #for mod in importModules('', globals(), locals(), excludes=['Exporter']).values():
> #    if hasattr(mod, '__all__'):
> #        names = mod.__all__
> #    else:
> #        names = [n for n in dir(mod) if n[0] != '_']
> #    for k in names:
> #        if hasattr(mod, k):
> #            Exporters.append(getattr(mod, k))
> 
> from . import CSVExporter
> from . import SVGExporter
> from . import Matplotlib
> from . import PrintExporter
> from . import ImageExporter
> 
> mods = [CSVExporter, SVGExporter, Matplotlib, PrintExporter, ImageExporter]
> for mod in mods:


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
