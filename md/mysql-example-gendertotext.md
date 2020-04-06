---
title: mysql example gendertotext (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'gendertotext'

Functions in program: 
* `def features(name):`

Modules used in program: 
* `import urllib `
* `import re`
* `import spacy`
* `import spacy `
* `import nltk `
* `import string`
* `import pickle`
* `import csv`
* `import numpy as np`
* `import pandas as pd`

## python gendertotext

Python mysql example: gendertotext

```python
import pandas as pd
import numpy as np
from nltk.tokenize import word_tokenize
from nltk import pos_tag
from nltk.corpus import stopwords
from nltk.stem import WordNetLemmatizer
from sklearn.preprocessing import LabelEncoder
from collections import defaultdict
from nltk.corpus import wordnet as wn
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn import model_selection, naive_bayes, svm
from sklearn.metrics import accuracy_score
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.externals import joblib
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import MultinomialNB
import csv
import pickle
import string
import nltk 
import spacy 
from sklearn.feature_extraction import DictVectorizer
from sklearn.tree import DecisionTreeClassifier
from sklearn import tree
import spacy
from functools import reduce
from operator import itemgetter
from urllib.parse import urlsplit
import re
#import mysql.connector
from pymongo import MongoClient
#from urllib.parse import quote_plus
import urllib 
from urllib.parse import urlsplit
try:
    from urllib.parse import urlparse
except ImportError:
    from urlparse import urlparse
#import mysql.connector
from pymongo import MongoClient
#from urllib.parse import quote_plus
try:
    from urllib import quote_plus  
except ImportError:
    from urllib.parse import quote_plus  
from mongoengine import Document, EmbeddedDocument, fields 
from mongoengine.connection import get_db, connect
username = urllib.parse.quote_plus('your_user_name')
password = urllib.parse.quote_plus('your password')
client = MongoClient('mongodb://%s:%s@host/?authSource=admin' % (username, password))
db = client['cyberads']
mycol = db["gender"]
nlp = spacy.load('en_core_web_sm')
np.random.seed(500)
scriptdata = pd.read_csv(r"fiitjee_18.csv")
scriptdata = scriptdata.drop(['_id','allbrowsercookies','bot','brand','browser','cookies','date','description','device','device_address','google_id','host_name',
 'image_text','impression','ip','language','local_hostname','location.IPv4','location.city','location.country_code','location.country_name','location.latitude','location.longitude','location.postal','location.state','mobile','model','mylocalstorage',
 'mysession','os','page_url','pc','referer','tablet','time','title','touch','user_id','visit_num','mobile_no','email','keywords','page_url'], axis = 1)

scriptdata = scriptdata.apply(lambda x: x.astype(str).str.lower())
scriptdata['name'] = scriptdata[scriptdata.columns[0:]].apply(lambda x: ','.join(x.dropna().astype(str)),axis=1)
scriptdata['name']=scriptdata.name.str.replace('\d+', '')
scriptdata['name']= scriptdata['name'].str.replace('[!�#$%&�()*+,-./:;<=>?@[\]^_`{}~]'.format(string.punctuation), " ")
scriptdata= scriptdata['name'].str.split()
script= scriptdata.values.tolist()
flat_list = reduce(lambda x, y: x+y, script)
script = list(dict.fromkeys(flat_list))
str1 = ' '.join(script)
m_increment = 0
f_increment = 0
o_increment = 0
v1 = 0
c1 = 0
v2 = 0
c2 = 0
keyword_output1 = []
keyword_output2 =  []
keyword_output3 = []
c_f1 = []
v_f1 =  []
c_m2 = []
v_m2 =  []
doc = nlp(str1) 
mylist = []
for token in doc: 
      mylist.append((token, token.pos_))
for word in mylist:
    if word[-1] in ['INTJ','ADJ','ADV','DET','PRON']:
        keyword_output2.append(word)
        f_increment = f_increment + 1
  
    elif word[-1] in ['VERBS','NOUN','PROPN','ADP','AUXILARY','CCONJ','SCONJ','PART','prep']:
        keyword_output1.append(word)
        m_increment = m_increment + 1
        
    elif word[-1] in ['SYM','PUNCT','NUM','X']:
        keyword_output3.append(word)
        o_increment = o_increment + 1
