---
title: tkinter example thedata   init   (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'thedata   init  '


## python thedata   init  

Python tkinter example: thedata   init  

```python
if __name__=='__main__':
    import pandas as pd,os,time,numpy
    from thedata.icd import *
    from thedata.data_clean import cleanit
    filepath = askfile()
    #filepath = "{}/onedrive/pycharmprojects/ds/beijing/data/test_data.xls".format(os.environ.get('USERPROFILE'))
    data = pd.read_excel(filepath)
    if '疾病名称' in data.columns:
        data = data.rename({'疾病名称':'诊断名称'},axis=1)
    data.dropna(axis=0,inplace=True,thresh=5)#第一步去除空白
    data = pd.DataFrame({'诊断名称':pd.Categorical(data['诊断名称']).categories})#将诊断名称化为分类变量减少计算量
    result = [pd.merge(data,pd.read_json("{}/thedata/icd.json"
                                         "".format(os.getcwd()))
                             .dropna(axis='rows',subset=['icd'])
                             #.reset_index().drop('index',axis=1))
                             ,left_on="诊断名称",right_on='诊断名称',how='left')#通过合并获得能直接匹配的诊断名称
        ]#增加一个没有任何万全匹配的情况的except
    data = data[result[0]['icd'].isna()].reset_index().drop('index',axis=1)#将分类去除已经匹配的减少计算量
    result[0]=result[0][result[0]['icd'].notna()]
    result[0]['match']=result[0]['诊断名称']#构建变量
    data['icd']=data['诊断名称'].copy().apply(has_icd)#将诊断名称中有icd的提取出来作为icd
    result.append(data[data['icd'].notnull()].copy())
    result[1] = pd.merge(result[1],
                      pd.read_json("{}/thedata/icd.json".format(os.getcwd()))
                      .dropna(axis='rows',subset=['icd'])
                      #.reset_index().drop('index',axis=1),
                      ,left_on='icd',how='left',right_on='icd').rename({'诊断名称_x':'诊断名称','诊断名称_y':'match'},axis=1)
    data = data[data['icd'].isnull()].drop('icd',axis=1)
    result[1]['match'].fillna("诊断含icd编码",inplace=True)
    result=[pd.concat(result).reset_index().drop('index',axis=1)]
    data['diag'] = data['诊断名称'].copy().apply(clean_diag)
    processnum = 4
    r1 = [0 + i * len(data) // processnum for i in range(processnum)]
    r2 = [(i+1) * len(data) // processnum for i in range(processnum)]
    Threads = []
    queue = multiprocessing.Queue()
    for i in range(processnum):
        p = fuzzit(data,r1[i], r2[i],queue)
        p.start()
        Threads.append(p)
    i=0
    no_alive=False
    while len(result)<(len(Threads)+1):
        i +=1
        print('\rProcessing...{0}% {1}'.format(len(result)*100//(len(Threads)+1),['\\','|','/','--'][i%4]),end='')
        try:
            result.append(queue.get(block=False))
            if any([p.is_alive() for p in Threads]):pass
            else:
                no_alive=True
                break
        except multiprocessing.queues.Empty: pass#print('\rno data...',end='')
        time.sleep(3)
    print('\rProcessing...{0}% {1}'.format(100, ['\\', '|', '/', '--'][i % 4]), end='')
    for p in Threads:
        p.join()
    result = pd.concat(result).reset_index().drop('index',axis=1)
    print("REPORT:\n完整性测试:",end='')
    #result.drop('index',axis=1).to_json("{}/onedrive/pycharmprojects/ds/beijing/data/test_data.json".format(os.environ.get('USERPROFILE')))
    data = cleanit(pd.read_excel(filepath))
    if '疾病名称' in data.columns:
        data = data.rename({'疾病名称':'诊断名称'},axis=1)
    data = pd.merge(data,result[['诊断名称','match','icd']],how="left")#还要将icd的也搬出来
    data = pd.concat([data[data['icd'].notna()],
                      pd.merge(data[data['icd'].isna()],#将没有icd的补全icd
                               pd.read_json("{}/thedata/icd.json"
                                            .format(os.getcwd())).dropna(axis='rows',subset=['icd'])
                               , how='left', left_on="match", right_on='诊断名称')\
                     .drop(["诊断名称_y","icd_x"], axis=1).rename({'诊断名称_x': '诊断名称',"icd_y":"icd"}, axis=1)])\
        .reset_index().drop('index', axis=1)
    if no_alive == True:
        print("Error in multiprocessing, processes were dead before ended")
    else:data.to_excel(asksave())
    print("MATCHED:{2}\nPERFECT_ACCURACY:{0}\nNOT_MATCHED:{1}"\
          .format(round(len(data.query("诊断名称==match"))/len(data),2),\
                  round(len(data.query("match=='NOT_MATCHED'"))),
                  round(len(data[data['icd'].notna()])/len(data),2)))
    #data.to_excel('_updated.'.join(re.split(r'\.',filepath)))
    #print("Finished, see your file at {}".format(filepath))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
