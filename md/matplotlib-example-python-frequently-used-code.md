---
title: matplotlib example python-frequently-used-code (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'python-frequently-used-code'

Functions in program: 
* `def save_list(lines, filename):`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib.pyplot as plt`
* `import _pickle as cPickle`
* `import tarfile`
* `import requests`
* `import sys`
* `import os`
* `import glob`
* `import glob`
* `import os`
* `import os`
* `import math`
* `import time`
* `import time`

## python python-frequently-used-code

Python matplotlib example: python-frequently-used-code

```python
###############################
#                             #
#    current time and date    #
#                             #
###############################

import time
time.strftime("%H:%M:%S") # 19:32:22'
time.strftime("%Y%m%d") # '20180214'




######################
#                    #
#    Elapsed time    #
#                    #
######################

import time
start = time.time() 

time.sleep(20)

end = time.time()
print(("Total running time: {} seconds".format(end-start)))




#########################
#                       #
#    print(function     #)
#                       #
#########################

# print(elements in sequence objects line by line )
print("Most Common Words", *vocab.most_common(50), sep='\n')
# Most Common Words
# ('said', 6027)
# ('million', 4627)
# ('new', 2793)
# ('company', 2680)
# ('year', 2379)
# ('would', 2308)
# ('says', 2092)




#############################
#                           #
#    replace using roop     #
#                           #
#############################

# Pad all punctuation with spaces on both sides
for char in ['.', '"', ',', '(', ')', '!', '?', ';', ':']:
    norm_text = norm_text.replace(char, ' ' + char + ' ')





###################
#                 #
#    selenium     #
#                 #
###################

# for mac after `brew install chromedriver`
driver = webdriver.Chrome()

# for windows after download from "https://sites.google.com/a/chromium.org/chromedriver/downloads"
driver = webdriver.Chrome('chromedriver')



###################
#                 #
#    Recording    #
#                 #
###################

import math

og = "12.08"
speed = 1.25
og_list = og.split(".")
og_seconds = int(og_list[0])*60+int(og_list[1])
og_m = int(og_list[0])
og_s = int(og_list[1])
cg_seconds = math.ceil(og_seconds/speed)        # roundup
cg_m = cg_seconds // 60
cg_s = cg_seconds % 60
print("[x%.2f]배속 : [%d분 %d초]가 [%d분 %d초]로 바뀌었습니다." % (speed, og_m, og_s, cg_m, cg_s))
# [x1.25]배속 : [12분 8초]가 [9분 43초]로 바뀌었습니다.




###################
#                 #
#    File Path    #
#                 #
###################

# get the abspath of the current file
import os
base_dir = os.path.dirname(os.path.abspath("__file__"))
my_file = os.path.join(base_dir, 'myfile.txt')
print(my_file)


# get all the files and directories (1) +++ you can search the file path back and forth by typing dot or double dot
import os
os.listdir('.')

# get all the files and directories (2)
from os import listdir
for filename in listdir('data/txt_sentoken/neg'):
    print(filename)

# get all the files but directories
import glob
glob.glob("*.*")

# get specific titled files
import glob
glob.glob("*.txt")


# get the absolute path by +++ you can search the file path back and forth by typing dot or double dot
import os
print(os.getcwd())
print(os.path.abspath("../data/books_text_full/Fantasy") ) 


# append parent directoy as path
import sys
sys.path.append(os.pardir)



###################
#                 #
#    File Save    #
#                 #
###################
 
# save contents of a list as file
def save_list(lines, filename):
    # convert lines to a single blob of text
    data = '\n'.join(lines)
    # open file
    file = open(filename, 'w')
    # write text
    file.write(data)
    # close file
    file.close()
    
# ...save tokens as a vocabulary file
save_list(tokens, 'vocab.txt')


# save the web content using requests
import requests

filename = 'aclImdb_v1.tar.gz'
url = u'http://ai.stanford.edu/~amaas/data/sentiment/' + filename
r = requests.get(url)
with open(filename, 'wb') as f:
    f.write(r.content)

# ...extract zip file
import tarfile

tar = tarfile.open(filename, mode='r')
tar.extractall()
tar.close()




########################
#                      #
#    Data Structure    #
#                      #
########################


# Counter: a dictionary mapping of words and their counts that allow us to easily update and query
from collections import Counter

vocab = Counter()
tokens = ['films', 'adapted', 'comic', 'books', 'plenty', 'plenty', 'success', 'success', 'success']
vocab.update(tokens)
print(vocab)
# Counter({'success': 3, 'plenty': 2, 'films': 1, 'adapted': 1, 'comic': 1, 'books': 1})
print(vocab.most_common(2))
# [('success', 3), ('plenty', 2)]



#################
#               #
#    cPickle    #
#               #
#################

import _pickle as cPickle
with open('pair.pkl', 'wb') as f:
    cPickle.dump(pair, f)

    
    
 
####################
#                  #
#    matplotlib    #
#                  #
#################### 

# matplotlib getting started (1) - plt.plot()
# ===================================================================
%matplotlib inline
import matplotlib.pyplot as plt
plt.plot([1,1.6,3])


# matplotlib getting started (2) - plt.show()
# ===================================================================
import matplotlib.pyplot as plt
import numpy as np

fig = plt.figure()
data = np.arange(900).reshape((30,30))
for i in range(1,5):
    subplot=fig.add_subplot(2,2,i)
    subplot.set_title("%d" % i)
    subplot.imshow(data)

plt.suptitle('Main title')
plt.show()  


# matplotlib debugger - when it does nothing or rasies error
# ===================================================================
import matplotlib
matplotlib.use('TkAgg')
import matplotlib.pyplot as plt


# MNIST test prediction
# ===================================================================
%matplotlib inline
import matplotlib.pyplot as plt
import numpy as np

feed_dict={X: mnist.test.images, Y: mnist.test.labels, keep_prob: 1}
labels = sess.run(predictions, feed_dict=feed_dict)

fig = plt.figure()
data = mnist.test.images[i].reshape((28, 28))
for i in range(10):
    subplot=fig.add_subplot(2,5,i+1)
    subplot.set_xticks([]) # x와 y의 눈금을 출력하지 않습니다.
    subplot.set_yticks([])
    subplot.set_title("%d" % np.argmax(labels[i]))
    data = mnist.test.images[i].reshape((28, 28))
    subplot.imshow(data, cmap="binary")

plt.suptitle('Test prediction')
plt.show()  



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
