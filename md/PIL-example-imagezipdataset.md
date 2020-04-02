---
title: PIL example imagezipdataset (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'imagezipdataset'

Functions in program: 
* `def test(cache=True, shuffle=True, num_workers=2):`

Modules used in program: 
* `import os`
* `import numpy as np`
* `import torch.multiprocessing as mp`
* `import mmap`
* `import zipfile`
* `import tarfile`
* `import torch`

## python imagezipdataset

Python PIL example: imagezipdataset

```python
import torch
from torch.utils.data import Dataset, DataLoader
import tarfile
import zipfile
from pathlib import Path
from PIL import Image
from tqdm import tqdm
from torchvision import transforms
import mmap
import torch.multiprocessing as mp
import numpy as np
import os

# mp.set_start_method('forkserver', force=True) # On linux, you may want to use the fork method (this the default with pytorch 0.4)

class ImageZipDataset(Dataset):
    def __init__(self, path, extension=".jpg", cache=False):

        zip = zipfile.ZipFile(open(path, mode="rb"))

        self._files = [m for m in zip.namelist() if m.endswith(extension)]
        self._path = path
        self._cache = cache
        self._archive = None
        self._pil_to_tensor = transforms.ToTensor()
        self._memmap_handle = None
        if mp.get_start_method() == 'fork' and self._cache is True:
            self._memmap_handle = self.get_memmap_handle()
            
    def get_memmap_handle(self):
        if self._memmap_handle is not None:
            return self._memmap_handle
        else:
            handle = open(self._path, "rb")
            if os.name == 'nt':
                memmap_handle = mmap.mmap(handle.fileno(), 0, self._path, access=mmap.ACCESS_READ)
            else:
                memmap_handle = mmap.mmap(handle.fileno(), 0, access=mmap.ACCESS_READ)
            return memmap_handle
    
    def get_zip_handle(self):
        if self._archive is None:
            if self._cache is True:
                file_handle = self.get_memmap_handle()
            else:
                file_handle = open(self._path, "rb")
            self._archive = zipfile.ZipFile(file_handle, mode="r")
        return self._archive

    def __getitem__(self, index):
        #return torch.Tensor(list(self.get_zip_handle().read(self._files[index]))) # Test without jpeg decoding
        img = Image.open(self.get_zip_handle().open(self._files[index]), mode="r") # use of jpeg-turbo is recommended
        return self._pil_to_tensor(img)

    def __len__(self):
        return len(self._files)

def test(cache=True, shuffle=True, num_workers=2):

    test = ZipDataset("myfiles.zip", cache=cache)
    if mp.get_start_method() != 'fork' and cache == True :
        handle = test.get_memmap_handle() # force having at least one handle of the memmaped file in spawn mode (not sure if done right?)
    dl = DataLoader(test, batch_size=1, shuffle=shuffle, num_workers=num_workers)

    res = 1
    for i in range(3):
        for x in tqdm(dl):
            res += x.sum()


if __name__ == "__main__":
    test()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
