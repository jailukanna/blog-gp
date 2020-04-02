---
title: PIL example extract layer nsfw (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'extract layer nsfw'

Functions in program: 
* `def main(argv):`
* `def caffe_preprocess_and_compute_nocrop(pimg, caffe_transformer=None, caffe_net=None,`
* `def resize_image(data, sz=(256, 256)):`

Modules used in program: 
* `import sys`
* `import caffe`
* `import argparse`
* `import sys`
* `import os`
* `import numpy as np`

## python extract layer nsfw

Python PIL example: extract layer nsfw

```python
#!/usr/bin/env python

import numpy as np
import os
import sys
import argparse
from PIL import Image
from StringIO import StringIO
import caffe
import sys

def resize_image(data, sz=(256, 256)):
    """
    Resize image. Please use this resize logic for best results instead of the 
    caffe, since it was used to generate training dataset 
    :param str data:
        The image data
    :param sz tuple:
        The resized image dimensions
    :returns bytearray:
        A byte array with the resized image
    """
    img_data = str(data)
    im = Image.open(StringIO(img_data))
    if im.mode != "RGB":
        im = im.convert('RGB')
    imr = im.resize(sz, resample=Image.BILINEAR)
    fh_im = StringIO()
    imr.save(fh_im, format='JPEG')
    fh_im.seek(0)
    return bytearray(fh_im.read())

def caffe_preprocess_and_compute_nocrop(pimg, caffe_transformer=None, caffe_net=None,
    output_layers=None):
    """
    Run a Caffe network on an input image after preprocessing it to prepare
    it for Caffe.
    :param PIL.Image pimg:
        PIL image to be input into Caffe.
    :param caffe.Net caffe_net:
        A Caffe network with which to process pimg afrer preprocessing.
    :param list output_layers:
        A list of the names of the layers from caffe_net whose outputs are to
        to be returned.  If this is None, the default outputs for the network
        are returned.
    :return:
        Returns the requested outputs from the Caffe net.
    """
    if caffe_net is not None:
        # Grab the default output names if none were requested specifically.
        if output_layers is None:
            output_layers = caffe_net.outputs
        img_data_rs = resize_image(pimg, sz=(256, 256))
        image = caffe.io.load_image(StringIO(img_data_rs))
        # H, W, _ = image.shape
        # _, _, h, w = caffe_net.blobs['data'].data.shape
        # h_off = max((H - h) / 2, 0)
        # w_off = max((W - w) / 2, 0)
        # crop = image[h_off:h_off + h, w_off:w_off + w, :]
        transformed_image = caffe_transformer.preprocess('data', image)
        transformed_image.shape = (1,) + transformed_image.shape
        input_name = caffe_net.inputs[0]
        all_outputs = caffe_net.forward(blobs=output_layers, end=output_layers[0],
                    **{input_name: transformed_image})
        return all_outputs
        # outputs = all_outputs[output_layers[0]][0].astype(float)
        # return outputs
    else:
        print("Error: caffe_net is undefined")
        return []

def main(argv):
    pycaffe_dir = os.path.dirname(__file__)

    parser = argparse.ArgumentParser()
    # Required arguments: input file.
    parser.add_argument(
        "input_file",
        help="Path to the input image file"
    )

    parser.add_argument(
        "--extract-layer",
        required=True,
        help="Layer to extract values from and save to disk ('data', 'conv_1', etc)"
    )

    parser.add_argument(
        "--layer-file",
        required=True,
        help="File to write layer values to"
    )

    # Optional arguments.
    parser.add_argument(
        "--model_def",
        required=True,
        help="Model definition file."
    )
    parser.add_argument(
        "--pretrained_model",
        required=True,
        help="Trained model weights file."
    )

    args = parser.parse_args()

    # Pre-load caffe model.
    nsfw_net = caffe.Net(args.model_def,  # pylint: disable=invalid-name
        args.pretrained_model, caffe.TEST)

    # Load transformer
    # Note that the parameters are hard-coded for best results
    caffe_transformer = caffe.io.Transformer({'data': nsfw_net.blobs['data'].data.shape})
    caffe_transformer.set_transpose('data', (2, 0, 1))  # move image channels to outermost *** convert from HxWxC to CxHxW
    caffe_transformer.set_mean('data', np.array([104, 117, 123]))  # subtract the dataset-mean value in each channel
    caffe_transformer.set_raw_scale('data', 255)  # rescale from [0, 1] to [0, 255]
    caffe_transformer.set_channel_swap('data', (2, 1, 0))  # swap channels from RGB to BGR
    
    image_data = open(args.input_file).read()
    
    # Classify.
    layer_data = caffe_preprocess_and_compute_nocrop(image_data, caffe_transformer=caffe_transformer, caffe_net=nsfw_net, output_layers=[args.extract_layer])

    np.savetxt(args.layer_file, layer_data[args.extract_layer].flatten(), '%.4f') # FYI DeepDetect returns the values with a different number of decimal digits depending on the values 

    # Scores is the array containing SFW / NSFW image probabilities
    # scores[1] indicates the NSFW probability
    # print("NSFW score:  " , scores[1])
    # print(scores)
    # print("{}, {}, {}".format(os.path.basename(f), scores[1], scores[0]))

    print("\n\nSaved layer data (flattened) to: {}".format(args.layer_file))


if __name__ == '__main__':
    main(sys.argv)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
