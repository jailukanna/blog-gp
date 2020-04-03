---
title: matplotlib example week2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'week2'

Functions in program: 
* `def train(X_train_bow, y_train):`
* `def text_to_bow(text, voc):`
* `def bow(texts_train):`
* `def read_data():`

Modules used in program: 
* `import pandas as pd`
* `import matplotlib`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python week2

Python matplotlib example: week2

```python
# https://github.com/yandexdataschool/nlp_course/blob/master/week02_classification/homework_part1.ipynb

import numpy as np
import matplotlib.pyplot as plt
import matplotlib
# import tkinter
# matplotlib.use('TkAgg')
# %matplotlib inline


# import numpy as np
# import matplotlib.pyplot as plt
# %matplotlib inline


import pandas as pd

# 1. read data
# 2. data to train,test set


def read_data():
    data = pd.read_csv("./data/comments.tsv", sep='\t')

    texts = data['comment_text'].values
    target = data['should_ban'].values
    print(data[50::200])

    from sklearn.model_selection import train_test_split
    texts_train, texts_test, y_train, y_test = train_test_split(
        texts, target, test_size=0.5, random_state=42)

    from nltk.tokenize import TweetTokenizer
    tokenizer = TweetTokenizer()
    def preprocess(text): return ' '.join(tokenizer.tokenize(text.lower()))

    text = 'How to be a grown-up at work: replace "fuck you" with "Ok, great!".'
    print("before:", text,)
    print("after:", preprocess(text),)

    _preprocess = np.vectorize(preprocess)

    texts_train = _preprocess(texts_train)
    texts_test = _preprocess(texts_test)

    assert texts_train[5] == 'who cares anymore . they attack with impunity .'
    assert texts_test[89] == 'hey todds ! quick q ? why are you so gay'
    assert len(texts_test) == len(y_test)

    return texts_train, texts_test, y_train, y_test


def bow(texts_train):
    # task: find up to k most frequent tokens in texts_train,
    # sort them by number of occurences (highest first)
    k = 10000

    wc = {}
    for text in texts_train.tolist():
        # from nltk.tokenize import TweetTokenizer # use this?
        tokens = text.split() # why split(' ') different with split('')?
        for token in tokens:
            if token in wc:
                wc[token] = wc[token]+1
            else:
                wc[token] = 1

    voc = []
    for term in wc:
        voc.append([term, wc[term]])
    voc.sort(key=lambda x: x[1], reverse=True)

    bow_voc = []
    for i in range(min(k, len(voc))):
        bow_voc.append(voc[i][0])
    print('example features:', sorted(bow_voc)[::100])

    return bow_voc


def text_to_bow(text, voc):
    """ convert text string to an array of token counts. Use bow_vocabulary. """
    tokens = text.split(' ')
    # print('voc len:', len(voc))

    voc_map = {}
    for i, term in enumerate(voc, start=0):
        voc_map[term] = i

    bow = [0]*len(voc)
    for token in tokens:
        if token in voc_map:
            bow[voc_map[token]] = bow[voc_map[token]]+1
        else:
            # print('unrecognized token: ', token)
            pass

    return np.array(bow, 'float32')

def train(X_train_bow, y_train):
    from sklearn.linear_model import LogisticRegression
    bow_model = LogisticRegression().fit(X_train_bow, y_train)

    from sklearn.metrics import roc_auc_score, roc_curve
    for name, X, y, model in [
        ('train', X_train_bow, y_train, bow_model),
        ('test ', X_test_bow, y_test, bow_model)
    ]:
        proba = model.predict_proba(X)[:, 1]
        auc = roc_auc_score(y, proba)
        plt.plot(*roc_curve(y, proba)[:2], label='%s AUC=%.4f' % (name, auc))

    plt.plot([0, 1], [0, 1], '--', color='black',)
    plt.legend(fontsize='large')
    plt.grid()
    plt.show()

if __name__ == '__main__':
    texts_train, texts_test, y_train, y_test = read_data()

    bow_vocabulary = bow(texts_train)

    def bow_converter(text): return text_to_bow(text, bow_vocabulary)

    print('bow_voc len', len(bow_vocabulary))

    X_train_bow = np.stack(list(map(bow_converter, texts_train)))
    X_test_bow = np.stack(list(map(bow_converter, texts_test)))

    train(X_train_bow, y_train)

    # test 
    # k = 10000
    # k_max = len(set(' '.join(texts_train).split()))
    # print('min ', min(k, k_max))
    # assert X_train_bow.shape == (len(texts_train), min(k, k_max))
    # assert X_test_bow.shape == (len(texts_test), min(k, k_max))
    # assert np.all(X_train_bow[5:10].sum(-1) ==
    #               np.array([len(s.split()) for s in texts_train[5:10]]))
    # assert len(bow_vocabulary) <= min(k, k_max)
    # assert X_train_bow[6, bow_vocabulary.index(
    #     '.')] == texts_train[6].split().count('.')




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
