---
title: matplotlib example wavelets (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'wavelets'


Modules used in program: 
* `import pywt`
* `import os`
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`
* `import matplotlib.dates as mdates`
* `import numpy as np`
* `import pandas as pd`

## python wavelets

Python matplotlib example: wavelets

```python
# Como implementar o pywt
import pandas as pd
import numpy as np
from matplotlib.finance import candlestick_ohlc
import matplotlib.dates as mdates
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
import os
import pywt
#
diretorio= os.getcwd()
diretorio
data=pd.read_csv(diretorio+"/btc.csv",sep=";",index_col="datetime")
print(data.shape)
data.tail(3)
(ca, cd)= pywt.dwt(data.close, 'haar')
cat = pywt.threshold(ca, np.std(ca), mode="soft")
cdt = pywt.threshold(cd, np.std(cd), mode="soft")
tx = pywt.idwt(cat, cdt, "haar")
data.close.shape, ca.shape, cd.shape, cat.shape, cdt.shape, tx.shape
plt.figure(figsize=(14, 4))
plt.subplot(1, 3, 1)
plt.plot(data.close, '-',lw=1)
plt.title('signal')
plt.subplot(1,3,2)
plt.plot(ca,'-',lw=1)
plt.title('noise')
plt.subplot(1,3,3)
plt.plot(tx,'--',lw=1)
plt.title('denoised')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
