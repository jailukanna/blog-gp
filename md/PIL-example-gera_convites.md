---
title: PIL example gera convites (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'gera convites'


## python gera convites

Python PIL example: gera convites

```python
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw

textos = {
    'fem': "Você está convidada para ...",
    'mas': "Você está convidado para ...",
    'fam': "Vocês estão convidados para ..."
}

nomes = [
    ("Vovó Martha", 'fem'),
    ("Tio Guilherme", "mas"),
    ("Tia Carol", "fem"),
    ("Vincenzo, Vania e Punk", "fam"),
]

for nome, texto in nomes:
    img = Image.open("convite.jpg")  # imagem base
    font = ImageFont.truetype(r'font.ttf', 35)  # arquivo ttf na mesma pasta do script.
    draw = ImageDraw.Draw(img)
    draw.text(
        (450, 108),  # x, y da imagem
        f"Olá, {nome}",
        (0, 0, 0),
        font=font
    )
    draw.text(
        (450, 160),  # x, y da imagem
        textos[texto],
        (0, 0, 0),
        font=font
    )
    img.save(f'./convites/{nome}.jpg')  # pasta `convites/` na pasta do script.

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
