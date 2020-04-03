---
title: matplotlib example ml helper (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'ml helper'

Functions in program: 
* `def linear_reg(df,formula):`
* `def feature_importance(rf,x,y,show_plot=False):`
* `def cv_model_selection(models,names,x,y,cv=10):`
* `def missing(df):`

Modules used in program: 
* `import seaborn as sns`
* `import pandas as pd`
* `import warnings`
* `import sklearn`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import numpy as np`

## python ml helper

Python matplotlib example: ml helper

```python


# 导入数据分析中常用的库并做初始化设置
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
import sklearn
import warnings
warnings.filterwarnings("ignore")
%matplotlib inline
import pandas as pd
import seaborn as sns
sns.set(style="white",color_codes=True)
plt.rcParams['figure.figsize'] = (15,9.27)
# Set the font set of the latex code to computer modern
matplotlib.rcParams['mathtext.fontset'] = "cm"


# 导入常用的分类库
from sklearn.linear_model import LogisticRegression #logistic regression
from sklearn.svm import SVC
from sklearn.ensemble import RandomForestClassifier,VotingClassifier
from sklearn.neighbors import KNeighborsClassifier #KNN
from sklearn.naive_bayes import GaussianNB #Naive bayes
from sklearn.tree import DecisionTreeClassifier #Decision Tree
from xgboost import XGBClassifier


# 导入模型选择相关的库
from sklearn.model_selection import cross_val_score,GridSearchCV

# 导入预处理相关的库
from sklearn.preprocessing import StandardScaler,MinMaxScaler


## ---------------------------------------------------------------------------------------------------------------


# 计算缺失值的数量及百分比并降序排列
def missing(df):
    res = pd.DataFrame()
    res['num'] = df.isnull().sum()
    res['pert'] = 100*res['num']/len(train)
    res.sort_values('pert',ascending=False,inplace=True)
    res = res[res.num>0]
    return res


# 使用交叉验证衡量不同模型的准确度及稳定性，从而为选择模型提供依据
def cv_model_selection(models,names,x,y,cv=10):
    means,stds = [],[]
    for model in models:
        model.fit(x,y)
        res_array = cross_val_score(model,x,y,cv=cv)
        means.append(res_array.mean())
        stds.append(res_array.std())
    res_df = pd.DataFrame({'model_name':names,'mean':means,'std':stds})
    res_df['mean/std'] = res_df['mean']/res_df['std']
    res_df = res_df.sort_values('mean/std',ascending=False)
    return res_df


# 使用随机森林算法判断特征的相对重要性
def feature_importance(rf,x,y,show_plot=False):
    rf.fit(x,y)
    res_df = pd.DataFrame({'feature':x.columns,'importance':rf.feature_importances_})
    res = res_df.sort_values('importance',ascending=False)
    res['cum_importance'] = res.importance.cumsum()
    if show_plot == True:
        plt.subplot(121)
        sns.barplot(res.feature,res.importance)
        plt.subplot(122)
        plt.plot(np.arange(1,res.shape[0]+1),res.cum_importance,linewidth=2)
    return(res)


# 使用statsmodels模块进行线性回归
def linear_reg(df,formula):
    import statsmodels.formula.api as sm
    result = sm.ols(formula=formula, data=df).fit()
    print(result.summary())

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
