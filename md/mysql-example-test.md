---
title: mysql example test (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'test'

Functions in program: 
* `def make_model(config_path, checkpoint_path, cateogry2ind):`
* `def make_bert_feature_model(config_path, checkpoint_path):`

Modules used in program: 
* `import numpy as np`
* `import pickle`
* `import tensorflow.keras.backend as K`
* `import tensorflow as tf`
* `import os`

## python test

Python mysql example: test

```python
from tqdm import tqdm

tqdm.pandas()
import os

os.environ["TF_KERAS"] = "1"
from sklearn.model_selection import train_test_split
import tensorflow as tf
import tensorflow.keras.backend as K
from tensorflow.keras.layers import *
from tensorflow.keras.optimizers import Adam
from bert4keras.tokenizers import Tokenizer
from bert4keras.models import build_transformer_model
from bert4keras.snippets import sequence_padding, DataGenerator
from bert4keras.backend import set_gelu
from sklearn.model_selection import train_test_split
from tensorflow.keras.callbacks import TensorBoard, EarlyStopping, ModelCheckpoint, ReduceLROnPlateau
from time import time
import pickle
import numpy as np
from db.mysqlClient import mysql_connection

mysql_client = mysql_connection()
mysql_engine = mysql_client.engine
all_topics = mysql_client.select_query("select category_id, category from PolicyCategory;")
# 去除索引
# assign target to res
ind2cateogry = {i: v for i, v in all_topics}
cateogry2ind = {v: k for k, v in ind2cateogry.items()}
all_policy_types = ['决议', '决定', '公报', '公告', '通告', '意见', '通知', '通报', '报告', '请示', '批复', '议案', '函', '纪要', '其它']
ind2type = {i: v for i, v in enumerate(all_policy_types)}
type2ind = {v: k for k, v in ind2type.items()}


def make_bert_feature_model(config_path, checkpoint_path):
    bert = build_transformer_model(
        config_path=config_path,
        checkpoint_path=checkpoint_path,
        # model='albert',
        return_keras_model=False,
    )

    bert_extract_feature_model = tf.keras.models.Model(
        inputs=bert.model.input,
        outputs=[
            bert.model.layers[-1].output,
            bert.model.layers[-5].output,
            bert.model.layers[-9].output,
            bert.model.layers[-13].output,
        ]
    )
    output = bert_extract_feature_model.outputs
    output1 = Lambda(lambda x: x[:, 0], name='Pooler1')(output[0])
    output2 = Lambda(lambda x: x[:, 0], name='Pooler2')(output[1])
    output3 = Lambda(lambda x: x[:, 0], name='Pooler3')(output[2])
    output4 = Lambda(lambda x: x[:, 0], name='Pooler4')(output[3])
    output = tf.keras.layers.concatenate([output1, output2, output3, output4], axis=1)
    return tf.keras.models.Model(inputs=bert.model.input, outputs=output)


def make_model(config_path, checkpoint_path, cateogry2ind):
    # 加载预训练模型
    bert_feature_model = make_bert_feature_model(config_path, checkpoint_path)
    title_keywords_num = 5
    content_keywords_num = 10
    keywords_in = []
    tfidf_score_in = []
    for i in range(title_keywords_num):
        keywords_in.append([Input(shape=(None,), dtype='float', name=f'title_input_tokens_{i}'),
                            Input(shape=(None,), dtype='float', name=f'title_input_segment_{i}')])
        tfidf_score_in.append(Input(shape=(1,), dtype='float',
                                    name=f'title_input_weights_{i}'))

    for i in range(content_keywords_num):
        keywords_in.append([Input(shape=(None,), dtype='float', name=f'content_input_tokens_{i}'),
                            Input(shape=(None,), dtype='float', name=f'content_input_segment_{i}')])
        tfidf_score_in.append(Input(shape=(1,), dtype='float',
                                    name=f'content_input_weights_{i}'))

    features = []
    for i in range(title_keywords_num + content_keywords_num):
        feature = bert_feature_model(inputs=keywords_in[i])
        weighted_features = Multiply()([feature, tfidf_score_in[i]])
        features.append(weighted_features)

    total_features = Concatenate()(features)
    total_features_norm = LayerNormalization()(total_features)
    output = Dense(units=768, activation='relu')(total_features_norm)
    output = Dense(units=len(cateogry2ind),
                   activation='softmax')(output)
    token_inputs = []
    seg_inputs = []
    weight_inputs = []
    for i, v in enumerate(keywords_in):
        token_input, seg_input = v
        token_inputs.append(token_input)
        seg_inputs.append(seg_input)
        weight_inputs.append(tfidf_score_in[i])

    model = tf.keras.models.Model(
        inputs=token_inputs+seg_inputs+weight_inputs, outputs=output)
    model.summary()
    return model

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
