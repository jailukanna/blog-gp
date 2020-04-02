---
title: PIL example certificado (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'certificado'


Modules used in program: 
* `import sys`

## python certificado

Python PIL example: certificado

```python
import sys
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw 

'''
==== Dependências ====

instalar PIL:
sudo apt-get install python-pil

se nao for ubuntu e/ou pacote nao estiver disponivel, instale o pip (gerenciador de pacotes python)
sudo apt-get install python-pip

e instale o PIL
sudo pip install PIL

==== Utilização ====

na linha de comando:
python certificado.py "Jose da Silva" certificado_jose_da_silva.jpg

dentro de script PHP:
<?php
$nome = "Jose da Silva";
$arquivo = "certificado_jose_da_silva.jpg";
exec("python certificado.py " . $nome . " " . $arquivo);

==== Troubleshooting ====
para resolver possiveis problemas de centralização:
http://stackoverflow.com/questions/1970807/center-middle-align-text-with-pil
http://stackoverflow.com/questions/2234874/draw-text-to-center-of-the-image-using-pil
http://stackoverflow.com/questions/19191286/how-to-center-text-with-pil
'''

# abre modelo do certificado
img = Image.open("sample_in.jpg")

# define canvas
draw = ImageDraw.Draw(img)

# define fonte
# o arquivo da fonte true type deve estar presente no diretorio do script
font = ImageFont.truetype("sans-serif.ttf", 16)

# escreve o texto na imagem
# (0,0) são as cordenadas x,y
draw.text((0, 0), sys.argv[1], (255,255,255), font=font)

# pega nome do arquivo do parametro passado na linha de comando
out_file = sys.argv[2]

#salva arquivo
img.save(out_file)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
