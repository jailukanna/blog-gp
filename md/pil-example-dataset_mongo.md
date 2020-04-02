---
title: pil example dataset mongo (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'dataset mongo'

Functions in program: 
* `def pil_loader(f):`

Modules used in program: 
* `import numpy as np`
* `import os`
* `import io`

## python dataset mongo

Python pil example: dataset mongo

```python
import io
import os
import numpy as np
from PIL import Image
from pymongo import MongoClient
from torch.utils.data import Dataset, DataLoader
from torchvision import transforms


def pil_loader(f):
    with Image.open(io.BytesIO(f)) as img:
        return img.convert('RGB')


class DatasetDB(Dataset):
    def __init__(self, db_name='images', col_name='train', transform=None):
        self._label_dtype = np.int32
        self.transform = transform

        client = MongoClient('localhost', 27017)
        db = client[db_name]
        self.col = db[col_name]
        self.examples = list(self.col.find({}, {'imgs': 0}))
        self.labels = self.get_labels()
        print(self.labels)
        # self.labels = dict([(line.strip(), idx) for idx, line in enumerate(open(labels_txt, "r"))])

    def __len__(self):
        return len(self.examples)

    def get_labels(self):
        category_ids = [e['category_id'] for e in self.examples]
        return {cid: i for i, cid in enumerate(sorted(list(set(category_ids))))}

    def __getitem__(self, i):
        _id = self.examples[i]['_id']
        doc = self.col.find_one({'_id': _id})

        img = doc['imgs'][0]['picture']
        img = pil_loader(img)

        if self.transform:
            img = self.transform(img)

        label = self.labels[doc['category_id']]
        assert type(label) == int
        return img, label, _id

#normalize = transforms.Normalize(mean=[0.485, 0.456, 0.406],
#                                 std=[0.229, 0.224, 0.225])
#
#transform = transforms.Compose([
#            transforms.RandomSizedCrop(224),
#            transforms.RandomHorizontalFlip(),
#            transforms.ToTensor(),
#        ])
#
#dataset = DatasetDB(transform=transform)
#loader = DataLoader(dataset, batch_size=256, shuffle=True, num_workers=8)
#for image, label, prod in loader:
#    print(image.max())
#    print(image.min())
#    print(" --- ")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
