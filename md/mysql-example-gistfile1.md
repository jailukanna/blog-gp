---
title: mysql example gistfile1 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'gistfile1'

Functions in program: 
* `def SimilariteCosinus(idPdt1,idPdt2,type):`
* `def top3 (vect,vec):`
* `def tfidf(word, blob, bloblist):`
* `def idf(word, bloblist):`
* `def n_containing(mot,list):`
* `def term_frequence (mot,vect):`

Modules used in program: 
* `import numpy`
* `import math`
* `import mysql.connector`
* `import nltk`

## python gistfile1

Python mysql example: gistfile1

```python
import nltk
import mysql.connector
from nltk.stem import lancaster
from nltk.corpus import stopwords
import math
import numpy
matriceFreq=numpy.zeros((0, 0))
from scipy import spatial
TotaliteMots=[]
NbProduits=0
AllSetMots={}
nbMots=0
#DATABASE:
conn=mysql.connector.connect(host="localhost",port=3306,user="root",password="",database="electronix")
cursor=conn.cursor()
print("it is OK")
cursor.execute("""SELECT * FROM products""") 
rows=cursor.fetchall()
print(rows)
#FUNCTIONS 
#frequence d'une mot dans un vecteur list
#TextBlob is a Python (2 and 3) library for processing textual data. 

def term_frequence (mot,vect):
    k=0
    li=list(vect)
    for l in li:
        if mot == l:
            k+=1
    return k/len(vect)
#occurence d'une mot dans une liste
def n_containing(mot,list):
    k=0
    for i in list:
        if mot == i:
            k+=1
    return k/len(list)

def idf(word, bloblist):
    return math.log(len(bloblist) / (1 + n_containing(word, bloblist)))

def tfidf(word, blob, bloblist):
    return term_frequence(word, blob) * idf(word, bloblist)
def top3 (vect,vec):
   return sorted(zip(vect,vec.keys()), reverse=True)[:5]

def SimilariteCosinus(idPdt1,idPdt2,type):
     if type == 1:
      return ( 1 - spatial.distance.cosine(matriceBin[idPdt1], matriceBin[idPdt2]))
     return(1 - spatial.distance.cosine(matriceFreq[idPdt1],matriceFreq[idPdt2]))

#A processing interface for removing morphological affixes from words.
# This process is known as stemming.
stemmer=lancaster.LancasterStemmer()
for row in rows:
    descrption=row[5]
    idPdt=row[0]
    print('Description: {}'.format(row[5]))
    #A tokenizer that divides a string into substrings by splitting on the specified string 
    Mots=nltk.wordpunct_tokenize(descrption)
    print(Mots) 
    MotsStemming=Mots[:]
    for i in range(len(MotsStemming)):
      MotsStemming[i]=stemmer.stem(MotsStemming[i])
    print(MotsStemming)
    ang_stopwords=list(stopwords.words())
    ang_stopwords.extend([".",",",",","#","!","?","...","'",'"'])
    MotsTous=[mot for mot in MotsStemming if mot.lower() not in ang_stopwords]
    print(MotsTous)
    MotsClefs={mot for mot in MotsStemming if mot.lower() not in ang_stopwords}
    TotaliteMots.extend([*MotsClefs])
    nbMots+=len(MotsClefs)
    AllSetMots[idPdt]=MotsClefs
    print(AllSetMots[idPdt])
    NbProduits=NbProduits+1
conn.close() 
matriceBin=numpy.zeros((NbProduits,nbMots))
matriceFreq=numpy.zeros((NbProduits,nbMots))
ListeMot=list(AllSetMots.values())
i=0
for id in AllSetMots.keys(): #idprodh
    j=0 
    for mot in TotaliteMots:
     if mot in ListeMot[i]:
      matriceBin[i][j]=1
      matriceFreq[i][j]=tfidf(mot,ListeMot[i],TotaliteMots)
     j+=1
    i+=1
print("****************matrice binaire**************************")
print("/n")

print(matriceBin)
print("****************matrice frequence**************************")
print("/n")
print(matriceFreq)

MatriceSimilariteBin=numpy.zeros((NbProduits,NbProduits))
for k in range(NbProduits):
 for z in range(NbProduits):
  MatriceSimilariteBin[k][z]=SimilariteCosinus(k,z,1) 
print("/n")

print("****************matrice Similarite  bin**************************")
print("/n")
print(MatriceSimilariteBin)
print("/n")
print("########################################################################################################")
MatriceSimFreq=numpy.zeros((NbProduits,NbProduits))
k=0
for id1 in AllSetMots.keys():
    z=0
    for id2 in AllSetMots.keys():
        MatriceSimFreq[k][z]=SimilariteCosinus(k,z,2)
        z+=1
    k+=1
print("****************matrice Similarite  bin**************************")
print("/n")
print(MatriceSimFreq)
for i in idPdt:
      ltop1=list(top3(MatriceSimFreq[i],AllSetMots))
      print(ltop1)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
