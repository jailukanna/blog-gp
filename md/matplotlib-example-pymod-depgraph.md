---
title: matplotlib example pymod-depgraph (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'pymod-depgraph'

Functions in program: 
* `def generate_digraph(modlist, colors, modgraph, **kwargs):`
* `def generate_colors(submods):`
* `def generate_depgraph(topdir):`
* `def depgraph(topdir):`
* `def imported_modules(modpath, pyfile):`
* `def module_list(topdir):`
* `def relative_imports(pyfile):`

Modules used in program: 
* `import sys`
* `import re`
* `import os`
* `import fnmatch`

## python pymod-depgraph

Python matplotlib example: pymod-depgraph

```python
#!/usr/bin/env python3

import fnmatch
import os
import re
import sys

from os import path
from pprint(import pprint)

def relative_imports(pyfile):
    import_pattern = re.compile(r'^\s*from\s+\.([\w\.]*)\s+import\s*' +
                                r'(?:\(([\w, \t\n\r\f\v]+)\)|([\w, \t\r\f\v]+))', re.M)
    as_pattern = re.compile(r'(\w+)\s+as\s+\w+')
    ret = {}
    with open(pyfile,'rt') as fin:
        for m in import_pattern.finditer(fin.read()):
            src, tgt = m.group(1), m.group(2) or m.group(3)
            tgts = []
            for t in tgt.split(','):
                t = t.strip()
                m = as_pattern.match(t)
                tgts.append(t if m is None else m.group(1))
            if src not in ret:
                ret[src] = []
            ret[src] += tgts
    return ret

def module_list(topdir):
    ret = []
    for root,dirs,files in os.walk(topdir):
        modpath = path.basename(topdir)
        r = path.relpath(root,topdir)
        if r != '.':
            modpath += '.' + r
        for f in fnmatch.filter(files, '*.py'):
            if f == '__init__.py':
                ret.append(modpath)
            elif f not in ['__main__.py']:
                ret.append('.'.join([modpath,path.splitext(f)[0]]))
    return ret

def imported_modules(modpath, pyfile):
    ret = []
    for k,v in relative_imports(pyfile).items():
        if k == '':
            ret += ['.'.join([modpath,i]) for i in v]
        else:
            s = re.search(r'[^.]',k)
            if s is None:
                relmod = '.'.join(modpath.split('.')[:-len(k)])
                ret += ['.'.join([relmod,i]) for i in v]
            else:
                n = s.start()
                if n == 0:
                    relmod = modpath
                else:
                    relmod = '.'.join(modpath.split('.')[:-n])
                ret.append(relmod+'.'+k[n:])
    return ret

def depgraph(topdir):
    ret = {}
    for root,dirs,files in os.walk(topdir):
        modpath = path.basename(topdir)
        r = path.relpath(root,topdir)
        if r != '.':
            modpath += '.' + r
        for f in fnmatch.filter(files, '*.py'):
            if f == '__init__.py':
                ret[modpath] = imported_modules(modpath, path.join(root,'__init__.py'))
            elif f not in ['__main__.py']:
                fmodpath = modpath+'.'+f.replace('.py','')
                ret[fmodpath] = imported_modules(modpath, path.join(root,f))
    return ret

def generate_depgraph(topdir):
    modlist = module_list(topdir)
    modgraph = depgraph(topdir)
    for k,v in modgraph.items():
        assert k in modlist, pprint(modlist)
        for i in v:
            assert i in modlist, pprint(modlist)

    submods = []
    for m in modlist:
        s = m.split('.')
        if len(s) > 1:
            submods.append(s[1])
    submods = sorted(set(submods))

    return modlist, submods, modgraph

def generate_colors(submods):
    import matplotlib
    matplotlib.use('GTK3Agg')

    import seaborn
    from matplotlib.colors import hex2color, rgb2hex
    ncolors = 1 + len(submods)
    palette = seaborn.husl_palette(ncolors, s=.5)
    colors = {m:rgb2hex(palette[i+1]) for i,m in enumerate(submods)}
    colors[modlist[0].split('.')[0]] = rgb2hex(palette[0])
    return colors

def generate_digraph(modlist, colors, modgraph, **kwargs):
    from graphviz import Digraph
    dot = Digraph(**kwargs)
    for m in modlist:
        s = m.split('.')
        submod = m if len(s)==1 else s[1]
        label = m if len(s)==1 else '.'.join(s[1:])
        dot.node(m, label=label, color=colors[submod], style='filled')
    for k,v in modgraph.items():
        for i in v:
            dot.edge(i,k)
    return dot

if __name__ == '__main__':
    modpath = sys.argv[1]
    modname = path.basename(modpath)
    modlist,submods,modgraph = generate_depgraph(modpath)
    try:
        colors = generate_colors(submods)
    except ImportError:
        class OneColor(object):
            def __getitem__(self,_):
                return 'white'
        colors = OneColor()
        print('please install matplotlib and seaborn for color.')

    try:
        dot = generate_digraph(modlist, colors, modgraph,
            engine='dot',
            format='png',
            strict=False,
            )
        dot.render(modname+'_digraph', view=True)
    except ImportError:
        print('please install graphviz for image output')
        pprint(modgraph)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
