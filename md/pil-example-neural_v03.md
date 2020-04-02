---
title: pil example neural v03 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'neural v03'

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

## python neural v03

Python pil example: neural v03

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
###########################################################################
# for debug
tracesOn = True

# Simple Neural Network
class Neural_Network(object):
  def __init__(self):
    #parameters
    self.inputSize = 100
    self.hiddenSize = 25
    self.outputSize = 2

    #weights
    np.random.seed()
    self.W1 = np.random.randn(self.inputSize, self.hiddenSize) # weight matrix from input to hidden layer
    self.W2 = np.random.randn(self.hiddenSize, self.outputSize) # weight matrix from hidden to output layer

  def forward(self, X):
    #forward propagation through our network
    self.z = np.dot(X, self.W1) # dot product of X (input) and first set of weights
    self.z2 = self.sigmoid(self.z) # activation function
    self.z3 = np.dot(self.z2, self.W2) # dot product of hidden layer (z2) and second set of weights
    o = self.sigmoid(self.z3) # final activation function
    return o

  def sigmoid(self, s):
    # activation function
    return 1/(1+np.exp(-s))

  def sigmoidPrime(self, s):
    #derivative of sigmoid
    return s * (1 - s)

  def backward(self, X, y, o):
    # backward propagate through the network
    self.o_error = y - o # error in output
    self.o_delta = self.o_error*self.sigmoidPrime(o) # applying derivative of sigmoid to error

    self.z2_error = self.o_delta.dot(self.W2.T) # z2 error: how much our hidden layer weights contributed to output error
    self.z2_delta = self.z2_error*self.sigmoidPrime(self.z2) # applying derivative of sigmoid to z2 error

    self.W1 += X.T.dot(self.z2_delta)*0.02 # adjusting first set (input --> hidden) weights
    self.W2 += self.z2.T.dot(self.o_delta)*0.02 # adjusting second set (hidden --> output) weights

  def train(self, X, y):
    o = self.forward(X)
    self.backward(X, y, o)

  def saveWeights(self):
    np.savetxt("w1.txt", self.W1, fmt="%s")
    np.savetxt("w2.txt", self.W2, fmt="%s")

  def loadWeights(self):
    self.W1 = np.loadtxt("w1.txt")
    self.W2 = np.loadtxt("w2.txt")

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
    self.bestLoss = {'val': 99, 'i': 0, 'W1': None, 'W2': None }
    for i in range(iterations):
      self.lossArray.append(np.mean(np.square(y - NN.forward(X))))
      if self.lossArray[i] <= self.bestLoss["val"]:
        self.bestLoss = {'val': min(self.lossArray), 'i': i, 'W1': np.copy(self.W1), 'W2': np.copy(self.W2) }
        self.train(X, y)
      if self.bestLoss["val"]<0.002:
        break
    finalLoss = self.bestLoss["val"]
    self.W1 = np.copy(self.bestLoss["W1"])
    self.W2 = np.copy(self.bestLoss["W2"])
    if tracesOn: # activate the plot by setting True
      plt.plot(self.lossArray)
      plt.plot(self.bestLoss["i"], self.bestLoss["val"], 'ro')
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
  if tracesOn:
    tempPil.resize((40,40)).show()
    
  return vector

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
  for dx in(-a, 0, a):
    for dy in(-a, 0, a):
      for th in(-th, 0, th):
        for k in (2,3,4,6,7,8):
          y.append(y0[k])
          X.append(getVector(mv.subviews[k], dx, dy, th))
  X = np.array(X, dtype=float)
  y = np.array(y, dtype=float)
  NN.trainAll(100)

def guess_action(sender):
  global NN, X, y
  if len(X) == 0:
    console.hud_alert('You need to do Steps 1 and 2 first.', 'error')
  else:
    p = getVector(mv.subviews[12])
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
