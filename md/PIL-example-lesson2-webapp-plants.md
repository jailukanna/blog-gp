---
title: PIL example lesson2-webapp-plants (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'lesson2-webapp-plants'

Functions in program: 
* `def redirect_to_homepage(request):`
* `def form(request):`
* `def predict_image_from_bytes(bytes):`
* `def encode(img):`

Modules used in program: 
* `import base64`
* `import os`
* `import aiohttp`
* `import uvicorn`
* `import torch`

## python lesson2-webapp-plants

Python PIL example: lesson2-webapp-plants

```python
# This webapp is based on Healthy or Not (https://github.com/nikhilno1/healthy-or-not) -> Thanks! 

from starlette.applications import Starlette
from starlette.responses import JSONResponse, HTMLResponse, RedirectResponse
from fastai import *
from fastai.vision import *
import torch
from io import BytesIO
import uvicorn
import aiohttp
import os
import base64
from PIL import Image as PILImage

async def get_bytes(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.read()
def encode(img):
    img = (image2np(img.data) * 255).astype('uint8')
    pil_img = PILImage.fromarray(img)
    buff = BytesIO()
    pil_img.save(buff, format="JPEG")
    return base64.b64encode(buff.getvalue()).decode("utf-8")

#create data object from csv file with only one image per class (easy workaround to get all classes right)
data_path = 'data/ImageCLEF2011/'
images_clef2013_pd_sheet_one_class = pd.read_csv(data_path + 'images_clef2013_pd_one_class.csv')
data_placeholder = ImageDataBunch.from_df(data_path, images_clef2013_pd_sheet_one_class, fn_col=2, label_col=0, ds_tfms=get_transforms(), size=224)

#initalize pretrained fastai model
learner = create_cnn(data_placeholder, models.resnet34)
learner.load('leaf_types_stage_1')
#run inference on CPU, not GPU
defaults.device = torch.device('cpu')

#initialize app
app = Starlette()

@app.route("/upload", methods=["POST"])
async def upload(request):
    data = await request.form()
    bytes = await (data["file"].read())
    return predict_image_from_bytes(bytes)

@app.route("/classify-url", methods=["GET"])
async def classify_url(request):
    bytes = await get_bytes(request.query_params["url"])
    return predict_image_from_bytes(bytes)


def predict_image_from_bytes(bytes):
    img = open_image(BytesIO(bytes))
    pred_class,pred_idx,outputs = learner.predict(img)
    confidence = outputs[pred_idx].item()
    img_data = encode(img)

    return HTMLResponse(
        """
        <html>
           <body>
             <p>Prediction: <b>%s</b></p>
             <p>Confidence: %s</p>
           </body>
        <figure class="figure">
          <img src="data:image/png;base64, %s" class="figure-img img-thumbnail input-image">
        </figure>
        </html>
    """ %(pred_class.upper(), confidence, img_data))

@app.route("/")
def form(request):
    return HTMLResponse(
        """
        <h1>Which  plant leaf is this??</h1>
        <p>Find out to what plant this leaf belongs to (based on https://www.imageclef.org/2013/plant)</p><br>
        <p>Upload image or specify URL.</p><br>
        <form action="/upload" method="post" enctype="multipart/form-data">
            <u>Select image to upload:</u><br><p>
            1. <input type="file" name="file"><br><p>
            2. <input type="submit" value="Upload and analyze image">
        </form>
        <br>
        <strong>OR</strong><br><p>
        <u>Submit a URL:</u>
        <form action="/classify-url" method="get">
            1. <input type="url" name="url" size="60"><br><p>
            2. <input type="submit" value="Fetch and analyze image">
        </form>
    """)


@app.route("/form")
def redirect_to_homepage(request):
    return RedirectResponse("/")


port = int(os.environ.get("PORT", 8008))
uvicorn.run(app, host="0.0.0.0", port=port)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
