---
title: mysql example data lake similarity similarity prefix filter (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'data lake similarity similarity prefix filter'


Modules used in program: 
* `import math`

## python data lake similarity similarity prefix filter

Python mysql example: data lake similarity similarity prefix filter

```python
import math
class SimilarityPrefixFilter:
    def __init__(self,list_s,list_r,threshold=0,l_prefix=0,jaccard_similarity_threshold=0):
        self.list_s = list_s
        self.list_r = list_r
        self.element2order = {}
        self.inverted_list = {}
        self.threshold = threshold #overlap
        self.l_prefix = l_prefix  # 1≤l_prefix ≤ overlap
        self.order = 0
        self.delta_inverted_list = {}
        self.jaccard_similarity_threshold = jaccard_similarity_threshold


    def prepare(self):
        records = self.list_s+self.list_r

        #建立全局顺序
        for record in records:
            for e in record:
                if e not in self.element2order.keys():
                    self.element2order[e] = self.order
                    self.order = self.order + 1
        self.element2order = {'jagadish': 0, 'koudas': 1, 'nick': 2, 'divesh': 3, '2011': 4, 'vldb': 5, 'edbt': 6, 'icde': 7, 'sigmod': 8}
        print(self.element2order)
        #根据全局顺序，重排self.list_s,self.list_r中的每一条rocord
        for record in self.list_s:
            record.sort(key=lambda x:self.element2order[x])
        for record in self.list_r:
            record.sort (key=lambda x: self.element2order[x])

        self.records =  self.list_s+self.list_r
        # #根据每个record的长度，对records排序
        self.records.sort(key = lambda x :len(x))

    def all_pairs(self):
        result = []
        for x_index in range(len(self.records)):
            x = self.records[x_index]
            record2int = {}#empty map from record id to int;
            p = len(x) - math.ceil(self.jaccard_similarity_threshold*len(x)) +1
            for i in range(p):
                w = x[i]
                if w in self.inverted_list.keys():
                    for j in range(len(self.inverted_list[w])):
                        y = self.inverted_list[w][j]
                        if len(self.records[y]) >= self.jaccard_similarity_threshold*len(x):
                            if y not in record2int.keys():
                                record2int[y] = 1
                            else:
                                record2int[y] = record2int[y] + 1
                    pi = len(x) - math.ceil(2*self.jaccard_similarity_threshold*len(x)/(1+self.jaccard_similarity_threshold)) +1
                    if i < pi:
                        self.inverted_list[w] = list(set(self.inverted_list[w]).union([x_index]))
                else:
                    self.inverted_list[w] = [x_index]
            for y in record2int.keys():
                similarity = self.similarity(self.records[x_index],self.records[y],x_index)
                if similarity > self.jaccard_similarity_threshold:
                    result.append((x_index,y))
        print(result)




    def build_inverted_list(self):
        for index in range(self.list_s.__len__()):
            for i in range(self.l_prefix+1):
                if self.list_s[index][i] not in self.inverted_list.keys():
                    self.inverted_list[self.list_s[index][i]] = [index]
                else:
                    self.inverted_list[self.list_s[index][i]].append(index)

    def build_delta_inverted_list(self):
        for prefix_len in range(1,self.threshold+1):
            self.delta_inverted_list[prefix_len] = {}
            for index in range (self.threshold+1):
                if prefix_len == 1 :
                    for j in range(2):
                        if self.list_s[index][j] not in  self.delta_inverted_list[prefix_len].keys ():
                            self.delta_inverted_list[prefix_len][self.list_s[index][j]] = [index]
                        else:
                            self.delta_inverted_list[prefix_len][self.list_s[index][j]].append (index)
                else:
                    if self.list_s[index][prefix_len] not in  self.delta_inverted_list[prefix_len].keys ():
                        self.delta_inverted_list[prefix_len][self.list_s[index][prefix_len]] = [index]
                    else:
                        self.delta_inverted_list[prefix_len][self.list_s[index][prefix_len]].append (index)
        print('delta_inverted_list: ',self.delta_inverted_list)

    def find_cadidates(self,reocrd_r):
        self.candidates = {}
        prefix_length_r = len(reocrd_r)-self.threshold+self.l_prefix
        prefix_r = reocrd_r[0:prefix_length_r]
        for e in prefix_r:
            if e in self.inverted_list.keys():
                for s in self.inverted_list[e]:
                    if s not in self.candidates.keys():
                        self.candidates[s] = 1
                    else:
                        self.candidates[s] = self.candidates[s] + 1
        self.caculate_similarity(reocrd_r)

    def caculate_similarity(self,record_r):
        for  index in self.candidates.keys():
            if self.candidates[index] >= self.l_prefix:
                record_s = self.list_s[index]
                self.similarity(record_s,record_r,index)

    def similarity(self,s,r,s_index=-1):
        s = set(s)
        r = set(r)
        result = float(len(s &r )/len(s | r))
        if result >= self.jaccard_similarity_threshold:
            print('similarity:   ',r,'&S[',s_index,']: ' ,result)
        return  result

s = [['nick', 'koudas', '2011', 'vldb', 'sigmod'],
     ['nick', 'vldb', 'icde', 'sigmod', 'edbt' ],
     ['koudas', 'divesh', 'sigmod', 'icde', 'edbt'],
     ['icde', 'sigmod', '2011', 'jagadish', 'divesh'],
     ['2011', 'vldb', 'edbt', 'icde', 'jagadish']]

r = [['vldb', 'sigmod', 'icde', '2011', 'jagadish'],
     ['jagadish', 'koudas', 'vldb', 'edbt', 'icde'],
     ['koudas', 'divesh', 'jagadish', 'edbt', 'icde'],
     ['vldb', 'icde', 'koudas', 'jagadish', 'divesh'],
     ['2011', 'divesh', 'edbt', 'vldb', 'sigmod']]



test = SimilarityPrefixFilter(s,r,jaccard_similarity_threshold=0.6)
test.prepare ()
# test.build_inverted_list ()
# print('s ','*'*20)
# print(test.list_s)
# print('r ','*'*20)
# print(test.list_r)
# test.find_cadidates(r[0])
# test.build_delta_inverted_list()
test.all_pairs()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
