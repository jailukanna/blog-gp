---
title: pil example equalized dataset (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'equalized dataset'

Functions in program: 
* `def default_loader(path):`
* `def accimage_loader(path):`
* `def pil_loader(path):`
* `def separate_datasets(path_label_tuples):`
* `def make_dataset(dir, class_to_idx):`
* `def find_classes(dir):`
* `def is_image_file(filename):`

Modules used in program: 
* `import os.path`
* `import os`
* `import torch.utils.data as data`

## python equalized dataset

Python pil example: equalized dataset

```python
import torch.utils.data as data

from PIL import Image
import os
import os.path

IMG_EXTENSIONS = [
    '.jpg', '.JPG', '.jpeg', '.JPEG',
    '.png', '.PNG', '.ppm', '.PPM', '.bmp', '.BMP',
]


def is_image_file(filename):
    return any(filename.endswith(extension) for extension in IMG_EXTENSIONS)


def find_classes(dir):
    classes = [d for d in os.listdir(dir) if os.path.isdir(os.path.join(dir, d))]
    classes.sort()
    class_to_idx = {classes[i]: i for i in range(len(classes))}
    return classes, class_to_idx


def make_dataset(dir, class_to_idx):
    images = []
    dir = os.path.expanduser(dir)
    for target in sorted(os.listdir(dir)):
        d = os.path.join(dir, target)
        if not os.path.isdir(d):
            continue

        for root, _, fnames in sorted(os.walk(d)):
            for fname in sorted(fnames):
                if is_image_file(fname):
                    path = os.path.join(root, fname)
                    item = (path, class_to_idx[target])
                    images.append(item)

    return images

def separate_datasets(path_label_tuples):
    # ('<path/to/data>', class_idx) 
    # Alphabetical order indexing w.r.t. subdirs
    # i.e. num of subdirs = num of class
    num_classes = path_label_tuples[-1][1] + 1
    class_lists = [[] for _ in range(num_classes)]
    for path, label in path_label_tuples:
        class_lists[label].append((path, label))
    return class_lists


def pil_loader(path):
    # open path as file to avoid ResourceWarning (https://github.com/python-pillow/Pillow/issues/835)
    with open(path, 'rb') as f:
        with Image.open(f) as img:
            return img.convert('RGB')


def accimage_loader(path):
    import accimage
    try:
        return accimage.Image(path)
    except IOError:
        # Potentially a decoding problem, fall back to PIL.Image
        return pil_loader(path)


def default_loader(path):
    from torchvision import get_image_backend
    if get_image_backend() == 'accimage':
        return accimage_loader(path)
    else:
        return pil_loader(path)


class ImageFolder(data.Dataset):
    """A generic data loader where the images are arranged in this way: ::
        root/dog/xxx.png
        root/dog/xxy.png
        root/dog/xxz.png
        root/cat/123.png
        root/cat/nsdf3.png
        root/cat/asd932_.png
    Args:
        root (string): Root directory path.
        transform (callable, optional): A function/transform that  takes in an PIL image
            and returns a transformed version. E.g, ``transforms.RandomCrop``
        target_transform (callable, optional): A function/transform that takes in the
            target and transforms it.
        loader (callable, optional): A function to load an image given its path.
     Attributes:
        classes (list): List of the class names.
        class_to_idx (dict): Dict with items (class_name, class_index).
        imgs (list): List of (image path, class_index) tuples
    """

    def __init__(self, root, transform=None, target_transform=None,
                 loader=default_loader, equal_sampling=True):
        classes, class_to_idx = find_classes(root)
        imgs = make_dataset(root, class_to_idx)
        if len(imgs) == 0:
            raise(RuntimeError("Found 0 images in subfolders of: " + root + "\n"
                               "Supported image extensions are: " + ",".join(IMG_EXTENSIONS)))

        self.root = root
        self.imgs = imgs
        self.class_subdirs = separate_datasets(imgs)
        self.classes = classes
        self.class_to_idx = class_to_idx
        self.transform = transform
        self.target_transform = target_transform
        self.loader = loader
        self.equal_sampling = equal_sampling

    def __getitem__(self, index):
        """
        Args:
            index (int): Index
        Returns:
            tuple: (image, target) where target is class_index of the target class.
        """
        if self.equal_sampling:
            class_idx = index % len(self.classes)
            index = index // len(self.classes) % len(self.class_subdirs[class_idx])
            path, target = self.class_subdirs[class_idx][index]
        else:
            path, target = self.imgs[index]
        img = self.loader(path)
        print(path)
        if self.transform is not None:
            img = self.transform(img)
        if self.target_transform is not None:
            target = self.target_transform(target)

        return img, target

    def __len__(self):
        return len(self.imgs)


ds = ImageFolder('testdir/')
print(ds.classes)
print('Global indexing')
for i, d in enumerate(ds.imgs):
    print(i, d)
print('*'*80)
print('Separated class index')
for i, d in enumerate(ds.class_subdirs):
    print(ds.classes[i], '(subdir%d)'%i)
    for j, s in enumerate(d):
        print(j, s)
print('\n\nTest sampling')
for i in ds:
    pass

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
