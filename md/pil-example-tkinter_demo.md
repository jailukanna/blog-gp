---
title: pil example tkinter demo (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'tkinter demo'

Functions in program: 
* `def main():`

Modules used in program: 
* `import numpy as np`
* `import PIL.Image`
* `import PIL.ImageTk`
* `import tkinter.messagebox as tk_messagebox`
* `import tkinter as tk`
* `import math`
* `import argparse`

## python tkinter demo

Python pil example: tkinter demo

```python
#!/usr/bin/env python
import argparse
import math
import tkinter as tk
import tkinter.messagebox as tk_messagebox

import PIL.ImageTk
import PIL.Image
import numpy as np


IMAGE_SIZE = (600, 400)


class CarwashDemoWindow:
    def __init__(self, image_path):
        self.root = tk.Tk()
        self.root.grid_columnconfigure(3, weight=1)
        self.root.grid_columnconfigure(7, weight=1)

        self.image = PIL.Image.open(image_path).convert('L')

        self.label_original = tk.Label(self.root)
        self.label_original.grid(row=0, column=0, columnspan=4)

        self.label_modified = tk.Label(self.root)
        self.label_modified.grid(row=0, column=4, columnspan=4)

        self.set_original_image(self.image)
        self.set_modified_image(self.image)

        self.setup_pixelwise_operation_controls()
        self.setup_thresholding_operation_controls()

    def setup_pixelwise_operation_controls(self):
        self.add_label = tk.Label(self.root, text='Add')
        self.add_label.grid(row=1, column=0, sticky='W')
        self.add_entry = tk.Entry(self.root)
        self.add_entry.grid(row=1, column=1, sticky='W')
        self.add_button = tk.Button(self.root, text='Perform',
                                    command=self.perform_addition)
        self.add_button.grid(row=1, column=2, sticky='W')

        self.multiply_label = tk.Label(self.root, text='Multiply')
        self.multiply_label.grid(row=2, column=0, sticky='W')
        self.multiply_entry = tk.Entry(self.root)
        self.multiply_entry.grid(row=2, column=1, sticky='W')
        self.multiply_button = tk.Button(self.root, text='Perform',
                                         command=self.perform_multiplication)
        self.multiply_button.grid(row=2, column=2, sticky='W')

        self.exponentiate_label = tk.Label(self.root, text='Exponentiate')
        self.exponentiate_label.grid(row=3, column=0, sticky='W')
        self.exponentiate_entry = tk.Entry(self.root)
        self.exponentiate_entry.grid(row=3, column=1, sticky='W')
        self.exponentiate_button = tk.Button(
            self.root, text='Perform', command=self.perform_exponentiation
        )
        self.exponentiate_button.grid(row=3, column=2, sticky='W')

        self.logarithm_label = tk.Label(self.root, text='Logarithm')
        self.logarithm_label.grid(row=4, column=0, sticky='W')
        self.logarithm_button = tk.Button(
            self.root, text='Perform', command=self.perform_logarithm
        )
        self.logarithm_button.grid(row=4, column=1, sticky='W')

        self.negation_label = tk.Label(self.root, text='Negate')
        self.negation_label.grid(row=5, column=0, sticky='W')
        self.negation_button = tk.Button(
            self.root, text='Perform', command=self.perform_negation
        )
        self.negation_button.grid(row=5, column=1, sticky='W')

        self.contrast_label = tk.Label(self.root, text='Contrast')
        self.contrast_label.grid(row=6, column=0, sticky='W')
        self.contrast_button = tk.Button(
            self.root, text='Perform', command=self.perform_contrasting
        )
        self.contrast_button.grid(row=6, column=1, sticky='W')

        self.reset_button = tk.Button(
            self.root, text='Reset', command=self.perform_reset
        )
        self.reset_button.grid(row=7, column=0, sticky='W')

    def setup_thresholding_operation_controls(self):
        self.bernsen_label = tk.Label(
            self.root, text='Bernsen thresholding'
        )
        self.bernsen_label.grid(row=1, column=4, sticky='W')
        self.bernsen_button = tk.Button(
            self.root, text='Perform',
            command=self.perform_bernsen
        )
        self.bernsen_button.grid(row=1, column=5, sticky='W')

        self.niblack_label = tk.Label(
            self.root, text='Niblack thresholding'
        )
        self.niblack_label.grid(row=2, column=4, sticky='W')
        self.niblack_button = tk.Button(
            self.root, text='Perform',
            command=self.perform_niblack
        )
        self.niblack_button.grid(row=2, column=5, sticky='W')

        self.adaptive_label = tk.Label(
            self.root, text='Adaptive thresholding'
        )
        self.adaptive_label.grid(row=3, column=4, sticky='W')
        self.adaptive_button = tk.Button(
            self.root, text='Perform',
            command=self.perform_adaptive
        )
        self.adaptive_button.grid(row=3, column=5, sticky='W')

    def set_original_image(self, image):
        self._scaled_tk_image_original = PIL.ImageTk.PhotoImage(
            self.scale_image(image)
        )
        self.label_original.config(image=self._scaled_tk_image_original)

    def set_modified_image(self, image):
        self.modified_image = image
        self._scaled_tk_image_modified = PIL.ImageTk.PhotoImage(
            self.scale_image(image)
        )
        self.label_modified.config(image=self._scaled_tk_image_modified)

    def perform_addition(self):
        try:
            argument = float(self.add_entry.get())
        except ValueError:
            tk_messagebox.showerror('Error', 'Invalid argument')
            return

        original_pixels = list(self.modified_image.getdata())
        modified_pixels = [
            self.normalize_color_value(original_value + argument)
            for original_value in original_pixels
        ]

        self.show_modified_pixels(modified_pixels)

    def perform_multiplication(self):
        try:
            argument = float(self.multiply_entry.get())
        except ValueError:
            tk_messagebox.showerror('Error', 'Invalid argument')
            return

        original_pixels = list(self.modified_image.getdata())
        modified_pixels = [
            self.normalize_color_value(original_value * argument)
            for original_value in original_pixels
        ]

        self.show_modified_pixels(modified_pixels)

    def perform_exponentiation(self):
        try:
            argument = float(self.exponentiate_entry.get())
        except ValueError:
            tk_messagebox.showerror('Error', 'Invalid argument')
            return

        _, max_value = self.modified_image.getextrema()

        original_pixels = list(self.modified_image.getdata())
        modified_pixels = [
            self.normalize_color_value(
                255 * (original_value / max_value) ** argument
            )
            for original_value in original_pixels
        ]

        self.show_modified_pixels(modified_pixels)

    def perform_logarithm(self):
        _, max_value = self.modified_image.getextrema()

        original_pixels = list(self.modified_image.getdata())
        modified_pixels = [
            self.normalize_color_value(
                255 * (math.log(original_value + 1) / math.log(max_value + 1))
            )
            for original_value in original_pixels
        ]

        self.show_modified_pixels(modified_pixels)

    def perform_negation(self):
        original_pixels = list(self.modified_image.getdata())
        modified_pixels = [
            self.normalize_color_value(255 - original_value)
            for original_value in original_pixels
        ]

        self.show_modified_pixels(modified_pixels)

    def perform_contrasting(self):
        min_value, max_value = self.modified_image.getextrema()
        coeff = 255 / (max_value - min_value)

        original_pixels = list(self.modified_image.getdata())
        modified_pixels = [
            self.normalize_color_value(
                coeff * (original_value - min_value)
            )
            for original_value in original_pixels
        ]

        self.show_modified_pixels(modified_pixels)

    def perform_bernsen(self):
        r = 5
        min_contrast = 15

        pixel_array = np.array(self.image)
        vertical_split = np.split(pixel_array, range(r, self.image.height, r))
        segments = [
            np.split(rows, range(r, self.image.width, r), axis=1)
            for rows in vertical_split
        ]
        for segment_row in segments:
            for segment in segment_row:
                min_value = int(segment.min())
                max_value = int(segment.max())
                mid_value = (min_value + max_value) / 2

                if max_value - min_value <= min_contrast:
                    if mid_value < 128:
                        fill_value = 0
                    else:
                        fill_value = 255
                    segment.fill(fill_value)
                else:
                    for i in range(segment.shape[0]):
                        for j in range(segment.shape[1]):
                            segment[i, j] = (
                                0 if segment[i, j] < mid_value else 255
                            )

        vertical_split = [
            np.concatenate(segment_row, axis=1)
            for segment_row in segments
        ]
        modified_pixels = np.concatenate(vertical_split)
        modified_image = PIL.Image.fromarray(modified_pixels)
        self.set_modified_image(modified_image)

    def perform_niblack(self):
        r = 15
        k = -0.2

        def clipped_range(dimension_size, coord, window):
            radius = (window - 1) // 2
            min_coord = max(0, coord - radius)
            max_coord = min(dimension_size, coord + radius + 1)
            return slice(min_coord, max_coord)

        pixel_array = np.array(self.image)
        new_array = pixel_array.copy()
        for i in range(pixel_array.shape[0]):
            print(i)
            for j in range(pixel_array.shape[1]):
                vertical_slice = clipped_range(pixel_array.shape[0], i, r)
                horizontal_slice = clipped_range(pixel_array.shape[1], j, r)
                segment = pixel_array[vertical_slice, horizontal_slice]

                mean = np.mean(segment)
                stddev = np.std(segment)
                threshold = mean + k * stddev
                new_array[i, j] = (
                    0 if pixel_array[i, j] < threshold else 255
                )

        modified_image = PIL.Image.fromarray(new_array)
        self.set_modified_image(modified_image)

    def perform_adaptive(self):
        k = 3
        alpha = 2 / 3

        def clipped_range(dimension_size, coord, radius):
            min_coord = max(0, coord - radius)
            max_coord = min(dimension_size, coord + radius + 1)
            return slice(min_coord, max_coord)

        pixel_array = np.array(self.image)
        new_array = pixel_array.copy()
        for i in range(pixel_array.shape[0]):
            print(i)
            for j in range(pixel_array.shape[1]):
                current_k = k

                threshold = None
                while True:
                    vertical_slice = clipped_range(
                        pixel_array.shape[0], i, current_k
                    )
                    horizontal_slice = clipped_range(
                        pixel_array.shape[1], j, current_k
                    )
                    segment = pixel_array[vertical_slice, horizontal_slice]

                    mean = np.mean(segment)
                    min_value = segment.min()
                    max_value = segment.max()
                    df_max = abs(max_value - mean)
                    df_min = abs(min_value - mean)

                    if df_max == df_min:
                        if min_value != max_value:
                            current_k += 1
                            continue
                        else:
                            threshold = alpha * mean
                            break
                    else:
                        if df_max > df_min:
                            threshold = alpha * (
                                2 / 3 * min_value +
                                1 / 3 * mean
                            )
                        else:
                            threshold = alpha * (
                                1 / 3 * min_value +
                                2 / 3 * mean
                            )
                        break

                new_array[i, j] = (
                    0 if pixel_array[i, j] < threshold else 255
                )

        modified_image = PIL.Image.fromarray(new_array)
        self.set_modified_image(modified_image)

    def perform_reset(self):
        self.set_modified_image(self.image)

    def show_modified_pixels(self, modified_pixels):
        modified_image = PIL.Image.new(
            'L', (self.image.width, self.image.height)
        )
        modified_image.putdata(modified_pixels)
        self.set_modified_image(modified_image)



def main():
    window = CarwashDemoWindow()
    window.root.mainloop()


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
