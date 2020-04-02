---
title: pil example neural v12 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'neural v12'

Functions in program: 
* `def clearAll_action(sender):`
* `def clear_action(sender):`
* `def guess_action(sender):`
* `def trainMore_action(sender):`
* `def train_action(sender):`
* `def showTrainData(i,n):`
* `def showTraining(i, n, V):`
* `def showLearning(i, n, v):`
* `def getColor(v):`
* `def trainNN():`
* `def updateLearninImage(img):`
* `def pil2ui(pil_image):`
* `def ui2pil(ui_img):`
* `def pil2np(img):`
* `def bbox(img):`
* `def snapshot(view):`
* `def vector2img(v, w=10, h=10):`
* `def getVector(img, dx=0, dy=0, theta=0, z=1.0):`
* `def zoom(img, z):`

Modules used in program: 
* `import objc_util`
* `import console, math, random, ImageMath`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import ui, io, gc`

## python neural v12

Python pil example: neural v12

```python
import ui, io, gc
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image as PILImage
from PIL import ImageChops as chops
from ImageColor import getrgb 
import console, math, random, ImageMath
import objc_util

###########################################################################
# history
# v01: 1/format output. 2/Landscape view.
# v02: 1/format output in % certainty. 2/ move templates by -a/0/+a in x and y, a =5
#      3/adjusted learning rate by x0.02 and learning epochs to 200
#       https://gist.github.com/d87a0833a64f0128a12c59547984ad2f
# v03: 1/put 2 neurons in output, to compare reliabilities 
#      2/show the bestloss (check bad learning)
#      3/random seed before weight initilizalization (to have another chance when learning is wrong)
#      4/added rotation by -th/0/+th in learning
#      5/learning is getting long: limit to 100 epoch, and stop when bestloss<0.002
#      https://gist.github.com/e373904d3ccba03803d80173f44b5eee
# v04: 1/ introducing a Layer class
#      2/ modified NN class to work with various layer numbers
#      https://gist.github.com/aea7738590793eefcd786be8657fa88b
# v05: 1/ made vector2img for cleaner image mngt
#      2/ change the learning order and the trace image creation
# v06: 1/ 3 channels: many changes to make code easier to manage, results easier to view
#      https://gist.github.com/3c9f5917224d8a70ea319af1df973c73
# v07: 1/ add images in ui
#      https://gist.github.com/549d071893cac00e84fcd1875d422d1a
# v08: 1/ added a white image and random image should return 0: to improve robustness
#      https://gist.github.com/d21c832208f33fe083b9200b29e1f073
# v09: 1/ cleaned up some code
#      2/ live color feedback during training on samples
#      https://gist.github.com/89684d9166746504bba88348240e26ff
# v10: 1/ added a [learn more] button to ... learn more.
#      https://gist.github.com/f7fc75b727c953e4dbb59c04f88acf74
# v11: big bug problem, not found, crashes => dead end.
# v12: clean up from v10, corrected a bug (thanks @cpv)
#      images are normalized before use => huge improvement!
###########################################################################

tracesOn = False # True for debug and illustration of learning process

# Simple Neuron layer
class Layer(object):
  def __init__(self, outputSize, inputLayer=False): 
    self.noise = 0.0
    self.outputSize = outputSize
    if inputLayer != False:
      self.hasInputLayer = True
      self.input = inputLayer
      self.inputSize = inputLayer.outputSize
      self.weights = np.random.randn(self.inputSize, self.outputSize) 
    else:
      self.hasInputLayer = False
    self.states = []
    
  def forward(self):
    #forward propagation through 1 network layer
    z = np.dot(self.input.states, self.weights) # dot product of input and set of weights
    z = z + self.noise * np.random.randn(self.outputSize) 
    self.states = self.sigmoid(z) # activation function
    
  def backward(self, err):
    #backward propagation through 1 network layer
    delta = err*self.sigmoidPrime( self.states ) # applying derivative of sigmoid to error
    newErr = delta.dot( self.weights.T ) # back-propagate error through the layer
    self.weights += self.input.states.T.dot(delta)*0.02 # adjusting weights
    return newErr
    
  def sigmoid(self, s):
    # activation function
    return 1/(1+np.exp(-s))

  def sigmoidPrime(self, s):
    #derivative of sigmoid
    return s * (1 - s)
  
