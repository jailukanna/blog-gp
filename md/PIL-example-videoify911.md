---
title: PIL example videoify911 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'videoify911'

Functions in program: 
* `def restartmetm():`
* `def resizeme(img, x, y):`
* `def zoom_at(img, x, y, zoom):`
* `def shift_hue(arr,hout):`
* `def savearray(a, filename, fmt='png'):`
* `def hsv_to_rgb(hsv):`
* `def rgb_to_hsv(rgb):`

Modules used in program: 
* `import sys`
* `import cv2`
* `import caffe`
* `import math`
* `import PIL.Image`
* `import os`
* `import scipy.ndimage as nd`
* `import multiprocessing`
* `import numpy as np`

## python videoify911

Python PIL example: videoify911

```python
# imports and basic notebook setup
from cStringIO import StringIO
import numpy as np
import multiprocessing
import scipy.ndimage as nd
import os
import PIL.Image
from IPython.display import clear_output, Image, display
from google.protobuf import text_format
import math

import caffe
import cv2
import sys

invertdirboi = False
loadmodel = True
loadrule = False
model_path = 'placez/googlenet/'
defimage69='lqsantoz.jpg'
fldr='carzcarz911'
myconv69='@bottomup 1048 40'
stepSize='@up 0.0096 168 2.6'
invertStepSize=False
iterN=3
octaveN=2

def rgb_to_hsv(rgb):
    rgb = rgb.astype('float')
    hsv = np.zeros_like(rgb)
    hsv[..., 3:] = rgb[..., 3:]
    r, g, b = rgb[..., 0], rgb[..., 1], rgb[..., 2]
    maxc = np.max(rgb[..., :3], axis=-1)
    minc = np.min(rgb[..., :3], axis=-1)
    hsv[..., 2] = maxc
    mask = maxc != minc
    hsv[mask, 1] = (maxc - minc)[mask] / maxc[mask]
    rc = np.zeros_like(r)
    gc = np.zeros_like(g)
    bc = np.zeros_like(b)
    rc[mask] = (maxc - r)[mask] / (maxc - minc)[mask]
    gc[mask] = (maxc - g)[mask] / (maxc - minc)[mask]
    bc[mask] = (maxc - b)[mask] / (maxc - minc)[mask]
    hsv[..., 0] = np.select(
        [r == maxc, g == maxc], [bc - gc, 2.0 + rc - bc], default=4.0 + gc - rc)
    hsv[..., 0] = (hsv[..., 0] / 6.0) % 1.0
    return hsv

def hsv_to_rgb(hsv):
    rgb = np.empty_like(hsv)
    rgb[..., 3:] = hsv[..., 3:]
    h, s, v = hsv[..., 0], hsv[..., 1], hsv[..., 2]
    i = (h * 6.0).astype('uint8')
    f = (h * 6.0) - i
    p = v * (1.0 - s)
    q = v * (1.0 - s * f)
    t = v * (1.0 - s * (1.0 - f))
    i = i % 6
    conditions = [s == 0.0, i == 1, i == 2, i == 3, i == 4, i == 5]
    rgb[..., 0] = np.select(conditions, [v, q, p, p, t, v], default=v)
    rgb[..., 1] = np.select(conditions, [v, v, v, q, p, p], default=t)
    rgb[..., 2] = np.select(conditions, [v, p, t, v, v, q], default=p)
    return rgb.astype('uint8')

def savearray(a, filename, fmt='png'):
    a = np.uint8(np.clip(a, 0, 255))
    with open(filename, 'wb') as f:
        PIL.Image.fromarray(a).save(f, fmt)

if loadmodel:
    net_fn   = model_path + 'deploy.prototxt'
    param_fn = model_path + 'model.caffemodel'

    # Patching model to be able to compute gradients.
    # Note that you can also manually add "force_backward: true" line to "deploy.prototxt".
    model = caffe.io.caffe_pb2.NetParameter()
    text_format.Merge(open(net_fn).read(), model)
    model.force_backward = True
    def setnettm():
        global net
        net = caffe.Classifier('tmp.prototxt', param_fn,
                            mean = np.float32([109.4, 118.7, 106.0]), # ImageNet mean, training set dependent
                            channel_swap = (2,1,0)) # the reference model has channels in BGR order instead of RGB
    open('tmp.prototxt', 'w').write(str(model))
    setnettm()
     
    allnamez = [nametm for nametm in net._layer_names]
    convallnamez = []
    mahnamez = []
    if invertdirboi:
        if end911[0] == "bottomup":
            end911[0] = "topdown"
        else:
            end911[0] = "bottomup"
    #print(end911[0])
    for ilayer in xrange(len(net.layers)):
        if net.layers[ilayer].type == "Convolution":
            mahnamez.append(net.layers[ilayer])
            convallnamez.append(allnamez[ilayer])
     # a couple of utility functions for converting to and from Caffe's input image layout
    def preprocess(net, img):
        return np.float32(np.rollaxis(img, 2)[::-1]) - net.transformer.mean['data']
    def deprocess(net, img):
        return np.dstack((img + net.transformer.mean['data'])[::-1])
    def objective_L2(destination):
        destination.diff[:] = destination.data
    def make_step(net, end, objectiveGuide, step_size, jitter=128, clip=True):
        '''Basic gradient ascent step.'''
        src = net.blobs['data'] # input image is stored in Net's 'data' blob
        dst = net.blobs[end]
        
        if invertStepSize:
            step_size=-step_size
        
        ox, oy = np.random.randint(-jitter, jitter+1, 2)
        src.data[0] = np.roll(np.roll(src.data[0], ox, -1), oy, -2) # apply jitter shift
               
        net.forward(end=end)
        objectiveGuide(dst)
        net.backward(start=end)
        g = src.diff[0]
        # apply normalized ascent step to the input image
        # print(g)
        src.data[:] += step_size/np.abs(g).mean() * g
     
        src.data[0] = np.roll(np.roll(src.data[0], -ox, -1), -oy, -2) # unshift image
               
        if clip:
            bias = net.transformer.mean['data']
            src.data[:] = np.clip(src.data, -bias, 255-bias)    
            
    def deepdream(net, base_img, frame_i2, frame_i69, end, objectiveGuide, iter_n, octave_n, octave_scale=0.98, clip=True, **step_params):
        global invertdirboi
        global stepSize
        # prepare base images for all octaves
        if end.startswith("@"):
            if not end.endswith(" "):
                end=end+" "
                end69=end[1:len(end)-1]
            end911=end69.split(" ")
            if end911[0] == "bottomup":
                if not int(math.floor((max(frame_i-int(end911[2]),1))/int(end911[1]))) > len(net.layers)-2:
                    end = convallnamez[(len(mahnamez)-int(math.floor((max(frame_i-int(end911[2]),1))/int(end911[1]))))-1]
                else:
                    invertdirboi = not invertdirboi
                    end = convallnamez[0]
            if end911[0] == "topdown":
                if not int(math.floor((max(frame_i-int(end911[2]),1))/int(end911[1]))) > len(net.layers)-2:
                    end = convallnamez[int(math.floor((max(frame_i-int(end911[2]),1))/int(end911[1])))]
                else:
                    invertdirboi = not invertdirboi
                    end = convallnamez[len(mahnamez)-1]
        if stepSize.startswith("@"):
            if not end.endswith(" "):
                stepSize=stepSize+" "
                seppsize69=stepSize[1:len(stepSize)-1]
            stepsize911=seppsize69.split(" ")
            if stepsize911[0] == "up":
                if float(stepsize911[2])>frame_i2:
                    stepSize69=str(float(stepsize911[3])+float(stepsize911[1])*(frame_i2-float(stepsize911[2])))
                else:
                    stepSize69=str(float(stepsize911[3]))
            if stepsize911[0] == "down":
                if float(stepsize911[2])>frame_i2:
                    stepSize69=str(float(stepsize911[3])-float(stepsize911[1])*(frame_i2-float(stepsize911[2])))
                else:
                    stepSize69=str(float(stepsize911[3]))
        else:
            stepSize69=stepSize
        octaves = [preprocess(net, base_img)]
        for i in xrange(octave_n-1):
            octaves.append(nd.zoom(octaves[-1], (1, 1.0/octave_scale,1.0/octave_scale), order=1))
        src = net.blobs['data']
        detail = np.zeros_like(octaves[-1]) # allocate image for network-produced details
        for octave, octave_base in enumerate(octaves[::-1]):
            h, w = octave_base.shape[-2:]
            if octave > 0:
                # upscale details from the previous octave
                h1, w1 = detail.shape[-2:]
                detail = nd.zoom(detail, (1, 1.0*h/h1,1.0*w/w1), order=1)

            src.reshape(1,3,h,w) # resize the network's input image size
            src.data[0] = octave_base+detail
            for i in xrange(iter_n):
                make_step(net, end=end, clip=clip, objectiveGuide=objectiveGuide,step_size=float(stepSize69), **step_params)
               
                # visualization
                vis = deprocess(net, src.data[0])
                if not clip: # adjust image contrast if clipping is disabled
                    vis = vis*(255.0/np.percentile(vis, 99.98))
                #showarray(vis)
                print(frame_i2, frame_i69, octave, i, end, vis.shape)
                clear_output(wait=True)
               
            # extract details produced on the current octave
            detail = src.data[0]-octave_base
        # returning the resulting image
        return deprocess(net, src.data[0])
mylizt = []
s = 0.012 # scale coefficient
angletm = 0.24
if not os.path.isfile(fldr+"/1.png"):
    for myfile in os.listdir(fldr):
        if os.path.isfile(fldr+'/'+myfile):
            mylizt.append(myfile.replace(".png",""))
    if len(mylizt)>0:
        mylizt.sort(key=lambda item: (int(item.partition(' ')[0])
                                   if item[0].isdigit() else float('inf')))
        frame = np.float32(PIL.Image.open(fldr+'/'+mylizt[len(mylizt)-1]+'.png'))
        #frame = np.float32(PIL.Image.open('66.jpg'))
        h, w = frame.shape[:2]
        frame = nd.affine_transform(frame, [1-s,1-s,1], [h*s/2,w*s/2,0], order=1)
        frame_i = 1+int(filter(str.isdigit, mylizt[len(mylizt)-1]))
    if len(mylizt)==0:
        frame = np.float32(PIL.Image.open(defimage69))
        h, w = frame.shape[:2]
        frame_i = 1
else:
    mytmframe=1
    while os.path.isfile(fldr+"/"+str(mytmframe)+".png"):
        mytmframe+=1
    frame = np.float32(PIL.Image.open(fldr+'/'+str(mytmframe-1)+'.png'))
    #frame = np.float32(PIL.Image.open('66.jpg'))
    h, w = frame.shape[:2]
    frame = nd.affine_transform(frame, [1-s,1-s,1], [h*s/2,w*s/2,0], order=1)
    frame_i = mytmframe
# print(str(os.path.isfile('/output69/'+str(frame_i)+'.png')))
#while os.path.isfile('/output69/'+str(frame_i)+'.png')
#    frame_i+=1

def shift_hue(arr,hout):
    hsv=rgb_to_hsv(arr)
    hsv[...,0]+=hout
    rgb=hsv_to_rgb(hsv)
    return rgb

if loadmodel and loadrule:
#guide = deepdream(net, np.float32(PIL.Image.open('11.jpg')),-1,1,'inception_4b/3x3', objectiveGuide=objective_L2)
    guide = np.float32(PIL.Image.open('ncavelr.jpg'))
#guide = deepdream(net, guide, -69, 1, 'inception_4b/3x3', objectiveGuide=objective_L2)
#showarray(guide)

    print('Forwarding guide.')

    def reobjective():
        h, w = guide.shape[:2]
        src, dst = net.blobs['data'], net.blobs[end]
        src.reshape(1,3,h,w)
        src.data[0] = preprocess(net, guide)
        net.forward(end=end)
        guide_features = dst.data[0].copy()
        return guide_features
        
    end = 'inception_4b/3x3'
    mypool = multiprocessing.Pool()

    try:
        mahrezult=mypool.apply_async(reobjective)
        while not mahrezult.ready():
            pass
        guide_features=mahrezult.get()
    except (KeyboardInterrupt, SystemExit):
        sys.exit()

    def objective_guide(dst):
        x = dst.data[0].copy()
        y = guide_features
        ch = x.shape[0]
        x = x.reshape(ch,-1)
        y = y.reshape(ch,-1)
        A = x.T.dot(y)
        dst.diff[0].reshape(ch,-1)[:] = y[:,A.argmax(1)]

print('DeepDreaming...')

def zoom_at(img, x, y, zoom):
    w, h = img.size
    zoom2 = zoom * 2
    img = img.crop((x - w / zoom2, y - h / zoom2, 
                    x + w / zoom2, y + h / zoom2))
    return img.resize((w, h), PIL.Image.BICUBIC)

def resizeme(img, x, y):
    img = img.crop((0,0,min(img.size[0],x),min(img.size[1],y)))
    return img.resize((x, y), PIL.Image.BICUBIC)
#help(PIL.Image.Image.resize)

'''if not os.path.isfile("filezrc.txt"):
    mytexttm = open("filezrc.txt","w+")
else:
    mytexttm = open("filezrc.txt","a+")
'''
neuspechtm=0
def restartmetm():
    try:
        pmy = psutil.Process(os.getpid())
        for handler in pmy.get_open_files() + pmy.connections():
            os.close(handler.fd)
    except Exception, e:
        logging.error(e)

    pythontm = sys.executable
    os.execl(pythontm, pythontm, *sys.argv)
#origframe = np.float32(PIL.Image.open(defimage69))
#origframe = nd.affine_transform(origframe, [1+(s*4),1+(s*4),1], [h*s/2,w*s/2,0], order=1)
maxnum=420
if frame_i<maxnum+1:
    for i in xrange((maxnum+1)-frame_i):
        #frame = shift_hue(frame,2/360)
        if not os.path.isfile(fldr+"/"+str(frame_i)+".png"):
            try:
                #asyncframe = mypool.apply_async(deepdream, args=(net, frame, frame_i, 'inception_4b/3x3'), kwds={'objectiveGuide': objective_guide})
                #while not asyncframe.ready():
                #    pass  
                #for myiter in xrange(2):
                if loadmodel:
                    drframe = deepdream(net, frame, frame_i, 1, myconv69, objectiveGuide=objective_L2,iter_n=iterN,octave_n=octaveN)
                else:
                    drframe = frame
                #drframe = cv2.addWeighted(drframe,0.84,origframe,0.16,0)
            except (KeyboardInterrupt, SystemExit):
                sys.exit()
            #frame = asyncframe.get()
            #myimg=zoom_at(myimg,myimg.width/2,myimg.height/2,1+s)
            ##mytexttm.write("##boi "+str(frame_i)+"\n"+str(frame)+"\n")
            if not np.isnan(drframe.any()):
                frame=drframe
                neuspechtm=0
                myimg=PIL.Image.fromarray(np.uint8(frame))
                myimg.save(fldr+"/"+str(frame_i)+".png")
                if os.path.getsize(fldr+"/"+str(frame_i)+".png")/1024<8:
                    os.remove(fldr+"/"+str(frame_i)+".png")
                    restartmetm()
                #frame = nd.rotate(frame,angletm)
                frame = nd.affine_transform(frame, [1-s,1-s,1], [h*s/2,w*s/2,0], order=1)
                #frame = nd.shift(frame, np.array([1.4, 1.4, 0]))
                #myimgtm=PIL.Image.fromarray(np.uint8(frame))
                #myimgtm=resizeme(myimgtm,myimg.size[0],myimg.size[1])
                #frame = np.float32(myimgtm)
                frame_i += 1
            else:
                neuspechtm+=1
                if neuspechtm==3:
                    setnettm()
                if neuspechtm>3:
                    restartmetm()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
