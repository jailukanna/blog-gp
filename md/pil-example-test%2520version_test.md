---
title: pil example test%2520version test (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'test%2520version test'


Modules used in program: 
* `import tkinter as tk`

## python test%2520version test

Python pil example: test%2520version test

```python
from PIL import Image
from torch.autograd import Variable
from torch.nn import functional
from model import *
from sklearn.externals import joblib
import tkinter as tk
from tkinter import filedialog

root = tk.Tk()
root.withdraw()
path = filedialog.askopenfilename()

rgb = RGB_model()
rgb.load_state_dict(torch.load('rgb.pkl'))
rgb.eval()
hsv = HSV_model()
hsv.load_state_dict(torch.load('hsv.pkl'))
hsv.eval()
ycbcr = YCbCr_model()
ycbcr.load_state_dict(torch.load('ycbcr.pkl'))
ycbcr.eval()
patch = patch_model()
patch.load_state_dict(torch.load('patch.pkl'))
patch.eval()

pil_img = Image.open(path).convert('RGB')
t = transforms.Compose([
    transforms.Resize((256, 256)),
    transforms.ToTensor()])
tr = transforms.Compose([
    transforms.Resize((256, 256)),
    transforms.RandomCrop(96),
    transforms.Resize((256, 256)),
    transforms.ToTensor()])
img = t(pil_img)
img.unsqueeze_(dim=0)
img = Variable(img)
imge = tr(pil_img)
imge.unsqueeze_(dim=0)
imge = Variable(imge)

rgb_out = rgb(img)
rgb_out = functional.softmax(rgb_out, dim=1)
rgb_out = rgb_out.detach().numpy()
hsv_out = hsv(img)
hsv_out = functional.softmax(hsv_out, dim=1)
hsv_out = hsv_out.detach().numpy()
ycbcr_out = ycbcr(img)
ycbcr_out = functional.softmax(ycbcr_out, dim=1)
ycbcr_out = ycbcr_out.detach().numpy()
patch_out = patch(imge)
patch_out = functional.softmax(patch_out, dim=1)
patch_out = patch_out.detach().numpy()


clf = joblib.load('svm.m')
test_X = [[rgb_out[0, 0], hsv_out[0, 0], ycbcr_out[0, 0], patch_out[0, 0]]]
print(int(clf.predict(test_X)[0]))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