# Simple Neural Network
class Neural_Network(object):
  def __init__(self):
    #create layers
    np.random.seed()
    self.layer = [] # now create layers from input to output:
    self.addLayer(100)
    self.addLayer(25)
    #self.addLayer(10)
    self.addLayer(3)
  
  def addLayer(self, nbr):
    n = len(self.layer)
    if n == 0:
      self.layer.append(Layer(nbr))
    else:
      self.layer.append(Layer(nbr, self.layer[n-1]))
    
  def forward(self, X):
    #forward propagation through our network
    n = len(self.layer)
    self.layer[0].states = X     # update input layer 
    for i in range(1,n):
      self.layer[i].forward()      # propagate through other layers
    return self.layer[n-1].states

  def backward(self, err):
    # backward propagate through the network
    n = len(self.layer)
    for i in range(1,n):
      err = self.layer[n-i].backward(err)

  def train(self, X, y):
    o = self.forward(X)
    self.backward(y - o)

  def predict(self, predict):
    o = self.forward(predict)
    #self.layer[1].weights2img(0,10).resize((30,30)).show()
    return o
  
  def trainAll(self, iterations= None):
    if iterations:
      for k in range(len(self.layer)): self.layer[k].noise = 0.1
      self.iterations = iterations
      self.count = 0
    self.yErr = y - self.forward(X)
    loss = np.mean(np.square(self.yErr))
    if self.count < self.iterations :
      self.train(X, y)
      self.count +=1
      showLearning(self.count, self.iterations, loss)
      showTraining(self.count, self.iterations, self.yErr)
      ui.delay(self.trainAll, 0.001) #enables the ui objects to update
    else:
      for k in range(len(self.layer)): self.layer[k].noise = 0.0
      console.hud_alert('Ready!')
      
  def showWeights(self):
    global BWlearningSetImage
    for i in range(len(self.layer)):
      temp = self.layer.allWeights2img(self,h)


###########################################################################
# The PathView class is responsible for tracking
# touches and drawing the current stroke.
# It is used by SketchView.

class PathView (ui.View):
  def __init__(self, frame):
    self.frame = frame
    self.flex = ''
    self.path = None
    self.action = None

  def touch_began(self, touch):
    x, y = touch.location
    self.path = ui.Path()
    self.path.line_width = 8.0
    self.path.line_join_style = ui.LINE_JOIN_ROUND
    self.path.line_cap_style = ui.LINE_CAP_ROUND
    self.path.move_to(x, y)

  def touch_moved(self, touch):
    x, y = touch.location
    self.path.line_to(x, y)
    self.set_needs_display()

  def touch_ended(self, touch):
    # Send the current path to the SketchView:
    if callable(self.action):
      self.action(self)
    # Clear the view (the path has now been rendered
    # into the SketchView's image view):
    self.path = None
    self.set_needs_display()

  def draw(self):
    if self.path:
      self.path.stroke()

###########################################################################
# The main SketchView contains a PathView for the current
# line and an ImageView for rendering completed strokes.

# We use a square canvas, so that the same image can be used in portrait and landscape orientation.
w, h = ui.get_screen_size()
canvas_size = max(w, h)
mv = ui.View(canvas_size, canvas_size)
mv.bg_color = 'white'
sketch = []  # global to handle the sketch views