res1 = list(map(itemgetter(0), keyword_output1)) 
res2 = list(map(itemgetter(0), keyword_output2)) 
m_key = [str(x) for x in res1]
f_key = [str(y) for y in res2]
unwanted_num = {'a', 'b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','x','y','z','vel', 'aaa', 'rd', 'nd', 'th', 'kg', 'pg', 'sr', 'st', 'csi', 'avs', 'scad', 'psn', 'npr', 'srm', 'trp', 'dpc', 'san', 'ksg', 'psg', 'rev', 'gr', 'vnr', 'rtmbs', 'kle', 'avn', 'gvg', 'dgim', 'byst', 'skd', 'ysr', 'gsfc', 'pmr', 'psv', 'rimt', 'gmr', 'sdnb', 'jct', 'kvsr', 'drbccc', 'pvkk', 'idrbt', 'srv', 'kpr', 'djr', 'bhu', 'bpr', 'ajk', 'psf', 'kgt', 'bt', 'gls', 'avp', 'aps', 'sns','ck','hkbk', 'qis', 'mvsr','mcm','nan', 'rbvrr', 'ggn', 'fdp', 'ndrk', 'svkm', 'nmims', 'kles', 'dyp', 'gd'} 
remove_gram1 = [ele for ele in m_key if ele not in unwanted_num] 
unwanted_num = {'a', 'b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','x','y','z','vel', 'aaa', 'rd', 'nd', 'th', 'kg', 'pg', 'sr', 'st', 'csi', 'avs', 'scad', 'psn', 'npr', 'srm', 'trp', 'dpc', 'san', 'ksg', 'psg', 'rev', 'gr', 'vnr', 'rtmbs', 'kle', 'avn', 'gvg', 'dgim', 'byst', 'skd', 'ysr', 'gsfc', 'pmr', 'psv', 'rimt', 'gmr', 'sdnb', 'jct', 'kvsr', 'drbccc', 'pvkk', 'idrbt', 'srv', 'kpr', 'djr', 'bhu', 'bpr', 'ajk', 'psf', 'kgt', 'bt', 'gls', 'avp', 'aps', 'sns','ck', 'qis', 'mcm', 'rbvrr','nan', 'ggn', 'fdp', 'ndrk', 'svkm', 'nmims', 'kles', 'dyp', 'gd'} 
remove_gram2 = [ele for ele in f_key if ele not in unwanted_num] 

for word in remove_gram2:
    if word[-1] in ['a','e','i','o','u']:
        v_f1.append(word)
        v1 = v1 + 1
  
    else:
        c_f1.append(word)
        c1 = c1 + 1
for word in remove_gram1:
    if word[-1] in ['a','e','i','o','u']:
        v_m2.append(word)
        v2 = v2 + 1
  
    else:
        c_m2.append(word)
        c2 = c2 + 1
keyword_cat1  =   c_m2 + c_f1
keyword_cat2   =   v_m2 + v_f1
data_type1 = pd.DataFrame(keyword_cat2)
data_type2 = pd.DataFrame(keyword_cat1)
data_type1.rename(columns = {0:'name'}, inplace = True) 
data_type2.rename(columns = {0:'name'}, inplace = True) 
data_type1['Gender'] = 'F'
data_type2['Gender'] = 'M'
data_type1.head(2),  data_type2.head(2)
original = data_type2.append(data_type1) 
print(original)
for d in original.values:
   
    mydict = { 'name' : d[0], 'gender' : d[1]} 
    dbins = mycol.insert_one(mydict)
#with open(r'D:\sep\gender.csv', 'w', newline='') as file:
    #writer = csv.writer(file, delimiter=',')
    #writer.writerows(original)
original['name'] = original['name'].str.capitalize() 

original = pd.read_csv(r"malefemale_1.csv")
df_names = original

df_names.gender.replace({'F':0,'M':1},inplace=True)
Xfeatures =df_names['name']
cv = CountVectorizer()
X = cv.fit_transform(Xfeatures)
#gender_vectorizer.close()
cv.get_feature_names()
X
y = df_names.gender
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)
clf = MultinomialNB()
clf.fit(X_train,y_train)
clf.score(X_test,y_test)
print("Accuracy of Model",clf.score(X_train,y_train)*100,"%")
def features(name):
    name = name.lower()
    return {
        'first-letter': name[0], # First letter
        'first2-letters': name[0:2], # First 2 letters
        'first3-letters': name[0:3], # First 3 letters
        'last-letter': name[-1],
        'last2-letters': name[-2:],
        'last3-letters': name[-3:],
    }
features = np.vectorize(features)
df_X = features(df_names['name'])
df_Y = df_names['gender']
#print(X_train)
#print(y_train)
dfX_train, dfX_test, dfy_train, dfy_test = train_test_split(df_X, df_Y, test_size=0.33, random_state=42)
dv = DictVectorizer()
dv.fit_transform(dfX_train)
dclf = DecisionTreeClassifier()
my_xfeatures =dv.transform(dfX_train)
dclf.fit(my_xfeatures, dfy_train)
#print(dclf)
decisiontreModel = open("decisiontreemodel.pkl","wb")
joblib.dump(dclf,decisiontreModel)
decisiontreModel.close()
dctreeModel = open("namesdetectormodel.pkl","wb")
pickle.dump(dclf,dctreeModel)
dctreeModel.close()
NaiveBayesModel = open("naivebayesgendermodel.pkl","wb")
joblib.dump(clf,NaiveBayesModel)
NaiveBayesModel.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
