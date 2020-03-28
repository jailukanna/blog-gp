---
title: tkinter example thedata icd (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'thedata icd'

Functions in program: 
* `def clean_diag(case):`
* `def split_case(case):`
* `def FuzzyMatchDisease(disease):`
* `def has_icd(input):`
* `def saveadress(lb):`
* `def fileadress(lb):`
* `def asksave():`
* `def askfile():`

Modules used in program: 
* `import multiprocessing`

## python thedata icd

Python tkinter example: thedata icd

```python
"""tool for ICD standardize"""
import multiprocessing

def askfile():
    import tkinter
    root = tkinter.Tk()
    root.title("File")
    width = root['width']=600
    height=root['height']=400
    root.resizable(False,False)
    screenwidth = root.winfo_screenwidth()
    screenheight = root.winfo_screenheight()
    alignstr = '%dx%d+%d+%d' % (width, height, (screenwidth - width) / 2, (screenheight - height) / 2)
    root.geometry(alignstr)
    lb = tkinter.Text(root,width=600,height=200)
    lb.place(x=0,y=10)
    #lb.grid(row=0,column=0)
    filenames = fileadress(lb)
    btn = tkinter.Button(root, text="Ok", command=root.destroy,width=10)
    btn.place(x=290,y=370)
    #btn.grid(row=2,column=0)
    root.mainloop()
    if len(filenames)==1:
        return filenames[0]
    else:
        return filenames


def asksave():
    import tkinter
    root = tkinter.Tk()
    root.title("Save File")
    width = root['width']=600
    height=root['height']=400
    root.resizable(False,False)
    screenwidth = root.winfo_screenwidth()
    screenheight = root.winfo_screenheight()
    alignstr = '%dx%d+%d+%d' % (width, height, (screenwidth - width) / 2, (screenheight - height) / 2)
    root.geometry(alignstr)
    lb = tkinter.Text(root,width=600,height=200)
    lb.place(x=0,y=10)
    #lb.grid(row=0,column=0)
    filenames = saveadress(lb)
    btn = tkinter.Button(root, text="Ok", command=root.destroy,width=10)
    btn.place(x=290,y=370)
    #btn.grid(row=2,column=0)
    root.mainloop()
    if len(filenames)==1:
        return filenames[0]
    else:
        return filenames


def fileadress(lb):
    import tkinter.filedialog
    filenames = list(tkinter.filedialog.askopenfilenames())
    text = ''.join(["{};\n".format(x) for x in filenames])
    lb.insert('0.0',"You've selected:[\n{}]".format(text))
    return filenames


def saveadress(lb):
    import tkinter.filedialog
    filenames = tkinter.filedialog.asksaveasfilename()
    lb.insert('0.0',"You've selected:[\n{}]".format(filenames))
    return filenames


def has_icd(input):
    import re
    try:return re.findall(r'[A-Z]\d\d\.\w\w\w',input)[0]
    except:return


def FuzzyMatchDisease(disease):
    """give a disease name and return the best match disease standard name,return name and icd"""
    import pandas as pd, fuzzywuzzy.fuzz as fuzz, fuzzywuzzy.process as process, os
    cutoff=60
    icd = pd.read_json(
        "{}\\thedata\\icd.json".format(os.getcwd()))
    if isinstance(disease,float):return "NOT MATCHED"
    if isinstance(disease, list):
        result1 = pd.DataFrame(columns=['r1','sc'])
        for d in disease:
            try:
                r1, sc = process.extractBests(d, list(icd.loc[:, '诊断名称']), limit=1,
                                              score_cutoff=cutoff, scorer=fuzz.token_sort_ratio)[0]
                # result1.append(r1)
                result1 = pd.concat([result1, pd.DataFrame({'r1': [r1], 'sc': [sc]})])
            except KeyboardInterrupt:
                return
            except:
                r1 = 'NOT_MATCHED'
                sc = 0
                result1 = pd.concat([result1, pd.DataFrame({'r1': [r1], 'sc': [sc]})])
                # r1 = 'NOT_MATCHED'
            # result1.append(r1)
        # result1.sort_values('sc',ascending=False,inplace=True)
        try:return result1['r1'].values[0] #KeyError:'r1'
        except KeyError:
            print(result1)
            return "KeyError"
        except IndexError: return "NOT_MATCHED"
    elif isinstance(disease, str) and disease != '':
        try:
            r1 = process.extractBests(disease, list(icd.loc[:, '诊断名称']), limit=1,
                                      score_cutoff=cutoff, scorer=fuzz.token_sort_ratio)[0][0]
        except KeyboardInterrupt:
            return
        except:
            r1 = 'NOT_MATCHED'
        return r1[0]
    else:
        return 'NOT_MATCHED'


def split_case(case):
    """return turple of ICD(if exists) and list of diagnosis"""
    import re
    # get icd-10
    if isinstance(case, float):
        return '', ''
    try:
        if re.search(r'\W\w\d+\.\d+', case):
            ICD = re.findall(r'\w\d+\.\d+', case.strip())[0]
        elif re.search(r'\w\w\w\d+'):
            ICD = re.findall(r'\w\w\w\d+')
        else:
            ICD = re.search(r'\w\d+', case.strip())[0]
    except:
        ICD = ''
    case = ''.join(re.split(ICD, case.strip()))
    case = ','.join(re.split(r'\d+\.', case))
    return re.findall('\w+', case)


def clean_diag(case):
    "接受一个字段,清理数字，字符等"
    import re
    if isinstance(case, float):
        return ['']
    else:
        # case = ''.join(re.split(r'(\(|\))',case))
        # case = ''.join(re.split('(\[|\])',case))
        case = re.sub(r'[a-z]{3,}','',case)
        case = re.sub(r'\d+','', case)
        case = re.sub('[\[\]\(\)\-]','', case)
        return re.findall(r"\w+", case)



class fuzzit(multiprocessing.Process):
    def __init__(self, data, range1, range2, queue):
        self.data = data[range1:range2]
        self.queue = queue
        multiprocessing.Process.__init__(self)

    def run(self):
        self.data.loc[:, 'match'] = self.data['diag'].apply(FuzzyMatchDisease)
        self.queue.put(self.data)
        # data.loc[self.r1:self.r2, 'match'] =data.loc[self.r1:self.r2,'diag'].apply(FuzzyMatchDisease)




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
