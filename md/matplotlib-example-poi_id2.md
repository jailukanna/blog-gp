---
title: matplotlib example poi id2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'poi id2'

Functions in program: 
* `def poiMessagesFraction(dataD):`
* `def computeFraction( poi_messages, all_messages ):`

Modules used in program: 
* `import matplotlib.pyplot`
* `import pickle`
* `import sys`

## python poi id2

Python matplotlib example: poi id2

```python
#!/usr/bin/python

import sys
import pickle
sys.path.append("../tools/")
#sys.path.append("../feature_selection/")

from feature_format import featureFormat, targetFeatureSplit
from tester import dump_classifier_and_data

### Task 1: Select what features you'll use.
### features_list is a list of strings, each of which is a feature name.
### The first feature must be "poi".
#features_list = ['poi','salary','bonus','from_poi_to_this_person', 'total_payments', 'from_this_person_to_poi']
features_list = ['poi','salary','bonus'] # You will need to use more features
from email_preprocess import preprocess
#from sklearn import tree
#clf = tree.DecisionTreeClassifier()


features_train, features_test, labels_train, labels_test = preprocess()
#clf = clf.fit(features_train, labels_train)
#importance = clf.feature_importances_

print("**************")
#print(features_train[0])
#print("**************")
print(" ")
#features_150=features_train[:150]
#
c=0
#for feature in importance:
#   if feature > 0:
#       print((feature))
#       print(('number: ',c))
#   c+=1
#print("features 150 ;  ", features_150)


### Load the dictionary containing the dataset
""" with open("final_project_dataset.pkl", "r") as data_file:
    data_dict = pickle.load(data_file)
    for item in data_dict:
        if c<20:
            print(item)
            print("***   ", data_dict[item])
        c+=1
    print("C  " ,c)
"""
### Task 2: Remove outliers

import matplotlib.pyplot

### read in data dictionary, convert to numpy array/ remove Total line
data_dict = pickle.load( open("../final_project/final_project_dataset.pkl", "r") )
data_dict.pop('TOTAL',0)
'''
#for key in data_dict:
    
    #TotalKey=data_dict(key,['Total'])
    #print(TotalKey)
    print(key)
    if key =='TOTAL':
        data_dict.pop(key,0)
        print("********")
        print("  ")
'''

#features_list = ['poi','salary','bonus','from_poi_to_this_person', 'total_payments', 'from_this_person_to_poi']
data = featureFormat(data_dict, features_list)

#Try plotting different features to see if there are any outliers/ anolmalies

from matplotlib import cm

### your code below
for point in data:
    poi = point[0]
    salary = point[1]
    bonus = point[2]
    #from_poi = point[3]
    #total_payments = point[4]
    #to_poi = point[5]
       
    matplotlib.pyplot.scatter(salary, bonus, c=poi,cmap=cm.jet,vmin=0.,vmax=1.)
    #matplotlib.pyplot.scatter( salary, total_payments,c=poi,cmap=cm.jet,vmin=0.,vmax=1. )
    #matpotlib.pyplot.scatter( bonus, from_poi,c=poi,cmap=cm.jet,vmin=0.,vmax=1.)
    #matplotlib.pyplot.scatter( bonus, total_payments,c=poi,cmap=cm.jet,vmin=0.,vmax=1. )
    #matplotlib.pyplot.scatter( salary, from_poi,c=poi,cmap=cm.jet,vmin=0.,vmax=1. )
    #matplotlib.pyplot.scatter( bonus, to_poi,c=poi,cmap=cm.jet,vmin=0.,vmax=1. )
    

matplotlib.pyplot.xlabel("salary")
#matplotlib.pyplot.xlabel("bonus")

matplotlib.pyplot.ylabel("bonus")
#matplotlib.pyplot.ylabel("total_payments")
#matplotlib.pyplot.ylabel("from_poi_to_this_person")
#matplotlib.pyplot.ylabel("to_poi_from_this_person")

matplotlib.pyplot.show()

### Task 3: Create new feature(s)
from  featureSelectFinalProject import featureSelectFinal

### Store to my_dataset for easy export below.

###ComputeFraction function to create a new feature that shows teh relationship between POI messages and all messages
def computeFraction( poi_messages, all_messages ):
    """ given a number messages to/from POI (numerator) 
        and number of all messages to/from a person (denominator),
        return the fraction of messages to/from that person
        that are from/to a POI
   """


    ###  this code, returns either the fraction of all messages to this person that come from POIs
    ###  or the fraction of all messages from this person that are sent to POIs
    ###  the same code can be used to compute either quantity

    ### "NaN" when there is no known email address and integer division! are dealt with!
    ### in case of poi_messages or all_messages having "NaN" value, returns 0.
       
    if (poi_messages == "naN") or (all_messages == "NaN") or (poi_messages ==0) or (all_messages ==0):
        fraction =0
    else:
        fraction= float(float(poi_messages)/ float(all_messages))
    return fraction

def poiMessagesFraction(dataD):
    submit_dict= {}
    submit_dict = dataD
    c=0
    for name in data_dict:

        data_point = dataD[name]
        c+=1
        #print
        #if c<10:
        #print("%%%%  ",submit_dict[name])
        from_poi_to_this_person = data_point["from_poi_to_this_person"]
        to_messages = data_point["to_messages"]
        fraction_from_poi = computeFraction( from_poi_to_this_person, to_messages )
        #if c<10:
        #    print(fraction_from_poi)
        data_point["fraction_from_poi"] = fraction_from_poi


        from_this_person_to_poi = data_point["from_this_person_to_poi"]
        from_messages = data_point["from_messages"]
        fraction_to_poi = computeFraction( from_this_person_to_poi, from_messages )
        #if c<10:
        #print(fraction_to_poi)
        submit_dict[name].update({"fraction from poi":fraction_from_poi,
                           "fraction_to_poi":fraction_to_poi})
        data_point["fraction_to_poi"] = fraction_to_poi
        #if c<10:
        #print("%%%%  ",submit_dict[name])
    return submit_dict

my_dataset = poiMessagesFraction(data_dict)
features_list = ['poi','salary','fraction_from_poi',  'fraction_to_poi']
print("***!***", features_list)

### Extract features and labels from dataset for local testing
data = featureFormat(my_dataset, features_list, sort_keys = True)
labels, features = targetFeatureSplit(data)

from sklearn import cross_validation

features_train,  features_test, labels_train,labels_test = cross_validation.train_test_split(features, labels, random_state=42, test_size=0.3)


### Task 4: Try a varity of classifiers
### Please name your classifier clf for easy export below.
### Note that if you want to do PCA or other multi-stage operations,
### you'll need to use Pipelines. For more info:
### http://scikit-learn.org/stable/modules/pipeline.html

# Provided to give you a starting point. Try a variety of classifiers.
from sklearn.metrics import accuracy_score, precision_score, recall_score

from sklearn.naive_bayes import GaussianNB
clf = GaussianNB()
clf.fit(features_train, labels_train)
pred = clf.predict(features_test)
accuracy= clf.score(features_train, labels_train)
#accuracy2= clf.accuracy_score(labels_test, pred)
print("NB Gaussian accuracy: ",accuracy)
#print("NB Gaussian accuracy2: ",accuracy2)
print("precision score: ", precision_score(labels_test, pred))
print("recall score; ", recall_score(labels_test, pred))

from sklearn.svm import SVC
clf = SVC(C=1000.0, kernel='rbf')
clf.fit(features_train, labels_train)
pred = clf.predict(features_test)
accuracy= clf.score(features_train, labels_train)
#accuracy2= clf.accuracy_score( labels_test, pred)
print("SVM accuracy: ",accuracy)
#print("SVM accuracy2: ",accuracy2)
print("precision score: ", precision_score(labels_test, pred))
print("recall score; ", recall_score(labels_test, pred))

from sklearn import tree
clf= tree.DecisionTreeClassifier(random_state=42)
clf.fit(features_train, labels_train)
pred = clf.predict(features_test)
accuracy= clf.score(features_train, labels_train)
print("decision tree accuracy: ",accuracy)
print("precision score: ", precision_score(labels_test, pred))
print("recall score; ", recall_score(labels_test, pred))

from sklearn.cluster import KMeans
clf= KMeans(n_clusters=3, random_state = 0).fit(data)
clf.labels_
#it(features_train, labels_train)
pred = clf.fit_predict(features_test)
clf.cluster_centers_

accuracy= clf.score(features_train, labels_train)
print("cluster accuracy: ",accuracy)
#print("precision score: ", precision_score(labels_test, pred))
#print("recall score; ", recall_score(labels_test, pred))
        
        
from sklearn import neighbors
clf= neighbors.KNeighborsClassifier(n_neighbors=3, algorithm='auto')
clf.fit(features_train, labels_train)
pred = clf.predict(features_test)
accuracy= clf.score(features_train, labels_train)
print("k-nearest neighbors accuracy score: ", accuracy_score(labels_test,pred))
print("precision score: ", precision_score(labels_test, pred))
print("recall score; ", recall_score(labels_test, pred))

from sklearn import linear_model
clf= linear_model.LinearRegression()
clf.fit(features_train, labels_train)
pred = clf.predict(features_test)
accuracy= clf.score(features_train, labels_train)
print("linear regression accuracy: ", accuracy)
#print("precision score: ", precision_score(labels_test, pred))
#print("recall score; ", recall_score(labels_test, pred))

### Task 5: Tune your classifier to achieve better than .3 precision and recall 
### using our testing script. Check the tester.py script in the final project
### folder for details on the evaluation method, especially the test_classifier
### function. Because of the small size of the dataset, the script uses
### stratified shuffle split cross validation. For more info: 
### http://scikit-learn.org/stable/modules/generated/sklearn.cross_validation.StratifiedShuffleSplit.html

# Example starting point. Try investigating other evaluation techniques!
from sklearn.cross_validation import train_test_split
features_train, features_test, labels_train, labels_test = \
    train_test_split(features, labels, test_size=0.3, random_state=42)


### Task 6: Dump your classifier, dataset, and features_list so anyone can
### check your results. You do not need to change anything below, but make sure
### that the version of poi_id.py that you submit can be run on its own and
### generates the necessary .pkl files for validating your results.

print(features_list)
dump_classifier_and_data(clf, my_dataset, features_list)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
