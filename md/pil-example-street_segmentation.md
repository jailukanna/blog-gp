---
title: pil example street segmentation (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'street segmentation'

Functions in program: 
* `def parse_pred(pred, n_classes):`
* `def get_n_rgb_colors(n):`
* `def segment(graph, image_file):`
* `def load_graph(frozen_graph_filename):`

Modules used in program: 
* `import numpy as np`
* `import tensorflow as tf`

## python street segmentation

Python pil example: street segmentation

```python
import tensorflow as tf
import numpy as np

from PIL import Image

def load_graph(frozen_graph_filename):
    """
    Args:
        frozen_graph_filename (str): Full path to the .pb file.
    """
    # We load the protobuf file from the disk and parse it to retrieve the
    # unserialized graph_def
    with tf.gfile.GFile(frozen_graph_filename, "rb") as f:
        graph_def = tf.GraphDef()
        graph_def.ParseFromString(f.read())

    # Then, we import the graph_def into a new Graph and returns it
    with tf.Graph().as_default() as graph:
        # The name var will prefix every op/nodes in your graph
        # Since we load everything in a new graph, this is not needed
        tf.import_graph_def(graph_def, name="prefix")
        return graph

def segment(graph, image_file):
    """
    Does the segmentation on the given image.
    Args:
        graph (Tensorflow Graph)
        image_file (str): Full path to your image
    Returns:
        segmentation_mask (np.array): The segmentation mask of the image.
    """
    # We access the input and output nodes
    x = graph.get_tensor_by_name('prefix/ImageTensor:0')
    y = graph.get_tensor_by_name('prefix/SemanticPredictions:0')

    # We launch a Session
    with tf.Session(graph=graph) as sess:

        image = Image.open(image_file)
        image = image.resize((299, 299))
        image_array = np.array(image)
        image_array = np.expand_dims(image_array, axis=0)

        # Note: we don't nee to initialize/restore anything
        # There is no Variables in this graph, only hardcoded constants
        pred = sess.run(y, feed_dict={x: image_array})

        pred = pred.squeeze()

    return pred

def get_n_rgb_colors(n):
    """
    Get n evenly spaced RGB colors.
    Returns:
        rgb_colors (list): List of RGB colors.
    """
    max_value = 16581375 #255**3
    interval = int(max_value / n)
    colors = [hex(I)[2:].zfill(6) for I in range(0, max_value, interval)]

    rgb_colors = [(int(i[:2], 16), int(i[2:4], 16), int(i[4:], 16)) for i in colors]

    return rgb_colors

def parse_pred(pred, n_classes):
    """
    Parses a prediction and returns the prediction as a PIL.Image.
    Args:
        pred (np.array)
    Returns:
        parsed_pred (PIL.Image): Parsed prediction that we can view as an image.
    """
    uni = np.unique(pred)

    empty = np.empty((pred.shape[1], pred.shape[0], 3))

    colors = get_n_rgb_colors(n_classes)

    for i, u in enumerate(uni):
        idx = np.transpose((pred == u).nonzero())
        c = colors[u]
        empty[idx[:,0], idx[:,1]] = [c[0],c[1],c[2]]

    parsed_pred = np.array(empty, dtype=np.uint8)
    parsed_pred = Image.fromarray(parsed_pred)

    return parsed_pred

if __name__ == '__main__':
    N_CLASSES = THE NUMBER OF CLASSES YOUR MODEL HAS
    MODEL_FILE = 'FULL PATH TO YOUR PB FILE'
    IMAGE_FILE = 'FULL PATH TO YOUR IMAGE'

    graph = load_graph(MODEL_FILE)
    prediction = segment(graph, IMAGE_FILE)
    segmented_image = parse_pred(prediction, N_CLASSES)

    segmented_image.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
