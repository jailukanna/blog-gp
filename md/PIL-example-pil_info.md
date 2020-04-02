---
title: PIL example pil info (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'pil info'


Modules used in program: 
* `import os`
* `import re`

## python pil info

Python PIL example: pil info

```python
#!/usr/bin/env python3
from sys import exit
from pathlib import Path
import re
import os

try:
    import PIL, PIL.Image
except:
    exit("PIL not found.")

ELFFile = None
if os.name == 'posix':
    try:
        from elftools.elf.elffile import ELFFile
    except:
        print("Install pyelftools package to get further information (pip or on conda-forge channel)")

print(f"PIL loaded from {PIL.__path__}")
is_pil_simd = '.post' in PIL.__version__
print(f"PIL version: {PIL.__version__}\n  {'Looks' if is_pil_simd else 'Doesn''t look'} like Pillow-SIMD")
if PIL.__version__.split('.') >= ['5','4','0']:
    from PIL.features import check_feature
    print(f"PIL libjpeg-turbo feature: {check_feature('libjpeg_turbo')}")
else:
    print("PIL version doesn't report libjpeg-turbo feature (requires 5.4.0+)")
ext_file = PIL.Image.core.__file__
print(f"PIL extension: {ext_file}")
print(f"PIL compiled jpeglib_version: {PIL.Image.core.jpeglib_version}")
if PIL._imaging.jpeglib_version.split('.') >= ['9','0']:
    print("PIL compiled against libjpeg API 9+. libjpeg-turbo only supports 8 and below.")
if ELFFile:
    print("Extension linking:")
    with open(ext_file, 'rb') as f:
        elf = ELFFile(f)
        dyn = elf.get_section_by_name('.dynamic')
        linked = {tag.needed.split('.')[0][3:]: tag.needed for tag in dyn.iter_tags() if tag.entry.d_tag == 'DT_NEEDED'}
        for lib in ['jpeg', 'tiff']:
            print(f"  {lib}: {linked.get(lib, 'Not linked')}")

if os.name != 'posix':
    print("Further analysis not implemented for non-posix systems")
    exit()

# JPEG library checks
jpeg_paths = set()
for l in open('/proc/self/maps', 'r'):
    if 'libjpeg' in l:
        # Last component should be path, just in case make sure it's a real path
        path = l.split()[-1] 
        if Path(path).exists():
            jpeg_paths.add(path)
if len(jpeg_paths) > 1:
    print("Multiple JPEG libraries loaded:")
else:
    print("Loaded JPEG Library:")
for path in jpeg_paths:
    print(f"  {path}")
    if ELFFile:
        with open(path, 'rb') as f:
            elf = ELFFile(f)
            syms = elf.get_section_by_name('.symtab')
            simd_syms = [sym.name for sym in syms.iter_symbols() if sym.name.startswith('jsimd_')]
            if simd_syms:
                print(f'    Identified libjpeg-turbo, found {len(simd_syms)} jsimd functions')
            else:
                print('    No jpeglib-turbo SIMD functions found.')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
