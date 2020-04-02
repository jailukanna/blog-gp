---
title: PIL example scaleai2 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'scaleai2'

Functions in program: 
* `def get_local_dataloaders(local_data_root: str, batch_size: int = 8, transform: Callable = None,`
* `def list_items_local(path: str) -> List[str]:`

Modules used in program: 
* `import torch.utils.data`
* `import torch`
* `import os`

## python scaleai2

Python PIL example: scaleai2

```python
# Full Example: https://gist.github.com/danielhavir/407a6cfd592dfc2ad1e23a1ed3539e07
import os
from typing import Callable, List, Tuple, Generator, Dict

import torch
import torch.utils.data
from PIL.Image import Image as ImageType


def list_items_local(path: str) -> List[str]:
    return sorted(os.path.splitext(f)[0] for f in os.listdir(path))


class ImageDataset(torch.utils.data.Dataset):
    def __init__(self, data_root: str, items: List[str], loader: Callable[[str], ImageType] = pil_loader, transform=None):
        self.data_root = data_root
        self.loader = loader
        self.items = items
        self.transform = transform

    def __len__(self):
        return len(self.items)

    def __getitem__(self, item):
        item_id = self.items[item]

        image = self.loader(os.path.join(self.data_root, "images", item_id + ".jpg"))
        label = self.loader(os.path.join(self.data_root, "labels", item_id + ".png"))

        if self.transform is not None:
            image, label = self.transform((image, label))

        return image, label


def get_local_dataloaders(local_data_root: str, batch_size: int = 8, transform: Callable = None,
                          test_ratio: float = 0.1, num_workers: int = 8) -> Dict[str, torch.utils.data.DataLoader]:
    # Local training
    local_items = list_items_local(os.path.join(local_data_root, "images"))
    dataset = ImageDataset(local_data_root, local_items, transform=transform)

    # Split using consistent hashing
    train_indices, test_indices = consistent_train_test_split(local_items, test_ratio)
    return {
        "train": torch.utils.data.DataLoader(dataset, batch_size=batch_size, sampler=torch.utils.data.SubsetRandomSampler(train_indices),
                                             num_workers=num_workers),
        "test": torch.utils.data.DataLoader(dataset, batch_size=batch_size, sampler=torch.utils.data.SubsetRandomSampler(test_indices),
                                            num_workers=num_workers)
    }


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
