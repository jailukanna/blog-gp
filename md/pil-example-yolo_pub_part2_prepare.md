---
title: pil example yolo pub part2 prepare (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'yolo pub part2 prepare'

Functions in program: 
* `def draw_grid(size, img):`
* `def write_bb(xml_id):`

## python yolo pub part2 prepare

Python pil example: yolo pub part2 prepare

```python
#Resizing and finding all metrics relative to resized image
#################################################
#Resize to 416 x 416 --> find resized corordinates for bounding boxes and all other necessary items
# 'r' --> resized (and rounded)

new_w_l = 416
new_size = 416

first_df['xmin_r'] = round(first_df['xmin'] * (new_w_l/first_df['width']))
first_df['ymin_r'] = round(first_df['ymin'] * (new_w_l/first_df['height']))
first_df['xmax_r'] = round(first_df['xmax'] * (new_w_l/first_df['width']))
first_df['ymax_r'] = round(first_df['ymax'] * (new_w_l/first_df['height']))

#Finding the mid_pt or center--> in cooridnates --> in resized scale
first_df['mid_x'] = round((first_df['xmax_r'] - first_df['xmin_r'])/2 + first_df['xmin_r'])
first_df['mid_y'] = round((first_df['ymax_r'] - first_df['ymin_r'])/2 + first_df['ymin_r'])

#Size of bbox width and height --> rescaled wrt new image size
first_df['w_size'] = round((first_df['xmax'] - first_df['xmin'])*new_w_l/first_df['width'])
first_df['h_size'] = round((first_df['ymax'] - first_df['ymin'])*new_w_l/first_df['height'])

#Relative location of mid_pts (centre) and relative size of w/h --> wrt whole image and not grid
first_df['x_r'] = first_df['mid_x']/new_size
first_df['y_r'] = first_df['mid_y']/new_size
first_df['w_r'] = first_df['w_size']/new_size
first_df['h_r'] = first_df['h_size']/new_size
########################################################


#Integers coding of objects classes
##############################################
from collections import OrderedDict


#Make 2 dictionaries for indexing objects and vice versa
all_obj= (list(set(first_df.obj_cla)))
sorted_obj = sorted(all_obj)
ix_2_ob = OrderedDict(zip([i for i in range(len(sorted_obj))], sorted_obj))
ob_2_ix = OrderedDict(zip(sorted_obj, [i for i in range(len(sorted_obj))]))

#Make a list which translates all objects in dataframe to integer categories
obj_cat_all = [ob_2_ix.get(key) for key in first_df.obj_cla]

#Finally update the table to include objects as integers
first_df.insert(5, 'obj_cat', obj_cat_all)
##############################################


#Assigning anchor boxes and grid address
##############################################
#Assigning anchor boxes --> to keep it simple, here just 2 anchor boxes are selected
#1 --> broad, 0 --> tall --> much fewer tall objects --> probablay needs better grouping
first_df['anchor'] = (first_df['w_size']/first_df['h_size'] > 1) * 1


#Assigning bounding boxes to grids --> img is divided into 9 (3x3) grids arbitrarily
#Both cell address (x,y) and  cell num --> starting top left --> right --> down --> down right --> grid number is found
#single pixel as center --> grid sie of 416 x 416, --> move from top left to bottom right as an alternative

#grid address as location
x_loc = (first_df['x_r']/0.33).astype(int)
y_loc = (first_df['y_r']/0.33).astype(int)
first_df['grid_a'] = list(zip(x_loc, y_loc))

#grid address as number (from 0 to 8, starting top-left, going left to right and down)
grid_n = x_loc  + y_loc * 3
first_df['grid_n'] = grid_n

#Center as single point/pixel
first_df['pix_pt'] = (first_df.mid_x + first_df.mid_y * 416).astype(int) #Not used here
########################################################


#Visualizing resized image with rescaled boxes, labels, anchors and grid information for verification
########################################################

#To check all derivations are correct --> resize the image and put the bounding box labeling center, anchor(orientation)
from IPython.display import display 
from PIL import ImageDraw
from PIL import ImageDraw, ImageFont


from torchvision import transforms
trans = transforms.Compose([transforms.Resize((416, 416))])

def write_bb(xml_id):
    sel_img_x = first_df[first_df['xml_id'] ==xml_id].iloc[:, np.r_[1, 4, 10:16, -4, -2]]
    pil_img = show_pil_img(sel_img_x.iloc[0, 0])
    #trans = transforms.Compose([transforms.Resize((416, 416))])
    im = trans(pil_img)
    
    draw = ImageDraw.Draw(im)
    #Draw a bounding box in resized image (using revised parameters) --> put the class of object at the center
    for i in range(sel_img_x.shape[0]):
        draw.rectangle(tuple(sel_img_x.iloc[i, 2:6].values), outline=(255, 255, 255))
        draw.text(tuple(sel_img_x.iloc[i, 6:8].values),sel_img_x.iloc[i, 1], stroke_width=0,  fill=(255,255,0,128))
        draw.text(tuple(sel_img_x.iloc[i, 6:8].values + (0, -10)),str(sel_img_x.iloc[i, -2]), stroke_width=0,  fill=(255,255,0,128))
        draw.text(tuple(sel_img_x.iloc[i, 6:8].values +(0, 10)),str(sel_img_x.iloc[i, -1]), stroke_width=0,  fill=(255,255,0,128))
    return im

#Put 3 x3 grid to verify that grids are correct
def draw_grid(size, img):
    w = img.size[0]
    h = w = img.size[1]
    get_cordinates = []
    draw = ImageDraw.Draw(img)
    
    for i in range(size):
        
        xmin_h = 0
        ymin_h = h//size * i
        xmax_h = w
        ymax_h = h//size * i
        draw.line((xmin_h, ymin_h, xmax_h, ymax_h), width = 2)
        
        xmin_v = w//size * i
        ymin_v = 0
        xmax_v = w//size * i
        ymax_v = h
        draw.line((xmin_v, ymin_v, xmax_v, ymax_v),  width = 1)
        
    return img
  ########################################################


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
