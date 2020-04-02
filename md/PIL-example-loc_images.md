---
title: PIL example loc images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'loc images'

Functions in program: 
* `def get_coordinates(geotags):`
* `def get_decimal_from_dms(dms, ref):`
* `def get_geotagging(exif):`
* `def get_exif(img):`
* `def get_image():`

## python loc images

Python PIL example: loc images

```python
# -*- coding: utf-8 -*-
#import sys, os, shutil, base64
from PIL import Image
from PIL.ExifTags import TAGS
from PIL.ExifTags import GPSTAGS


def get_image():
    '''
      Pegando imagem e associando a um objeto Image    
    '''
    img_path = 'https://raw.githubusercontent.com/ClaudioRicardo/inflexaodigital/master/posts/coleta-de-dados-imagens/images/20190518_092637.jpg'
    img = Image.open(img_path);
    return img


def get_exif(img):
    '''
      Pegando os metadados da imagem 
    '''
    img.verify()
    exif = img._getexif() 
    return exif

def get_geotagging(exif):
    '''
        Pega as geotags caso eles existam na imagem.
        As geotags são os dados GPS que podem existir em algumas imagens
    '''
    if not exif:
        raise ValueError("No EXIF metadata found")

    geotagging = {}
    for (idx, tag) in TAGS.items():
        if tag == 'GPSInfo':
            if idx not in exif:
                raise ValueError("No EXIF geotagging found")

            for (key, val) in GPSTAGS.items():
                if key in exif[idx]:
                    geotagging[val] = exif[idx][key]

    return geotagging


def get_decimal_from_dms(dms, ref):
    '''
        Converte os dados de latitude e lungitude que se encontram no formato
        Graus º, Minutos' e Segundos" para o valor decimal de latitude e longitude.
    '''

    degrees = dms[0][0] / dms[0][1]
    minutes = dms[1][0] / dms[1][1] / 60.0
    seconds = dms[2][0] / dms[2][1] / 3600.0

    if ref in ['S', 'W']:
        degrees = -degrees
        minutes = -minutes
        seconds = -seconds

    return round(degrees + minutes + seconds, 5)

def get_coordinates(geotags):
    '''
     Pega as coordenadas contidas nas geotags
    '''
    lat = get_decimal_from_dms(geotags['GPSLatitude'], geotags['GPSLatitudeRef'])
    lon = get_decimal_from_dms(geotags['GPSLongitude'], geotags['GPSLongitudeRef'])

    return (lat,lon)

if __name__=="__main__":
    '''
        Aqui é possivel printar e acompanhar todas as etapas para a coleta de metadados.
    '''
    img = get_image()
    exif = get_exif(img)
    geotags = get_geotagging(exif)
    coords = get_coordinates(geotags)
    
    print(coords)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
