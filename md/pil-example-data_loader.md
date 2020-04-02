---
title: pil example data loader (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'data loader'

Functions in program: 
* `def get_loader(root, json, vocab, transform, batch_size, shuffle,num_workers):`
* `def collate_fn(data):`

Modules used in program: 
* `import jieba`
* `import numpy as np`
* `import pickle`
* `import os`
* `import torch.utils.data as data`
* `import torchvision.transforms as transforms`
* `import torch`

## python data loader

Python pil example: data loader

```python
import torch
import torchvision.transforms as transforms
import torch.utils.data as data
import os
import pickle
import numpy as np
from PIL import Image
from build_vocab import Vocabulary
import jieba

class myDataset(data.Dataset):
    """Custom Dataset"""
    def __init__(self, root, json, vocab, transform=None):
        '''
        :param root: image dir
        :param json: annotation file path
        :param vocab: vocab wrapper
        :param transform: image transform
        '''
        self.root = root
        self.json = json
        self.vocab = vocab
        self.transform = transform

    def __getitem__(self, index):
        """return one data pair
        (image, caption_jieba_list)
        iamge: PIL image metadata
        captions_jieba_list: [[1,2,3,,,], [4,5,6,,,],,,]
        """
        vocab = self.vocab
        img_name = self.json[index]['image_id']
        img_path = os.path.join(self.root, img_name)
        image = Image.open(img_path).convert("RGB")

        captions_list = self.json[index]['caption']

        if self.transform is not None:
            image = self.transform(image)

        captions_jieba = [[] for i in range(len(captions_list))]

        for i, cap in enumerate(captions_list):
            captions_jieba[i].append(vocab('<start>'))
            captions_jieba[i].extend([vocab(c) for c in jieba.cut(cap)])
            captions_jieba[i].append(vocab('<end>'))
        return image, captions_jieba

    def __len__(self):
        return len(self.json)

def collate_fn(data):
    """
    Create mini-batch tensors from a list of tuple (image, caption)
    返回的是图片乘以5倍之后的结果

    Args:
        data: 从getitem返回的data类型
            - image: torch tensor of shape
            - caption: [[1,2,3,,,], [4,5,6,,,],,,]
    Returns:
        images: torch tensor of shape (batch_size, 3, 256, 256)
        targets: torch tensors of shape (batch_size, padded_length)
        lengths: List; valid length for each padded caption
    """
    images, captions = zip(*data)
    images_five_times = []
    for image in images:
        for i in range(5):
            images_five_times.append(image.clone())

    # Merge into 4D
    images_five_times = torch.stack(images_five_times, 0)

    # Merge captions into 2D
    lengths = [len(cap) for i in captions for cap in i]

    targets = torch.zeros(len(lengths), max(lengths)).long()
    for cap_l in enumerate(captions):
        for i, cap in enumerate(cap_l):
            end = lengths[i]
            targets[i, :end] = cap[:end]
    return images, targets, lengths



def get_loader(root, json, vocab, transform, batch_size, shuffle,num_workers):
    """Returns torch.utils.data.DataLoader for custom dataset"""
    data = myDataset(root=root,
                     json=json,
                     vocab=vocab,
                     transform=transform)
    # 返回 (images, caption, lengths) 对于每一次iteration
    data_loader = torch.utils.data.DataLoader(dataset=data,
                                              batch_size=batch_size,
                                              shuffle=shuffle,
                                              num_workers=num_workers,
                                              collate_fn=collate_fn)
    return data_loader


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