class SketchView (ui.View):
  def __init__(self, x, y, width=200, height=200):
    global sketch,mv
    # the sketch region
    self.bg_color = 'lightgrey'
    iv = ui.ImageView(frame=(0, 0, width, height)) #, border_width=1, border_color='black')
    pv = PathView(iv.bounds)
    pv.action = self.path_action
    self.add_subview(iv)
    self.add_subview(pv)
    self.image_view = iv
    self.bounds = iv.bounds
    self.x = x
    self.y = y
    mv.add_subview(self)
    
    # some info
    lb = ui.Label()
    self.text='sample ' + str(len(sketch))
    lb.text=self.text
    lb.flex = ''
    lb.x = x+50
    lb.y = y+205
    lb.widht = 100
    lb.height = 20
    lb.alignment = ui.ALIGN_CENTER
    mv.add_subview(lb)
    self.label = lb
    
  
  def getImg(self):
    img = ui2pil(snapshot(self.subviews[0]))
    _,_,_,img = img.split()
    x, y, w, h = bbox(img)
    img = img.offset(-x,-y)
    s = max(w,h)
    img = img.crop((0,0,s,s))
    img = img.offset( int((s-w)/2), int((s-h)/2))
    img = img.resize((100, 100),PILImage.BILINEAR)
    img1 = PILImage.new('L',[150,150])
    img1.paste(img,(25,25))
    img = img1.resize((100, 100),PILImage.BILINEAR)
    #img = ImageMath.eval('a*2', a=img)
    img = img.resize((50, 50),PILImage.BILINEAR)
    #img = ImageMath.eval('a*2', a=img)
    img = img.resize((25, 25),PILImage.BILINEAR)
    #img = ImageMath.eval('a*2', a=img)
    #img.resize((100,100)).show()
    self.img = img
    return self.img.copy()
  
  def resetImage(self):
    self.image_view.image = None
    self.sImage = None
  
  def resetText(self,newText=None):
    if newText != None:
      self.text = newText
    self.label.text = self.text
    self.label.bg_color = 'white'
    
  def showResult(self,v):
    txt = '{:d}%'.format(int(100*float(v)))
    self.label.text = txt
    if   v > 0.90: c = 'lightgreen'
    elif v > 0.75: c = 'lightblue'
    elif v > 0.50: c = 'yellow'
    elif v > 0.25: c = 'orange'
    else : c = 'red'
    self.label.bg_color = c

  def path_action(self, sender):
    path = sender.path
    old_img = self.image_view.image
    width, height = self.image_view.width, self.image_view.height
    with ui.ImageContext(width, height) as ctx:
      if old_img:
        old_img.draw()
      path.stroke()
      self.image_view.image = ctx.get_image()

###########################################################################
# Various helper functions



def zoom(img, z):
  if z==1.0:
    return img 
  w0 = img.width
  h0 = img.height
  w = int( w0 * z )
  h = int( h0 * z )
  img1 = img.resize((w,h),PILImage.BILINEAR)
  if z<1.0:
    img = img.copy()
    x = int((w0-w)/2)
    y = int((h0-h)/2)
    img.paste(img1,(x,y))
  if z>1.0:
    x = int((w-w0)/2)
    y = int((h-h0)/2)
    img = img1.crop((x,y,x+w0-1,y+h0-1))
    img = img.copy()
  return img

def getVector(img, dx=0, dy=0, theta=0, z=1.0):
  pil_image = img.copy()
  pil_image = pil_image.rotate(theta,PILImage.BILINEAR)
  pil_image = chops.offset(pil_image, dx, dy)
  pil_image = zoom(pil_image, z)
  pil_image = pil_image.resize((10, 10),PILImage.BILINEAR)
  vector = []
  for x in range(0, 10):
    for y in range(0, 10):
      v = pil_image.getpixel((x,y))
      vector.append( (v>30)*1.0 )
  return vector

def vector2img(v, w=10, h=10):
  maxi = max(v)
  mini = min(v)
  r = maxi-mini
  if r == 0: r = 1
  tempPil = PILImage.new('L',[w,h])
  k=0
  for x1 in range(w):
    for y1 in range(h):
      if k<len(v):
        val = v[k]
        val = 255 - (val-mini)/r*250
      else:
        val = 255
      tempPil.putpixel([x1,y1],val)
      k+=1
  return tempPil
  
def snapshot(view):
  with ui.ImageContext(view.width, view.height) as ctx:
    view.draw_snapshot()
    return ctx.get_image()

def bbox(img):
  img = pil2np(img)
  rows = np.any(img, axis=1)
  cols = np.any(img, axis=0)
  rmin, rmax = np.where(rows)[0][[0, -1]]
  cmin, cmax = np.where(cols)[0][[0, -1]]
  x, y, w, h = cmin, rmin, cmax-cmin+1, rmax-rmin+1
  return x, y, w, h

