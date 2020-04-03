---
title: matplotlib example crosshairs for drift correction (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'crosshairs for drift correction'

Functions in program: 
* `def crosshair_milling(center_coordinate_pixels, image, milling_depth=2e-6,`
* `def cut_crosshair_patch(center_coordinate_pixels, image):`

Modules used in program: 
* `import autoscript_toolkit.vision as vision_toolkit`
* `import numpy as np`

## python crosshairs for drift correction

Python matplotlib example: crosshairs for drift correction

```python
import numpy as np

import autoscript_toolkit.vision as vision_toolkit


def cut_crosshair_patch(center_coordinate_pixels, image):
    """Returns cropped image region with crosshair drift correction reference.
    
    Parameters
    ----------
    center_coordinate_pixels : List-like
        Row, column format & units of pixels.
    image : AdornedImage
    
    Returns
    -------
    cropped_image
        AdornedImage of cropped region centered at position of crosshairs.
    """
    center_y_pixels, center_x_pixels = center_coordinate_pixels
    cropped_image_size = [0.1 * np.min(image.data.shape)] * 2
    cropped_image = vision_toolkit.cut_image(image, center=np.flip(center_coordinate_pixels, axis=0), size=cropped_image_size)
    return cropped_image

  
def crosshair_milling(center_coordinate_pixels, image, milling_depth=2e-6,
                      display_matplotlib=False):
    """Returns crosshair milling lines for drift correction reference.
    
    Parameters
    ----------
    center_coordinate_pixels : List-like
        Row, column format & units of pixels.
    image : AdornedImage
    milling_depth : float, optional
        Depth to mill crosshair pattern, by default 2e-6
    display_matplotlib : bool, optional
        Visualize crosshair pattern with matplotlib, by default False
    
    Returns
    -------
    crosshair_horizontal, crosshair_vertical
        Autoscript line patterns for ion beam milling.
    """
    microscope.patterning.clear_patterns()
    center_y_pixels, center_x_pixels = center_coordinate_pixels
    half_length_pixels = 0.05 * np.min(image.data.shape)
    # Pixel coordinates
    # Horizontal line
    start_horizontal_x_pixels = numpy.clip(
        center_x_pixels - half_length_pixels, 1, image.data.shape[1] - 1)
    start_horizontal_y_pixels = numpy.clip(
        center_y_pixels, 1, image.data.shape[0])
    end_horizontal_x_pixels = numpy.clip(
        center_x_pixels + half_length_pixels, 1, image.data.shape[1] - 1)
    end_horizontal_y_pixels = numpy.clip(
        center_y_pixels, 1, image.data.shape[0] - 1)
    # Vertical line
    start_vertical_x_pixels = numpy.clip(
        center_x_pixels, 1, image.data.shape[1] - 1)
    start_vertical_y_pixels = numpy.clip(
        center_y_pixels - half_length_pixels, 1, image.data.shape[0] - 1)
    end_vertical_x_pixels = numpy.clip(
        center_x_pixels, 1, image.data.shape[1] - 1)
    end_vertical_y_pixels = numpy.clip(
        center_y_pixels + half_length_pixels, 1, image.data.shape[0] - 1)
    # Visualize
    if display_matplotlib:
        plt.imshow(image.data, cmap='gray')
        plt.plot([start_horizontal_x_pixels, end_horizontal_x_pixels],
                 [start_horizontal_y_pixels, end_horizontal_y_pixels], 'y')
        plt.plot([start_vertical_x_pixels, end_vertical_x_pixels],
                 [start_vertical_y_pixels, end_vertical_y_pixels], 'y')
        plt.show()

    # Realspace coordinates
    pixel_size = image.metadata.binary_result.pixel_size
    half_length_realspace = half_length_pixels * pixel_size.x
    center_realspace = _pixel_coord_to_realspace(
        center_coordinate_pixels, pixel_size, image.data.shape)
    # Horizontal milling line pattern
    start_horizontal_y, start_horizontal_x = _pixel_coord_to_realspace(
        [start_horizontal_y_pixels, start_horizontal_x_pixels],
        pixel_size, image.data.shape)
    end_horizontal_y, end_horizontal_x = _pixel_coord_to_realspace(
        [end_horizontal_y_pixels, end_horizontal_x_pixels],
        pixel_size, image.data.shape)
    # Vertical milling line pattern
    start_vertical_y, start_vertical_x = _pixel_coord_to_realspace(
        [start_vertical_y_pixels, start_vertical_x_pixels],
        pixel_size, image.data.shape)
    end_vertical_y, end_vertical_x = _pixel_coord_to_realspace(
        [end_vertical_y_pixels, end_vertical_x_pixels],
        pixel_size, image.data.shape)
    # Create Autoscript miling patterns for crosshairs
    crosshair_horizontal = microscope.patterning.create_line(
        start_horizontal_x, start_horizontal_y,
        end_horizontal_x, end_horizontal_y, milling_depth)
    crosshair_vertical = microscope.patterning.create_line(
        start_vertical_x, start_vertical_y,
        end_vertical_x, end_vertical_y, milling_depth)
    return crosshair_horizontal, crosshair_vertical

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
