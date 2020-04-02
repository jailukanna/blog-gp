---
title: PIL example neural v04 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'neural v04'

Functions in program: 
* `def clearAll_action(sender):`
* `def clear_action(sender):`
* `def guess_action(sender):`
* `def train_action(sender):`
* `def pil2ui(pil_image):`
* `def ui2pil(ui_img):`
* `def snapshot(view):`
* `def getVector(v,dx=0,dy=0, theta=0):`

Modules used in program: 
* `import console`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import ui, io`

## python neural v04

Python PIL example: neural v04

```python
import ui, io
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image as PILImage
from PIL import ImageChops as chops
import console

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
###########################################################################
# for debug
tracesOn = True

# Simple Neuron layer
class Layer(object):
  def __init__(self, outputSize, inputLayer=False): 
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
    self.addLayer(10)
    self.addLayer(2)
  
  def addLayer(self, nbr):
    n = len(self.layer)
    if n == 0:
      self.layer.append(Layer(nbr))
    else:
      self.layer.append(Layer(nbr, self.layer[n-1]))
    
  def forward(self, X):
    #forward propagation through our network
    n = len(self.layer)
    self.layer[0].states = X     # update input layer = layer0
    for i in range(1,n):
      self.layer[i].forward()      # propagate through layers
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
    decision = ''
    if o[0]>o[1]:
      decision = 'Top'
    else:
      decision = 'Bot'
    reliability0 = 'Top: {:d}%'.format(int(100*float(o[0])))
    reliability1 = 'Bot: {:d}%'.format(int(100*float(o[1])))
    output = decision + ' (' + reliability0 + ', ' + reliability1 + ')'
    if tracesOn:
      print(output)
    return output

  def trainAll(self, iterations):
    self.lossArray = []
    for i in range(iterations):
      self.lossArray.append(np.mean(np.square(y - NN.forward(X))))
      self.train(X, y)
      finalLoss = self.lossArray[i]
      if finalLoss<0.002:
        break

    if tracesOn: # activate the plot by setting True
      plt.plot(self.lossArray)
      plt.grid(1)
      plt.xlabel('Iterations')
      plt.ylabel('Cost')
      plt.show()
      finalLoss = 'final loss: {:5.3f}%'.format(finalLoss)
      console.hud_alert(finalLoss)

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

class SketchView (ui.View):
  def __init__(self, width, height):
    self.bg_color = 'lightgrey'
    iv = ui.ImageView(frame=(0, 0, width, height)) #, border_width=1, border_color='black')
    pv = PathView(iv.bounds)
    pv.action = self.path_action
    self.add_subview(iv)
    self.add_subview(pv)
    self.image_view = iv
    self.bounds = iv.bounds

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

def getVector(v,dx=0,dy=0, theta=0):
  pil_image = ui2pil(snapshot(v.subviews[0]))
#       pil_image.show()
  pil_image = pil_image.resize((200,200))
  pil_image = chops.offset(pil_image, dx, dy)
  pil_image = pil_image.rotate(theta)
#       print(pil_image.size)
  w, h = int(v.image_view.width), int(v.image_view.height)
#       print(w,h)
  px = 20
  p = int(w / px)
  xStep = int(w / p)
  yStep = int(h / p)
  tempPil = PILImage.new('RGB',[10,10],'lightgrey')
  vector = []
  for x in range(0, w, xStep):
    for y in range(0, h, yStep):
      crop_area = (x, y, xStep + x, yStep + y)
      cropped_pil = pil_image.crop(crop_area)
#                       print(crop_area)
#                       cropped_pil.show()
      crop_arr = cropped_pil.load()

      nonEmptyPixelsCount = 0
      for x1 in range(xStep):
        for y1 in range(yStep):
          isEmpty = (crop_arr[x1,y1][3] == 0)
#                                       print(x1, y1, crop_arr[x1,y1], isEmpty)
          if not isEmpty:
            nonEmptyPixelsCount += 1
      if nonEmptyPixelsCount > 0:
         nonEmptyPixelsCount = 1
         tempPil.putpixel([int(x/xStep),int(y/yStep)],(0,0,0))
      vector.append(nonEmptyPixelsCount)
      
#                       print(len(vector))
  #maxi = max(max(vector),1)
  #vector = [x / maxi for x in vector]
    
  return vector, tempPil

def snapshot(view):
  with ui.ImageContext(view.width, view.height) as ctx:
    view.draw_snapshot()
    return ctx.get_image()

def ui2pil(ui_img):
  return PILImage.open(io.BytesIO(ui_img.to_png()))