def pil2np(img):
  return np.array(img.getdata(), np.uint8).reshape(img.size[1], img.size[0])

def ui2pil(ui_img):
  return PILImage.open(io.BytesIO(ui_img.to_png()))

def pil2ui(pil_image):
  buffer = io.BytesIO()
  pil_image.save(buffer, format='PNG')
  return ui.Image.from_data(buffer.getvalue())

class prepareTrainSet():
  def __init__(self):
    global X, y, sketch
    X = []
    y = []
    self.y0 = [ [1,0,0], 
                [0,1,0],
                [0,0,1]]
    images = []
    for i in range(len(sketch)):
      images.append( sketch[i].getImg() )
    self.images = images
    self.temp = None
    self.run(3*150)
  
  def run(self, n=None):
    global X, y, pts, NN
    if n!=None:
      self.n = n
      self.count = 0
    else:
      self.count +=1
    count = self.count
    if count < self.n :
      if count%10==1:
        showTrainData( count+1, self.n )
      k = count % len(self.images)
      y.append(self.y0[k])
      img = self.images[k]
      a = 2
      theta = 20
      dx,dy,z = 0,0,1.0
      dx = int(random.uniform(-a,a)+0.5)
      dy = int(random.uniform(-a,a)+0.5)
      th = random.uniform(-theta,theta)
      #z = random.uniform(0.9, 1.2)
      zy = random.uniform(0.9, 1.2)
      v = getVector(img, dx, dy, th, z)
      X.append(v)
      if True or tracesOn:
        nb = 30
        if self.temp == None:
          w = (10+1)*nb
          h = (10+1)*math.ceil(n/nb)-1
          temp = PILImage.new('L',[w,h],250)
          self.temp = temp
          
        x1 = int(math.fmod(count,nb))
        y1 = int(math.floor(count/nb))
        img = vector2img(v)
        self.temp.paste(img,(x1*11,y1*11))
        if count % nb == nb-1:
          updateLearninImage(self.temp)
      ui.delay(self.run, 0.001)
    else:
      self.count = 0
      X = np.array(X, dtype=float)
      y = np.array(y, dtype=float)
      updateLearninImage(self.temp)
      
      NN.trainAll(200)
      pts = None
      
def updateLearninImage(img):
  w1,h1 = int(learningSetImage.width), int(learningSetImage.height)
  temp = img.resize((w1,h1))
  learningSetImage.image = pil2ui(temp)
  global BWlearningSetImage
  BWlearningSetImage = temp #.convert('RGB')

def trainNN():
  global pts,NN
  pts = prepareTrainSet()

def getColor(v):
  if   v > 0.1: c = 'red'
  elif v > 0.02: c = 'orange'
  elif v > 0.005: c = 'yellow'
  elif v > 0.001: c = 'lightblue'
  else : c = 'lightgreen'
  return c

def showLearning(i, n, v):
  if (i % 10 == 1) or (i == n): 
    trainInfo.bg_color = getColor(v)
    txt = 'Loss {:d} : {:5.2f}%'.format(i+1, int(10000*float(v))/100)
    trainInfo.text = txt
  else:
    pass

def showTraining(i, n, V):
  if (i % 20 == 1) or (i == n):
    w = 30
    h = math.ceil(len(V)/w)
    temp = PILImage.new('RGB',[w,h],'white')
    global BWlearningSetImage
    BWlearningSetImage = BWlearningSetImage.convert('RGB')
    for k in range(len(V)):
      x = int(math.fmod(k,w))
      y = int(math.floor(k/w))
      c = getrgb(getColor(np.mean(np.square(V[k]))))
      temp.putpixel((x,y),c)
    #temp.show()
    w1,h1 = int(learningSetImage.width), int(learningSetImage.height)
    temp = temp.resize((w1,h1))
    temp = chops.multiply(temp, BWlearningSetImage)
    learningSetImage.image = pil2ui(temp)
  else:
    pass
  
