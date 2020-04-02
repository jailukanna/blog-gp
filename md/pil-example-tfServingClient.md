---
title: pil example tfServingClient (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'tfServingClient'

Functions in program: 
* `def main():`

Modules used in program: 
* `import numpy as np`
* `import requests`

## python tfServingClient

Python pil example: tfServingClient

```python
import requests
import numpy as np
from PIL import Image
from io import BytesIO

SERVER_URL = "http://tensorflow-model-serving-route-aiops-dev-prometheus-lts.cloud.paas.upshift.redhat.com/v1/models/saved_model:predict"

IMAGE_URL = "https://www.redhat.com/cms/managed-files/redhat-internships-300x200.jpg"

def main():
    # # Download the image
    # dl_request = requests.get(IMAGE_URL, stream=True)
    # pilImage = Image.open(BytesIO(dl_request.content))
    # dl_request = [(np.array(pilImage.resize((320, 640)))).tolist()]

    # Just use a 4 dimensional zeros array
    dl_request = [np.zeros((640, 320, 3)).tolist()]


    predict_request = '{"instances": %s}' % str(dl_request)

    response = requests.post(SERVER_URL, data=predict_request)
    print(response.json())

if __name__ == "__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