def pil2ui(pil_image):
  buffer = io.BytesIO()
  pil_image.save(buffer, format='PNG')
  return ui.Image.from_data(buffer.getvalue())

def train_action(sender):
  global NN, X, y
  X = []
  y = []
  y0 = [[],[],[1,0], [1,0], [1,0], [], [0,1], [0,1], [0,1]]
  a = 5
  th = 5
  w = (10+1)*27-1
  for k in (2,3,4,6,7,8):
    temp = PILImage.new('RGB',[w,10],'lightgrey')
    j= 0
    for dx in(-a, 0, a):
      for dy in(-a, 0, a):
        for th in(-th, 0, th):
            y.append(y0[k])
            v,img = getVector(mv.subviews[k], dx, dy, th)
            X.append(v)
            if tracesOn:
              temp.paste(img,(j*11,0))
              j+=1
    if tracesOn:
      zoom = 3
      temp.resize((w*zoom,10*zoom)).show()
  X = np.array(X, dtype=float)
  y = np.array(y, dtype=float)
  NN.trainAll(100)

def guess_action(sender):
  global NN, X, y
  if len(X) == 0:
    console.hud_alert('You need to do Steps 1 and 2 first.', 'error')
  else:
    p,img = getVector(mv.subviews[12])
    if tracesOn:
      zoom = 3
      img.resize((10*zoom,10*zoom)).show()
    p = np.array(p, dtype=float)
    console.hud_alert(NN.predict(p))
    

def clear_action(sender):
  mv.subviews[12].image_view.image = None

def clearAll_action(sender):
  mv.subviews[2].image_view.image = None
  mv.subviews[3].image_view.image = None
  mv.subviews[4].image_view.image = None
  mv.subviews[6].image_view.image = None
  mv.subviews[7].image_view.image = None
  mv.subviews[8].image_view.image = None
  mv.subviews[12].image_view.image = None

##############################################



NN = Neural_Network()

# We use a square canvas, so that the same image can be used in portrait and landscape orientation.
w, h = ui.get_screen_size()
canvas_size = max(w, h)
box_size = 200

X = []
y = []

mv = ui.View(canvas_size, canvas_size)
mv.bg_color = 'white'

clearAll_button = ui.ButtonItem()
clearAll_button.title = 'Reset !!'
clearAll_button.tint_color = 'red'
clearAll_button.action = clearAll_action
mv.right_button_items = [clearAll_button]

lb = ui.Label()
lb.text='First, prepare the data'
lb.flex = 'W'
lb.x = 290
lb.y = 10
mv.add_subview(lb)

lb = ui.Label()
lb.text='Draw 3 positive images'
lb.flex = 'W'
lb.x = 280
lb.y = 30
mv.add_subview(lb)

sv = SketchView(box_size, box_size)
sv.x = 30
sv.y = 100
mv.add_subview(sv)

sv = SketchView(box_size, box_size)
sv.x = 260
sv.y = 100
mv.add_subview(sv)

sv = SketchView(box_size, box_size)
sv.x = 490
sv.y = 100
mv.add_subview(sv)

lb = ui.Label()
lb.text='Then draw 3 negative images'
lb.flex = 'W'
lb.x = 270
lb.y = 270
mv.add_subview(lb)

sv = SketchView(box_size, box_size)
sv.x = 30
sv.y = 340
mv.add_subview(sv)

sv = SketchView(box_size, box_size)
sv.x = 260
sv.y = 340
mv.add_subview(sv)

sv = SketchView(box_size, box_size)
sv.x = 490
sv.y = 340
mv.add_subview(sv)

lb = ui.Label()
lb.text='Once you have images above, Train the Model'
lb.flex = 'W'
lb.x = 200
lb.y = 520
mv.add_subview(lb)

train_button = ui.Button(frame = (330, 590, 80, 32))
train_button.border_width = 2
train_button.corner_radius = 4
train_button.title = 'Train'
train_button.action = train_action
mv.add_subview(train_button)

lb = ui.Label()
lb.text='OK now lets see if it can Guess right'
lb.flex = 'W'
lb.x = 700
lb.y = 120
mv.add_subview(lb)

sv = SketchView(box_size, box_size)
sv.x = 740
sv.y = 200
mv.add_subview(sv)

guess_button = ui.Button(frame = (750, 450, 80, 32))
guess_button.border_width = 2
guess_button.corner_radius = 4
guess_button.title = 'Guess'
guess_button.action = guess_action
mv.add_subview(guess_button)

clear_button = ui.Button(frame = (850, 450, 80, 32))
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
