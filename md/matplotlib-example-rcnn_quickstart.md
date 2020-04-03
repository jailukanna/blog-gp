---
title: matplotlib example rcnn quickstart (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'rcnn quickstart'

Functions in program: 
* `def main():`

Modules used in program: 
* `import pickle`
* `import matplotlib.backends.backend_pdf`
* `import matplotlib`
* `import keras`
* `import numpy`
* `import keras_rcnn.models`
* `import keras_rcnn.utils`
* `import keras_rcnn.preprocessing`
* `import keras_rcnn.datasets.shape`
* `import keras_rcnn`
* `import os`

## python rcnn quickstart

Python matplotlib example: rcnn quickstart

```python
# -*- coding: utf-8 -*-

"""
Object detection
================

Shape dataset
"""

import os

import keras_rcnn
import keras_rcnn.datasets.shape
import keras_rcnn.preprocessing
import keras_rcnn.utils
import keras_rcnn.models
import numpy
import keras

import matplotlib
import matplotlib.backends.backend_pdf

import pickle

print(os.getcwd())


def main():
    training_dictionary, test_dictionary = keras_rcnn.datasets.shape.load_data()

    categories = {"circle": 1, "rectangle": 2, "triangle": 3}

    generator = keras_rcnn.preprocessing.ObjectDetectionGenerator()

    generator = generator.flow_from_dictionary(
        dictionary=training_dictionary,
        categories=categories,
        target_size=(224, 224)
    )

    validation_data = keras_rcnn.preprocessing.ObjectDetectionGenerator()

    validation_data = validation_data.flow_from_dictionary(
        dictionary=test_dictionary,
        categories=categories,
        target_size=(224, 224),
        shuffle=False
    )

    target, _ = generator.next()

    # Plotting expected predictions
    target_bounding_boxes, target_categories, target_images, target_masks, target_metadata = target
    target_images = numpy.squeeze(target_images)
    target_bounding_boxes = numpy.squeeze(target_bounding_boxes)
    target_categories = numpy.argmax(target_categories, -1)
    target_categories = numpy.squeeze(target_categories)
    keras_rcnn.utils.show_bounding_boxes(target_images, target_bounding_boxes, target_categories)
    matplotlib.pyplot.show()
    matplotlib.pyplot.gcf().clear()

    model = keras_rcnn.models.RCNN((224, 224, 3), ["circle", "rectangle", "triangle"])

    optimizer = keras.optimizers.Adam()
    model.compile(optimizer)

    model.fit_generator(
        epochs=100,
        generator=generator,
        validation_data=validation_data
    )

    # TODO: save model
    # model.save('rcnn_quickstart_1_epoch_model.h5')

    predictions = model.predict_generator(validation_data)

    pickle.dump(predictions, open('rcnn_quickstart_100_epochs_model_predictions.p', 'wb'))
    predictions = pickle.load(open('rcnn_quickstart_100_epochs_model_predictions.p', 'rb'))

    # Example prediction on validation data
    (target_bounding_boxes, target_categories, target_images, target_masks, _), _ = validation_data[60]
    target_images = numpy.squeeze(target_images)
    predicted_bad = predictions[0][60]
    predicted_bad = numpy.squeeze(predicted_bad)
    predicted_bad_category = predictions[1][60]
    predicted_bad_category = numpy.squeeze(predicted_bad_category)
    predicted_bad_category_idx = numpy.argmax(predicted_bad_category, axis=1)
    keras_rcnn.utils.show_bounding_boxes(target_images, predicted_bad, predicted_bad_category_idx)
    matplotlib.pyplot.show()
    matplotlib.pyplot.gcf().clear()

    # Plotting perfect predictions first
    pdf = matplotlib.backends.backend_pdf.PdfPages("rcnn_quickstart_100_epochs_model_expected_plot.pdf")
    for i in range(len(validation_data)):
        (target_bounding_boxes, target_categories, target_images, target_masks, _), _ = validation_data[i]
        target_bounding_boxes = numpy.squeeze(target_bounding_boxes)
        target_images = numpy.squeeze(target_images)
        target_categories = numpy.argmax(target_categories, -1)
        target_categories = numpy.squeeze(target_categories)
        keras_rcnn.utils.show_bounding_boxes(target_images, target_bounding_boxes, target_categories)
        pdf.savefig()
        matplotlib.pyplot.gcf().clear()
    pdf.close()

    pdf = matplotlib.backends.backend_pdf.PdfPages("rcnn_quickstart_100_epochs_model_predictions_plot.pdf")
    for i in range(len(validation_data)):
        (target_bounding_boxes, target_categories, target_images, target_masks, _), _ = validation_data[i]
        target_images = numpy.squeeze(target_images)
        predicted_bad = predictions[0][i]
        predicted_bad = numpy.squeeze(predicted_bad)
        predicted_bad_category = predictions[1][i]
        predicted_bad_category = numpy.squeeze(predicted_bad_category)
        predicted_bad_category_idx = numpy.argmax(predicted_bad_category, axis=1)
        keras_rcnn.utils.show_bounding_boxes(target_images, predicted_bad, predicted_bad_category_idx)
        pdf.savefig()
        matplotlib.pyplot.gcf().clear()
    pdf.close()


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
