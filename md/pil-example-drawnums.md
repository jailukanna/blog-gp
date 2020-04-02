---
title: pil example drawnums (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'drawnums'


## python drawnums

Python pil example: drawnums

```python
'''
Adds numbers to a sprite sheet for easy reference.
'''
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw

if __name__ == '__main__':
    TILE_SIZE = 16
    
    img = Image.open('spritesheet.png')
    draw = ImageDraw.Draw(img)
    font = ImageFont.truetype('font.ttf', 8)
    
    counter = 1
    
    for y in range(img.height / TILE_SIZE):
        for x in range(img.width / TILE_SIZE):
            draw.text((x * TILE_SIZE + 1, y * TILE_SIZE), str(counter), (0, 0, 0), font=font)
            draw.text((x * TILE_SIZE, y * TILE_SIZE + 1), str(counter), (0, 0, 0), font=font)
            draw.text((x * TILE_SIZE, y * TILE_SIZE), str(counter), (255, 0, 255), font=font)
            counter += 1
    
    img.save('spritesheet_numbered.png')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
