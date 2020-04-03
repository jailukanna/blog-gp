---
title: matplotlib example matplotlib PDF ja (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib PDF ja'


Modules used in program: 
* `import matplotlib.font_manager`
* `import matplotlib.pyplot as plt`
* `import sys`

## python matplotlib PDF ja

Python matplotlib example: matplotlib PDF ja

```python
# -*- coding: utf-8 -*-
"""
Mac OSXのmatplotlibで日本語フォント(Osaka)を使うサンプル
python2.7(anaconda-2.1.0)
cf. http://matplotlib.org/users/customizing.html
"""
# デフォルトの文字コードを変更する．
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
# import
import matplotlib.pyplot as plt
import matplotlib.font_manager
from matplotlib.font_manager import FontProperties
from matplotlib.backends.backend_pdf import PdfPages

# for Mac
font_path = '/Library/Fonts/Osaka.ttf'

font_prop = matplotlib.font_manager.FontProperties(fname=font_path)
matplotlib.rcParams['font.family'] = font_prop.get_name()
# pdfのフォントをTrueTypeに変更
matplotlib.rcParams['pdf.fonttype'] = 42
# defaultのdpi=100から変更
matplotlib.rcParams['savefig.dpi'] = 300
# 数式（Latex)のフォントを変更
matplotlib.rcParams['mathtext.default'] = 'regular'

pdf = PdfPages("test.pdf")

plt.figure()
plt.plot(range(10))
plt.title(u'日本語のテスト')
plt.xlabel(u'Latexのテスト' + r'  $y=a x^{2} \alpha \beta $')
plt.ylabel(u'y軸 (m)')

plt.savefig('test.png')
pdf.savefig()
pdf.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
