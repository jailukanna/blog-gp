---
title: PIL example image frying (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image frying'


Modules used in program: 
* `import imageio`
* `import tqdm`
* `import pathlib`

## python image frying

Python PIL example: image frying

```python
import pathlib
import tqdm
import imageio

# import PIL.Image
# PIL.Image.MAX_IMAGE_PIXELS = 933120000
# input_path = pathlib.Path('Starry Night.jpg')

input_path = pathlib.Path('Starry Night (small).jpg')
im = imageio.imread(input_path)

image_folder = input_path.with_suffix('')
image_folder.mkdir(parents=True, exist_ok=True)

for i in tqdm.trange(100):
    image_path = image_folder / f'image{i}.jpg'
    imageio.imwrite(image_path, im, format='jpg', quality=100-i)
    im = imageio.imread(image_path)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
