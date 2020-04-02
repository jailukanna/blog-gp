---
title: pil example doc reader (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'doc reader'

Functions in program: 
* `def reconhecimento(campos):`
* `def segmentacao(src_gray,ori):`
* `def preprocessamento(src_gray,ori):`

Modules used in program: 
* `import re`
* `import random`
* `import sys`
* `import glob`
* `import time`
* `import datetime`
* `import numpy as np`
* `import subprocess`
* `import os`
* `import tesseract`
* `import pytesseract`
* `import scipy.io`
* `import scipy.misc`
* `import cv2`
* `import PIL`

## python doc reader

Python pil example: doc reader

```python
import PIL
from PIL import Image, ImageFilter, ImageOps
import cv2
from scipy import ndimage
import scipy.misc
import scipy.io
import pytesseract
import tesseract
import os
import subprocess
import numpy as np
import datetime
import time
import glob
from subprocess import call
import sys
import random
import re

# from matplotlib

path = "/home/danielle/projects/python/pyParseDocts/data/"


def preprocessamento(src_gray,ori):
    '''Muda a imagem para grayscale'''
    ori = cv2.cvtColor(ori,cv2.COLOR_BGR2GRAY)
    src_gray = cv2.cvtColor(src_gray,cv2.COLOR_BGR2GRAY)

    ''' Imagem negativa '''
    # ori = cv2.bitwise_not(ori, ori)

    return (src_gray,ori)

    '''Tira o brilho da img
        saturated = Binarize[ColorConvert[thresh, "Grayscale"], .9]
        Inpaint[img, Dilation[saturated, DiskMatrix[20]]] '''
    # RESIZE MASK
    # =============================================================
    basewidth = 640
    aux = path + 'mask.jpg'
    img = PIL.Image.open(aux,'r')
    wpercent = (basewidth / float(img.size[0]))
    hsize = int((float(img.size[1]) * float(wpercent)))
    img = img.resize((basewidth,hsize),PIL.Image.ANTIALIAS)
    img.save(file_resized)

    mask = scipy.misc.imread(file_resized,0)

    mask = cv2.cvtColor(mask,cv2.COLOR_BGR2GRAY)
    # =============================================================

    dst = cv2.inpaint(thresh,mask,3,cv2.INPAINT_TELEA)

    aux = path + 'teste.jpg'
    cv2.imwrite(aux,dst)

    return (aux,ori)


def segmentacao(src_gray,ori):
    '''Processo de dividir uma imagem digital em multiplas regioes
                [vert_acima, vert_abaixo, horiz_dir, horiz_esq]'''

    # dicionario
    campos = {}

    nome = src_gray[107:133,91:583]
    campos['nome'] = nome
    path_nome = path + "nome.jpg"
    cv2.imwrite(path_nome,nome)

    rg = src_gray[151:175,302:580]
    campos['rg'] = rg
    path_rg = path + "rg.jpg"
    cv2.imwrite(path_rg,rg)

    cpf = src_gray[192:215,302:455]
    campos['cpf'] = cpf
    path_cpf = path + "cpf.jpg"
    cv2.imwrite(path_cpf,cpf)

    dt_nasc = src_gray[192:215,463:580]
    campos['dt_nasc'] = dt_nasc
    path_dt_nasc = path + "dt_nasc.jpg"
    cv2.imwrite(path_dt_nasc,dt_nasc)

    pai = src_gray[238:282,302:580]
    campos['nome_pai'] = pai
    path_pai = path + "nome_pai.jpg"
    cv2.imwrite(path_pai,pai)

    mae = src_gray[283:326,302:580]
    campos['nome_mae'] = mae
    path_mae = path + "nome_mae.jpg"
    cv2.imwrite(path_mae,mae)

    num_reg = src_gray[394:414,107:287]
    campos['num_reg'] = num_reg
    path_num_reg = path + "num_reg.jpg"
    cv2.imwrite(path_num_reg,num_reg)

    validade = src_gray[389:410,297:427]
    campos['validade'] = validade
    path_validade = path + "validade.jpg"
    cv2.imwrite(path_validade,validade)

    prim_hblt = src_gray[389:410,438:576]
    campos['primeira_habilitacao'] = prim_hblt
    path_prim_hblt = path + "primeira_habilitacao.jpg"
    cv2.imwrite(path_prim_hblt,prim_hblt)

    num_espelho = src_gray[245:437,44:79]
    campos['num_espelho'] = num_espelho
    path_num_espelho = path + "num_espelho.jpg"

    # rotate the image by 90 degrees
    rotated = ndimage.rotate(num_espelho,270)
    cv2.imwrite(path_num_espelho,rotated)

    # print(len(campos))
    # print(type(campos))
    # print(campos.keys())
    # print(campos['NOME'])
    # print(campos)
    # print('\n')
    return campos


def reconhecimento(campos):
    '''cv2.threshold requires a gray-scale image for an argument, not a string representing a filename.'''

    cpo_gray = (campos['nome'])

    '''Histograms Equalization'''
    equ = cv2.equalizeHist(cpo_gray)
    cv2.imwrite(path + 'img_equ.jpg', equ)


    '''CLAHE (Contrast Limited Adaptive Histogram Equalization)'''
    clahe = cv2.createCLAHE(clipLimit=2.0, tileGridSize=(8, 8))
    cli = clahe.apply(equ)
    cv2.imwrite(path + 'img_clahe.jpg', cli)


    '''It makes the reduction of a graylevel image to a binary image (Also uses THRESH_OTSU) THRESH_BINARY'''
    (retval, thresh) = cv2.threshold(cli, 54, 255, cv2.THRESH_BINARY)
    cv2.imwrite(path + 'nome_thresh.jpg', thresh)


    '''Opening is just another name of erosion followed by dilation. It is useful in removing noise'''
    kernel = np.ones((1,2),np.uint8)
    opening = cv2.morphologyEx(thresh, cv2.MORPH_OPEN, kernel)
    cv2.imwrite(path + 'nome_opening.jpg', opening)


    '''Closing is reverse of Opening, Dilation followed by Erosion.'''
    kernel = np.ones((2,2),np.uint8)
    closing = cv2.morphologyEx(thresh, cv2.MORPH_CLOSE, kernel)
    cv2.imwrite(path + 'nome_closing.jpg', closing)


    ''' Imagem negativa '''
    #thresh = cv2.bitwise_not(thresh, thresh)


    '''Erosion'''
    #kernel = np.ones((1,2),np.uint8)
    kernel = np.ones((1,2),np.uint8)
    erosion = cv2.erode(thresh, kernel, iterations=1)
    cv2.imwrite(path + 'nome_erosion.jpg', erosion)

    '''Dilation'''
    #kernel = np.ones((1,1),np.uint8)
    kernel = np.ones((3,1),np.uint8)
    dilation = cv2.dilate(erosion, kernel, iterations=1)
    cv2.imwrite(path + 'nome_dilation.jpg', dilation)

    '''Blur'''
    #aux = PIL.Image.open(path + 'nome_opening.jpg', 'r')
    #aux = aux.filter(ImageFilter.GaussianBlur(1))
    #cv2.imwrite(path + 'nome_blur.jpg', aux)


    code = pytesseract.image_to_string(Image.open(path + 'nome_opening.jpg'))

    ''''
    api = tesseract.TessBaseAPI()
    api.Init(".", "eng", tesseract.OEM_DEFAULT)
    api.SetVariable("tessedit_char_whitelist", "ABCDEFGHIJKLMNOPQRSTUVWXYZ")
    api.SetPageSegMode(tesseract.PSM_AUTO)
    '''

    code = code.replace('\n', ' ')
    code = code.replace(', ', '')
    code = code.replace('. ', '')
    code = code.replace(': ', ' ')


    '''
    filename = path + 'nome_aux.jpg'

    outfilename = path + 'nome_out'
    outfilename_txt = outfilename + '.txt'

    subprocess.call(['tesseract', filename, outfilename])

    fd = open(outfilename_txt, 'r')

    raw_code = fd.read()

    print('RAW_CODE [' + raw_code + ']')

    fd.close()

    # os.remove (filename)
    os.remove(outfilename_txt)

    code = raw_code.replace('\n','')

    '''
    return code


if __name__ == "__main__":
    file_orig = path + "cnh_frente.jpg"
    file_resized = path + "res_cnh_frente.jpg"
    file_bw_gray = path + "bw_cnh_frente_gray.jpg"
    file_bw_ori = path + "bw_cnh_frente_ori.jpg"

    # RESIZE IMG
    basewidth = 640
    img = PIL.Image.open(file_orig,'r')
    wpercent = (basewidth / float(img.size[0]))
    hsize = int((float(img.size[1]) * float(wpercent)))
    img = img.resize((basewidth,hsize),PIL.Image.ANTIALIAS)
    img.save(file_resized)

    src_gray = scipy.misc.imread(file_resized)
    ori = scipy.misc.imread(file_resized)

    # APLICA O GREYSCALE, TIRA O BRILHO E CONVERTE PARA BW
    (src_gray,ori) = preprocessamento(src_gray,ori)
    cv2.imwrite('/home/danielle/projects/python/pyParseDocts/data/' + file_bw_gray, src_gray)
    cv2.imwrite('/home/danielle/projects/python/pyParseDocts/data/' + file_bw_ori, ori)

    # QUEBRA CAMPOS
    ''' PSQ: Tipos de Segmentacao = Deteccao de Bordas.  '''
    campos = segmentacao(src_gray, ori)

    # OCR
    palavra = reconhecimento(campos)

    print("Palavra [" + palavra + "]")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
