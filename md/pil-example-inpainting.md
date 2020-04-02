---
title: pil example inpainting (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'inpainting'


Modules used in program: 
* `import PIL`
* `import numpy as np`
* `import cv2`

## python inpainting

Python pil example: inpainting

```python

!pip install opencv-python

!wget https://images.pexels.com/photos/860562/pexels-photo-860562.jpeg

import cv2
import numpy as np
import PIL
from IPython.display import display

#Resmi okuyalım
imge = cv2.imread("pexels-photo-860562.jpeg")

#Rengi BGR'den RGB'ye çevirelim
imge = cv2.cvtColor(imge, cv2.COLOR_BGR2RGB)

#Resmin boyutlarını normalize edelim
imge = cv2.resize(imge, (500, 500))

#Bastıralım
display(PIL.Image.fromarray(imge))

#Maskeyi oluşturalım, rastgele çizgiler ve bir kare çizip resmi bozalım
imge = imge.astype("uint8")
maske = np.ones((500, 500,3), dtype=imge.dtype)
maske[300:310,:,:] = 0
maske[400:405,:,:] = 0
maske[200:220,400:420,:] = 0
maske[:,100:103,:] = 0

#Bunu bitwise and gibi düşünebilirsiniz
imge = np.multiply(imge, maske)

# Bastıralım
display(PIL.Image.fromarray(imge))

#inpainting fonksiyonu bizim oluşturduğumuz maskenin tam tersini istiyor, o yüzden 1'den çıkarıyoruz bütün maskeyi
maske = 1-maske[:,:,0]

#CV'nin inpainting fonksiyonu
imge = cv2.inpaint(imge,maske,3,cv2.INPAINT_TELEA)

# Bastıralım
display(PIL.Image.fromarray(imge))



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
