---
title: matplotlib example lib-nlp-keras (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'lib-nlp-keras'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python lib-nlp-keras

Python matplotlib example: lib-nlp-keras

```python
%matplotlib inline

import numpy as np
import matplotlib.pyplot as plt

from keras.preprocessing.text import Tokenizer
from keras.preprocessing.sequence import pad_sequences
from keras.models import Model
from keras.layers import GRU, Input, Dense, TimeDistributed, Activation, RepeatVector, Bidirectional
from keras.layers import SimpleRNN, LSTM
from keras.models import Sequential
from keras.layers.embeddings import Embedding
from keras.optimizers import Adam
from keras.losses import sparse_categorical_crossentropy

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
