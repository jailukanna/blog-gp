---
title: PIL example create labels (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'create labels'


Modules used in program: 
* `import pandas as pd`

## python create labels

Python PIL example: create labels

```python
import pandas as pd
from PIL import Image
from pathlib import Path
from button import ButtonDetectionAPI, rgb2bgr

if __name__ == '__main__':
    root = Path("eval_images")

    api = ButtonDetectionAPI(model_path="model/frozen_inference_graph.pb",
                             label_map_path="model/labelmap.pbtxt",
                             num_classes=1)
    api.load_model()

    data = {"filename": [],
            "width": [],
            "height": [],
            "class": [],
            "xmin": [],
            "ymin": [],
            "xmax": [],
            "ymax": [],
            "box_height": [],
            "box_width": []}
    for image_path in root.glob("*.png"):
        pil_img = Image.open(image_path)
        width, height = pil_img.size
        boxes, _ = api.detect(image=rgb2bgr(pil_img), thresh_hold=0.8)
        for box in boxes:
            _y_min, _x_min, _y_max, _x_max = box
            data["filename"].append(str(image_path.name))
            data["width"].append(width)
            data["height"].append(height)
            data["class"].append(1)
            x_min = int(_x_min * width)
            y_min = int(_y_min * height)
            x_max = int(_x_max * width)
            y_max = int(_y_max * height)
            data["xmin"].append(x_min)
            data["ymin"].append(y_min)
            data["xmax"].append(x_max)
            data["ymax"].append(y_max)
            data["box_height"].append(y_max - y_min)
            data["box_width"].append(x_max - x_min)

    pd.DataFrame.from_dict(data).to_csv("labels.csv", index=False)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
