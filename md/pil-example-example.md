---
title: pil example example (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'example'


Modules used in program: 
* `import boto3`

## python example

Python pil example: example

```python
from typing import IO
from dataclasses import dataclass, field
from io import BytesIO

import boto3
from PIL import Image

s3 = boto3.resource("s3")


@dataclass
class S3File:
    bucket: str
    key: str
    io: IO = field(init=False)

    def __post_init__(self):
        self.io = BytesIO()

    @property
    def s3_object(self):
        return s3.Object(self.bucket, self.key)

    @property
    def write(self):
        return self.io.write

    def read(self):
        return self.s3_object.get()["Body"].read()

    def flush(self):
        """ After writing to a file, PIL will run 'flush' if available """
        self.io.seek(0)
        return self.s3_object.put(Body=self.io)


if __name__ == "__main__":
    bucket = "my-bucket"
    src_file = S3File(bucket, "planet/PSScene4Band-20180112_105605_102c/thumb.png")
    dst_file = S3File(bucket, "planet/PSScene4Band-20180112_105605_102c/thumb-cropped.png")
    
    # Do some PIL things...
    src = Image.open(src_file)
    src.crop(src.getbbox()).save(dst_file, format=src.format)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
