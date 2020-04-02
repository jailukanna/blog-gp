---
title: PIL example style transfer (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'style transfer'

Functions in program: 
* `def main():`
* `def transfer_style(model, content_image_path, style_image_path, gen_img_folder, img_size,`
* `def total_variation_loss(tensor):`
* `def calculate_style_loss(style_image_features, gen_image_features, style_layer_weights):`
* `def gram_matrix(tensor):`
* `def calculate_content_loss(content_image_features, gen_image_features, content_layer):`
* `def get_features(model, image, vgg_layers):`
* `def load_image(img_path, img_size, transform):`

Modules used in program: 
* `import os`
* `import time`
* `import torch.nn.functional as F`
* `import torchvision.transforms.functional as VF`
* `import torch.optim as optim`
* `import torch.nn as nn`
* `import torch`
* `import argparse`

## python style transfer

Python PIL example: style transfer

```python

import argparse
import torch
import torch.nn as nn
import torch.optim as optim
from torchvision import models, transforms
import torchvision.transforms.functional as VF
import torch.nn.functional as F
from torch.utils.tensorboard import SummaryWriter
from PIL import Image
import time
import os


parser = argparse.ArgumentParser()
parser.add_argument("--content_weight", type=float, default=1e5)
parser.add_argument("--style_weight", type=float, default=1e2)
parser.add_argument("--tv_weight", type=float, default=1e1)
parser.add_argument("--vgg_model", default="vgg19")
parser.add_argument("--lambda_noise", default=1,  help="G_init = lambda_noise*C+(1-lambda)*N")
parser.add_argument("--content_image_path", required=True)
parser.add_argument("--style_image_path", required=True)
parser.add_argument("--gen_img_folder", default="generated/")
parser.add_argument("--img_size", default=512)
parser.add_argument("--num_iterations", default=50000)
parser.add_argument("--learning_rate", default=1e-3)
# parser.add_argument("--log_dir", default="logs/{}/".format(int(time.time())))
parser.add_argument("--log_dir", default="logs/")


def load_image(img_path, img_size, transform):
    image = Image.open(img_path).convert('RGB')
    image = image.resize((img_size, img_size), Image.BICUBIC)
    image = transform(image)
    return image.unsqueeze(0)


def get_features(model, image, vgg_layers):
    x = image
    img_features = {}
    for num, layer in enumerate(model.features):
        x = layer(x)
        if num in vgg_layers.keys():
            img_features[vgg_layers[num]] = x
    return img_features


def calculate_content_loss(content_image_features, gen_image_features, content_layer):
    feat_content = content_image_features[content_layer]
    feat_gen = gen_image_features[content_layer]
    return F.mse_loss(feat_content, feat_gen)


def gram_matrix(tensor):
    _, c, h, w = tensor.shape
    tensor = tensor.view(c, h*w)
    return torch.mm(tensor, tensor.T)


def calculate_style_loss(style_image_features, gen_image_features, style_layer_weights):
    style_loss = 0
    for layer, weight in style_layer_weights.items():
        feat_style = style_image_features[layer]
        feat_gen = gen_image_features[layer]
        GM_S = gram_matrix(feat_style)
        GM_G = gram_matrix(feat_gen)
        c, c = GM_S.shape
        layer_loss = F.mse_loss(GM_S, GM_G)
        style_loss += weight*layer_loss
    return style_loss


def total_variation_loss(tensor):
    x1 = tensor[:,:,1:,:]
    x2 = tensor[:,:,:-1,:]
    y1 = tensor[:,:,:,1:]
    y2 = tensor[:,:,:,:-1]
    return torch.sum(torch.abs(x1-x2)) + torch.sum(torch.abs(y1-y2))


def transfer_style(model, content_image_path, style_image_path, gen_img_folder, img_size,
        lambda_noise, content_weight, style_weight, tv_weight, content_layer, style_layer_weights, vgg_layers, device, learning_rate, num_iterations, writer):
    # transform = transforms.Compose([transforms.ToTensor(), transforms.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225))])
    # untransform = transforms.Compose([transforms.Normalize((-0.485, -0.456, -0.406), (0.229, 0.224, 0.225))])
    transform = transforms.Compose([transforms.ToTensor()])
    content_image = load_image(content_image_path, img_size, transform)
    style_image = load_image(style_image_path, img_size, transform)
    noise_image = torch.rand(img_size, img_size)
    gen_image = lambda_noise*content_image + (1-lambda_noise)*noise_image

    model = model.to(device)
    content_image = content_image.to(device)
    style_image = style_image.to(device)
    gen_image = gen_image.to(device)

    gen_img_path = "{}/{}.jpg".format(gen_img_folder, "000")
    img_to_save = gen_image[0].cpu()
    # VF.to_pil_image(untransform(img_to_save)).save(gen_img_path)
    VF.to_pil_image(img_to_save).save(gen_img_path)

    # replace all max pool with avg pool
    for i, layer in enumerate(model.features):
        if isinstance(layer, torch.nn.MaxPool2d):
            model.features[i] = torch.nn.AvgPool2d(kernel_size=2, stride=2, padding=0)

    for param in model.parameters():
        param.requires_grad=False

    gen_image.requires_grad = True
    optimizer = optim.Adam(params=[gen_image], lr=learning_rate)

    for itr in range(num_iterations):
        optimizer.zero_grad()
        content_image_features = get_features(model, content_image, vgg_layers)
        style_image_features = get_features(model, style_image, vgg_layers)
        gen_image_features = get_features(model, gen_image, vgg_layers)

        content_loss = calculate_content_loss(content_image_features, gen_image_features, content_layer)
        style_loss = calculate_style_loss(style_image_features, gen_image_features, style_layer_weights)
        # tv_loss = total_variation_loss(gen_image)
        # total_loss = content_weight*content_loss + style_weight*style_loss + tv_weight*tv_loss
        total_loss = content_weight*content_loss + style_weight*style_loss

        # print("Iteration = {}: Content Loss = {}, Style Loss = {}, TV Loss = {}, Total Loss = {}".format(itr, content_loss, style_loss, tv_loss, total_loss))
        print("Iteration = {}: Content Loss = {}, Style Loss = {}, Total Loss = {}".format(itr, content_loss, style_loss, total_loss))

        writer.add_scalar("Content Loss", content_loss, itr)
        writer.add_scalar("Style Loss", style_loss, itr)
        # writer.add_scalar("TV Loss", tv_loss, itr)
        writer.add_scalar("Total Loss", total_loss, itr)

        total_loss.backward(retain_graph=True)
        optimizer.step()

        if itr%10==0:
            gen_img_path = "{}/{}.jpg".format(gen_img_folder, int(time.time()))
            img_to_save = gen_image[0].cpu()
            # VF.to_pil_image(untransform(img_to_save)).save(gen_img_path)
            VF.to_pil_image(img_to_save).save(gen_img_path)


def main():
    args = parser.parse_args()
    content_weight = args.content_weight
    style_weight = args.style_weight
    tv_weight = args.tv_weight
    lambda_noise  = args.lambda_noise
    vgg_model = args.vgg_model
    gen_img_folder = args.gen_img_folder
    content_image_path = args.content_image_path
    style_image_path = args.style_image_path
    img_size = args.img_size
    log_dir = args.log_dir
    learning_rate = args.learning_rate
    num_iterations = args.num_iterations

    model_options = {"vgg16": models.vgg16(pretrained=True),
                     "vgg19": models.vgg19(pretrained=True)
                    }
    model = model_options[vgg_model]
    os.makedirs(gen_img_folder, exist_ok=True)
    writer = SummaryWriter(log_dir=log_dir)
    device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
    print("Device: {}".format(device))

    vgg_layers = {0: 'conv1_1', 5: 'conv2_1', 10: 'conv3_1', 19: 'conv4_1', 21: 'conv4_2', \
                    28: 'conv5_1', 30: 'conv5_2'} # 30 is content layer

    content_layer = "conv5_2"
    # style_layer_weights = {'conv1_1': 0.75, 'conv2_1': 0.5, 'conv3_1': 0.2, 'conv4_1': 0.2, 'conv5_1': 0.2}
    style_layer_weights = {'conv1_1': 0.2, 'conv2_1': 0.2, 'conv3_1': 0.2, 'conv4_1': 0.2, 'conv5_1': 0.2}

    transfer_style(model, content_image_path, style_image_path, gen_img_folder, img_size, \
        lambda_noise, content_weight, style_weight, tv_weight, content_layer, style_layer_weights, vgg_layers, device, learning_rate, num_iterations, writer)


if __name__=="__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
