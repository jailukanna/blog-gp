---
title: pil example model activation visualization (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'model activation visualization'

Functions in program: 
* `def get_activation(name):`
* `def pil_to_np(img_PIL):`
* `def get_image(path, imsize=-1):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import torchvision`
* `import torch`
* `import PIL`

## python model activation visualization

Python pil example: model activation visualization

```python
import PIL
import torch
import torchvision
import numpy as np
import matplotlib.pyplot as plt


def get_image(path, imsize=-1):
    """Load an image and resize to a cpecific size. 

    Args: 
        path: path to image
        imsize: tuple or scalar with dimensions; -1 for `no resize`
    """
    """Load PIL image."""
    img = PIL.Image.open(path).convert('RGB')

    if isinstance(imsize, int):
        imsize = (imsize, imsize)

    img_np = pil_to_np(img)

    return img_np

def pil_to_np(img_PIL):
    '''Converts image in PIL format to np.array.
    
    From W x H x C [0...255] to C x W x H [0..1]
    '''
    ar = np.array(img_PIL)

    if len(ar.shape) == 3:
        ar = ar.transpose(2,0,1)
    else:
        ar = ar[None, ...]

    return ar.astype(np.float32) / 255.


# Get image as numpy
# You can download this F16_GT.png here: https://github.com/DmitryUlyanov/deep-image-prior/raw/master/data/denoising/F16_GT.png
image = np.expand_dims(get_image("F16_GT.png"),axis=0)
print(image.shape)
data = torch.Tensor(image)


# Use pretrain Resnet18
model = torchvision.models.resnet18(pretrained=True)

# Regist forward_hook
activation = {}
def get_activation(name):
    def hook(model, input, output):
        activation[name] = output.detach()
    return hook


# For each conv layer in a model
for name, module in model.named_modules():
    if 'conv' in name:
        # Visualize feature maps
        module.register_forward_hook(get_activation(name))
        output = model(data)
        act = activation[name].squeeze()
        means = torch.Tensor(np.zeros_like(act[0]))
        # Mean activations of each filters in a given layer
        for idx in range(act.size(0)):
            means += act[idx]
        plt.imsave('./F16_GT/{}.png'.format(name),means)


        
        
# Visualizing gradient
from captum.attr import IntegratedGradients
# Get target label index
target = model(data).argmax().item()
ig = IntegratedGradients(model)

# Get integrated gradient of image
# More details regarding the integrated gradient method can be found in the
# original paper here:
# https://arxiv.org/abs/1703.01365
attributions, delta = ig.attribute(data, target=target, return_convergence_delta=True)

# Normalize and multiply original image with its gradient
image = attributions.squeeze().numpy()
image = normalize(image)
image = image_input * image
image = np.swapaxes(image.squeeze() ,0,2)

plt.imsave('IG_Attributions_F16_GT.png',image.squeeze())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
