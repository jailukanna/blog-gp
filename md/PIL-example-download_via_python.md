---
title: PIL example download via python (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'download via python'

Functions in program: 
* `def requests_pil(url: str):`
* `def requests_chunk(url: str):`
* `def requests_2(url: str):`
* `def requests_1(url: str):`
* `def urllib_urlretrieve(url: str):`
* `def get_file_name(url: str) -> str:`

Modules used in program: 
* `import requests`
* `import pathlib`
* `import urllib`
* `import shutil`

## python download via python

Python PIL example: download via python

```python
import shutil
import urllib
import pathlib
import requests
from PIL import Image
from io import BytesIO


def get_file_name(url: str) -> str:
    return url.split('/')[-1].split('?')[0]


def urllib_urlretrieve(url: str):
    file_name = 'urllib_urlretrieve_' + get_file_name(url)
    file_path = pathlib.Path(file_name)

    urllib.request.urlretrieve(url, file_path)


def requests_1(url: str):
    file_name = 'requests_1_' + get_file_name(url)
    file_path = pathlib.Path(file_name)

    # `Response.raw` is a raw stream of bytes – it does not transform the response content.
    # If you really need access to the bytes as they were returned, use `Response.raw`.
    # Must set `stream=True` in your initial request.
    r = requests.get(url, stream=True)
    with file_path.open('wb') as o:
        shutil.copyfileobj(r.raw, o)


def requests_2(url: str):
    file_name = 'requests_2_' + get_file_name(url)
    file_path = pathlib.Path(file_name)

    # use `r.content` access the response body as bytes, for non-text requests
    # The gzip and deflate transfer-encodings are automatically decoded for you.
    r = requests.get(url)
    with file_path.open('wb') as o:
        o.write(r.content)


def requests_chunk(url: str):
    file_name = 'requests_chunk_' + get_file_name(url)
    file_path = pathlib.Path(file_name)

    # get raw socket response from the server by using `r.raw`
    # must set `stream=True` in your initial request.
    r = requests.get(url, stream=True)
    with file_path.open('wb') as o:
        # `Response.iter_content` will automatically decode the gzip and deflate transfer-encodings.
        for chunk in r.iter_content(chunk_size=32):
            o.write(chunk)


def requests_pil(url: str):
    file_name = 'requests_pil_' + get_file_name(url)
    file_path = pathlib.Path(file_name)

    r = requests.get(url)
    image = Image.open(BytesIO(r.content))
    image.save(file_path)


if __name__ == '__main__':
    url = 'http://docs.python-requests.org/en/master/_static/requests-sidebar.png'

    urllib_urlretrieve(url)
    requests_1(url)
    requests_2(url)
    requests_chunk(url)
    requests_pil(url)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
