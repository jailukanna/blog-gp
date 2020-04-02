---
title: PIL example extract fish from eventmeasure (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'extract fish from eventmeasure'

Functions in program: 
* `def draw_box(image, box, color, thickness=2):`
* `def find_fish_bounds(lx0, ly0, lx1, ly1):`
* `def find_fish_bounds1(lx0, ly0, lx1, ly1):`

Modules used in program: 
* `import os`
* `import cv2`
* `import numpy as np`
* `import pandas`

## python extract fish from eventmeasure

Python PIL example: extract fish from eventmeasure

```python
import pandas
import numpy as np
import cv2
import os
from PIL import Image

def find_fish_bounds1(lx0, ly0, lx1, ly1):

    original_lx0 = lx0
    original_lx1 = lx1
    original_ly0 = ly0
    original_ly1 = ly1

    print("Orig", lx0, ly0, lx1, ly1)

    # if fish is measured right to left, we need to swap the xys so we can draw a bounding box consistently
    lx0 = original_lx0 if original_lx0 < original_lx1 else original_lx1
    lx1 = original_lx0 if original_lx0 > original_lx1 else original_lx1
    ly0 = original_ly0 if original_ly0 < original_ly1 else original_ly0
    ly1 = original_ly0 if original_ly0 > original_ly1 else original_ly0

    # adjust the xys to crop a bounding box
    width = lx1 - lx0
    quarter_width = width / 4

    if not ((ly1-ly0) > (quarter_width*2)):
        ly0 -= quarter_width
        ly1 += quarter_width

    #if lx0 == lx1 or ly0 == ly1:
    #    continue

    if lx0 >= lx1 or ly0 >= ly1:
        print("Adju", lx0, ly0, lx1, ly1)
        print("False")
        #exit(0)

    return int(lx0), int(ly0), int(lx1), int(ly1)


def find_fish_bounds(lx0, ly0, lx1, ly1):

    original_lx0 = lx0
    original_lx1 = lx1
    original_ly0 = ly0
    original_ly1 = ly1

    #print("Orig", lx0, ly0, lx1, ly1)

    # if fish is measured right to left, we need to swap the xys so we can draw a bounding box consistently
    lx0 = original_lx0 if original_lx0 < original_lx1 else original_lx1
    lx1 = original_lx0 if original_lx0 > original_lx1 else original_lx1
    ly0 = original_ly0 if original_ly0 < original_ly1 else original_ly1
    ly1 = original_ly0 if original_ly0 > original_ly1 else original_ly1
    midy = ly0 + ((ly1-ly0)/2)
    diffy = ly1 - ly0

    # adjust the xys to crop a bounding box
    width = lx1 - lx0
    quarter_width = width / 3

    ly0_tmp = midy - quarter_width
    ly1_tmp = midy + quarter_width

    if ((diffy) < (ly1_tmp-ly0_tmp)):
        ly0 = midy - quarter_width
        ly1 = midy + quarter_width

    if ly0 < 0:
        ly0=0

    #if lx0 == lx1 or ly0 == ly1:
    #    continue

    if lx0 >= lx1 or ly0 >= ly1:
        print("Adju", lx0, ly0, lx1, ly1)
        print("False")
        #exit(0)

    return int(lx0), int(ly0), int(lx1), int(ly1)


def draw_box(image, box, color, thickness=2):
    """ Draws a box on an image with a given color.

    # Arguments
        image     : The image to draw on.
        box       : A list of 4 elements (x1, y1, x2, y2).
        color     : The color of the box.
        thickness : The thickness of the lines to draw a box with.
    """
    b = np.array(box).astype(int)
    cv2.rectangle(image, (b[0], b[1]), (b[2], b[3]), color, thickness, cv2.LINE_AA)


if __name__ == "__main__":

    em_lengths_file = "/path/file_Lengths.txt"
    em_image_pt_pair_file = "/path/file_ImagePtPair.txt"
    VIDEO_BASE_DIR = "/path/"
    FRAMES_OUTPUT_DIR = "/path/"

    df_em_lengths = pandas.read_csv(em_lengths_file, sep='\t', lineterminator='\r')
    df_em_pt_pair = pandas.read_csv(em_image_pt_pair_file, sep='\t', lineterminator='\r')

    df = pandas.merge(df_em_lengths, df_em_pt_pair, on=['OpCode', 'ImagePtPair'])
    df["Index"] = pandas.to_numeric(df["Index"])
    df["FrameLeft"] = pandas.to_numeric(df["FrameLeft"])

    op_codes = df.OpCode.unique()
    point_pairs = df.ImagePtPair.unique()
    filenames = df.FilenameLeft.unique()

    '''
    species_list = df.Species.unique()
    genus_list = df.Genus.unique()
    family_list = df.Family.unique()

    def write_list(filename, the_list):
        with open(filename, "w") as text_file:
            for index, name in enumerate(the_list):
                text_file.write(str(name) + "," + str(index) + "\n")

    write_list("species_ids.csv", species_list)
    write_list("genus_ids.csv", genus_list)
    write_list("family_ids.csv", family_list)

    exit(0)
    '''
    '''
    for filename in filenames:
        exists = os.path.exists(os.path.join(VIDEO_BASE_DIR, filename))
        print(os.path.join(VIDEO_BASE_DIR, filename), exists)
    '''

    species_df = pandas.DataFrame(columns=['filename', 'x0', 'y0', 'x1', 'y1', 'label'])
    genus_df = pandas.DataFrame(columns=['filename', 'x0', 'y0', 'x1', 'y1', 'label'])
    family_df = pandas.DataFrame(columns=['filename', 'x0', 'y0', 'x1', 'y1', 'label'])
    count = 0
    for code in op_codes:
        for point in point_pairs:

            if df[(df.OpCode == code) & (df.ImagePtPair == point)].shape[0] == 0:
                continue

            filename = df[(df.OpCode == code) & (df.ImagePtPair == point)]["FilenameLeft"].values[0]
            frame = df[(df.OpCode == code) & (df.ImagePtPair == point)]["FrameLeft"].values[0]
            lx0 = df[(df.OpCode == code) & (df.ImagePtPair == point) & (df.Index == 0)]["Lx"].values[0]
            ly0 = df[(df.OpCode == code) & (df.ImagePtPair == point) & (df.Index == 0)]["Ly"].values[0]
            lx1 = df[(df.OpCode == code) & (df.ImagePtPair == point) & (df.Index == 1)]["Lx"].values[0]
            ly1 = df[(df.OpCode == code) & (df.ImagePtPair == point) & (df.Index == 1)]["Ly"].values[0]

            lx_0 = int(lx0)
            ly_0 = int(ly0)
            lx_1 = int(lx1)
            ly_1 = int(ly1)
            midx = int(lx0 + ((lx1 - lx0) / 2))
            midy = int(ly0 + ((ly1 - ly0) / 2))

            frame = int(frame)

            species = df[(df.OpCode == code) & (df.ImagePtPair == point)]["Species"].values[0]
            genus = df[(df.OpCode == code) & (df.ImagePtPair == point)]["Genus"].values[0]
            family = df[(df.OpCode == code) & (df.ImagePtPair == point)]["Family"].values[0]

            #if lx0 > lx1:
            #    continue

            lx0, ly0, lx1, ly1 = find_fish_bounds(lx0, ly0, lx1, ly1)


            if lx0 >= lx1 or ly0 >= ly1:
                print("skipping", filename, frame, lx0, ly0, lx1, ly1, family, genus, species)
                continue

            #print(filename, frame, lx0, ly0, lx1, ly1, family, genus, species)


            video = os.path.join(VIDEO_BASE_DIR, filename)

            if not os.path.exists(video):
                print("continuing")
                continue

            output_frame_path = os.path.join(FRAMES_OUTPUT_DIR, filename + "." + str(int(frame)) + ".png")

            species_df = species_df.append(
                {"filename": output_frame_path, "x0": lx0, "y0": ly0, "x1": lx1, "y1": ly1, "label": species},
                ignore_index=True)
            genus_df = genus_df.append(
                {"filename": output_frame_path, "x0": lx0, "y0": ly0, "x1": lx1, "y1": ly1, "label": genus},
                ignore_index=True)
            family_df = family_df.append(
                {"filename": output_frame_path, "x0": lx0, "y0": ly0, "x1": lx1, "y1": ly1, "label": family},
                ignore_index=True)

#            if os.path.exists(output_frame_path):
#                continue

            cap = cv2.VideoCapture(video)  # video_name is the video being called
            #amount_of_frames = cap.get(cv2.CAP_PROP_FRAME_COUNT)
            #print(amount_of_frames)
            cap.set(cv2.CAP_PROP_POS_FRAMES, int(frame))  # Where frame_no is the frame you want
            ret, frame = cap.read()  # Read the frame

            pil_image = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
            pil_image = Image.fromarray(pil_image)
            pil_image.save(output_frame_path, "PNG")


            draw_box(frame, (lx0, ly0, lx1, ly1), (0, 255, 0))
            cv2.circle(frame, (lx_0, ly_0), 3, (0, 0, 255), 2, cv2.LINE_AA)
            cv2.circle(frame, (lx_1, ly_1), 3, (0, 0, 255), 2, cv2.LINE_AA)
            cv2.circle(frame, (midx, midy), 3, (0, 0, 255), 2, cv2.LINE_AA)

            #image_utils.draw_bounding_box(frame, lx_0, ly_0, lx_1, ly_1, "yellow")
            #frame.show()
            #time.sleep(8)


            cv2.imshow('window_name', frame)
            if cv2.waitKey() == ord('q'):
                exit(0)


            print(df[(df.OpCode == code) & (df.ImagePtPair == point) & (df.Index == 0)]["Lx"].values[0])
            print(df[(df.OpCode == code) & (df.ImagePtPair == point) & (df.Index == 0)]["Ly"].values[0])
            print(df[(df.OpCode == code) & (df.ImagePtPair == point) & (df.Index == 0)]["Rx"].values[0])
            print(df[(df.OpCode == code) & (df.ImagePtPair == point) & (df.Index == 0)]["Ry"].values[0])

            print(df[(df.OpCode == code) & (df.ImagePtPair == point) & (df.Index == 1)]["Lx"].values[0])
            print(df[(df.OpCode == code) & (df.ImagePtPair == point) & (df.Index == 1)]["Ly"].values[0])
            print(df[(df.OpCode == code) & (df.ImagePtPair == point) & (df.Index == 1)]["Rx"].values[0])
            print(df[(df.OpCode == code) & (df.ImagePtPair == point) & (df.Index == 1)]["Ry"].values[0])

            #print(df[(df.OpCode == code) & (df.Index == 0)]["Ly"][0])

    species_df.to_csv("./outputs/species_train.csv")
    family_df.to_csv("./outputs/family_train.csv")
    genus_df.to_csv("./outputs/genus_train.csv")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
