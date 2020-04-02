---
title: PIL example sort rotate (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'sort rotate'

Functions in program: 
* `def optRenameSort(unsFil):`
* `def irfanviewAutoOpti(filPat):`
* `def get_sha256(filename, block_size=65536):`
* `def get_exif_dict(edata):`
* `def get_exif_date(edata):`

Modules used in program: 
* `import subprocess`
* `import hashlib`
* `import datetime`
* `import os`

## python sort rotate

Python PIL example: sort rotate

```python
import os
import datetime
import hashlib
import subprocess

from PIL import Image
from PIL.ExifTags import TAGS


def get_exif_date(edata):
    """Converts the DateTimeOriginal from PIL exif data
    into a datetime object"""
    # https://www.awaresystems.be/imaging/tiff/tifftags/search.html?q=Date&Submit=Find+Tags
    dateObj = None
    if edata is not None:
        if 36867 in edata.keys():
            # DateTimeOriginal
            dateObj = datetime.datetime.strptime(edata[36867], "%Y:%m:%d %H:%M:%S")
        elif 306 in edata.keys():
            # DateTime
            print("DateTimeOriginal was empty, using DateTime instead")
            dateObj = datetime.datetime.strptime(edata[306], "%Y:%m:%d %H:%M:%S")
    return dateObj


def get_exif_dict(edata):
    """Converts PIL exif data into a dict with readable keys"""
    exif_dict = {}
    if edata:
        for tag, value in edata.items():
            decoded = TAGS.get(tag, tag)
            exif_dict[decoded] = value
    return exif_dict


def get_sha256(filename, block_size=65536):
    """Get SHA 256 of a file"""
    sha256 = hashlib.sha256()
    with open(filename, 'rb') as fil:
        for block in iter(lambda: fil.read(block_size), b''):
            sha256.update(block)
    return sha256.hexdigest()


def irfanviewAutoOpti(filPat):
    """Call IrfanView, optimize and auto-rotate"""
    exePat = os.path.join("C", os.sep, "Program Files", "IrfanView", "i_view64.exe")
    cmdArgs = "/jpg_rotate=(6,1,1,0,1,300,0,0) /cmdexit"
    cmd = "\"{}\" \"{}\" {}".format(exePat, filPat, cmdArgs)
    proc = subprocess.run(cmd, shell=True)
    return proc.returncode


def optRenameSort(unsFil):
    """Main script: Optimize and rename file, sort to new dir"""
    if not os.path.isfile(unsFil):
        print("Could not find this file:\n\t", unsFil)
    else:
        with Image.open(unsFil) as img:
            exif = img._getexif()
            filDat = get_exif_date(exif)

        if filDat is None:
            print("No EXIF date found, skipping:\n\t", unsFil)
        else:
            if filDat.date() < datetime.date(2010, 1, 1):
                print("Pic was taken before 2010, skipping:\n\t", unsFil)
            else:
                # Call IrfanView to auto-rotate and optimize
                ret = irfanviewAutoOpti(unsFil)
                if ret != 0:
                    print("Irfanview was not able to process this file:", ret, "\n\t", unsFil)
                else:
                    # Hash AFTER rotating
                    filHash = get_sha256(unsFil)
                    sorFilNam = filDat.strftime("%Y%m%d_%H%M%S_") + filHash[0:4] + ".jpg"
                    sorFilDir = os.path.join(sorDir, filDat.strftime("%Y"), filDat.strftime("%m"))
                    sorFilPat = os.path.join(sorFilDir, sorFilNam)
                    if os.path.isfile(sorFilPat):
                        # target exists, compare full hash and delete one of the two
                        if filHash == get_sha256(sorFilPat):
                            print("Target has identical SHA256, deleting:\n\t", unsFil)
                            os.remove(unsFil)
                        else:
                            print("Target has identical name, skipping:\n\t", unsFil, "\n\t", sorFilPat)
                    else:
                        os.renames(unsFil, sorFilPat)


if __name__ == '__main__':
    print(os.getcwd())
    cwd = os.getcwd()
    unsDir = os.path.join(cwd, "unsorted")
    sorDir = os.path.join(cwd, "sorted")

    for root, dirs, files in os.walk(unsDir):
        for file in files:
            if file.endswith(".jpg"):
                # print(os.path.join(root, file))
                optRenameSort(os.path.join(root, file))
            elif file.endswith(('Thumbs.db', 'Thumbnails.db', 'Desktop.ini', 'picasa.ini')):
                print("Deleting file:\n\t", os.path.join(root, file))
                os.remove(os.path.join(root, file))

        for dir in dirs:
            try:
                dirPat = os.path.join(root, dir)
                os.rmdir(dirPat)
                print("Deleting empty directory:\n\t", dirPat)
            except OSError as ex:
                pass


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