def showTrainData(i,n):
  trainInfo.bg_color = 'white'
  txt = 'Preparing {:d} / {:d}'.format(i, n)
  trainInfo.text = txt

###########################################################################
# action functions

def train_action(sender):
  ui.delay(trainNN,0.2)
  
def trainMore_action(sender):
  global NN
  NN.trainAll(200)

def guess_action(sender):
  global NN, X, y
  if len(X) == 0:
    console.hud_alert('You need to do Steps 1 and 2 first.', 'error')
  else:
    p = getVector(newSketch.getImg())
    if tracesOn:
      img = vector2img(p)
      zoom = 3
      img.resize((10*zoom,10*zoom)).show()
    p = np.array(p, dtype=float)
    result = NN.predict(p)
    #console.hud_alert('done')
    for i in range(len(sketch)):
      sketch[i].showResult(result[i])

def clear_action(sender):
  newSketch.resetImage()
  for sv in sketch:
      sv.resetText()

def clearAll_action(sender):
  for sv in sketch:
    sv.resetImage()
    sv.resetText()
  newSketch.resetImage()
  showLearning(0,0,1)
  

##############################################

NN = Neural_Network()

clearAll_button = ui.ButtonItem()
clearAll_button.title = 'Reset !!'
clearAll_button.tint_color = 'red'
clearAll_button.action = clearAll_action
mv.right_button_items = [clearAll_button]

lb = ui.Label()
lb.text='First, prepare the data:'
lb.flex = 'W'
lb.x = 290
lb.y = 0
mv.add_subview(lb)

lb = ui.Label()
lb.text='Draw 3 different images (ex: A, B, C)'
lb.flex = 'W'
lb.alignment = ui.ALIGN_CENTER
lb.x = -150
lb.y = 20
mv.add_subview(lb)

sv = SketchView( 30, 100)
sketch.append(sv)
sv = SketchView(260, 100)
sketch.append(sv)
sv = SketchView(490, 100)
sketch.append(sv)
#sv = SketchView( 30, 340)
#sv = SketchView(260, 340)
#sv = SketchView(490, 340)

iv = ui.ImageView(frame=(30, 350, 660, 300), border_width=1, border_color='black')
learningSetImage = iv
mv.add_subview(iv)

lb = ui.Label()
lb.text='Now, Train the Model'
lb.flex = 'W'
lb.x = 690+50
lb.y = 50+50
lb.height = 20
mv.add_subview(lb)

train_button = ui.Button(frame = (800, 80+50, 80, 32))
train_button.border_width = 2
train_button.corner_radius = 4
train_button.title = '1/ Train'
train_button.action = train_action
mv.add_subview(train_button)

train_more = ui.Button(frame = (800, 80+50+50, 80, 32))
train_more.border_width = 2
train_more.corner_radius = 4
train_more.title = 'Train more'
train_more.action = trainMore_action
mv.add_subview(train_more)

trainInfo = ui.Label()
lb = trainInfo
lb.text='0%'
lb.flex = ''
lb.x = 750
lb.y = 120+50+50+10
lb.height = 20
lb.width = 200
lb.alignment = ui.ALIGN_CENTER
mv.add_subview(trainInfo)
showLearning(0, 0, 1.0)

lb = ui.Label()
lb.text='OK now lets see if it can Guess right'
lb.flex = 'w'
lb.x = 700
lb.y = 200+50
mv.add_subview(lb)

sv = SketchView(740, 280+50)
#sketch = sketch[:-1] # this last view is not part of the example set => remove it
sv.resetText('')
mv.add_subview(sv)
newSketch = sv

guess_button = ui.Button(frame = (750, 530+50, 80, 32))
guess_button.border_width = 2
guess_button.corner_radius = 4
guess_button.title = '2/ Guess'
guess_button.action = guess_action
mv.add_subview(guess_button)

clear_button = ui.Button(frame = (850, 530+50, 80, 32))
clear_button.border_width = 2
clear_button.corner_radius = 4
clear_button.title = 'Clear'
clear_button.action = clear_action
mv.add_subview(clear_button)

mv.name = 'Image Recognition'
mv.present('full_screen', orientations='landscape')



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
