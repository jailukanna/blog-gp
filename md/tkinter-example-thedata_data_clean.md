---
title: tkinter example thedata data clean (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'thedata data clean'

Functions in program: 
* `def cleanit(data):`
* `def standarizegender(data):`
* `def test_gender(data):`
* `def get_age(data):`
* `def date2age(birth):`
* `def standarizedate(date):`
* `def testicd(data):`
* `def testint(data):`
* `def testthe(data):`

## python thedata data clean

Python tkinter example: thedata data clean

```python
def testthe(data):
    """测试总费用的准确性"""
    data['费用检查'] = ''
    t = data.eval("总费用 == (药品费+挂号费+挂号费+诊察费+检查费+治疗费+手术费+化验费+其他费)")
    data.loc[t == False, '费用检查'] += "总费用总和核算错误；"
    t = data.eval("总费用==(医保统筹支付+医保账户支付+自付费用)")
    data.loc[t == False, '费用检查'] += "统筹支付核算错误；"
    t = data.eval("药品费==(西药费+中成药费+中草药费)")
    data.loc[t == False, '费用检查'] += "药物费用核算出错；"
    if not all(data['费用检查']):
        print("费用检查未通过")
    else:
        print("核算全通过")
    data.loc[data['费用检查'] == '', '费用检查'] = '检查通过'


def testint(data):
    global intg
    """测试数据的完整性"""
    data = data.rename({"患者编号": "病人ID", "疾病名称": "诊断名称", "就诊日期": '诊病时间', "就诊科室": "科室名称",
                        "门诊总费用": "总费用", "西药": "西药费", "中成药": "中成药费", "中草药": "中草药费",
                        "保险统筹基金支付费用": "医保统筹支付",
                        "个人账户支付费用": "医保账户支付", "医保帐户支付":"医保账户支付",
                        "患者自付费用": "自付费用"}, axis=1)
    vars = data.columns
    missing = []
    for var in ['病人ID', '性别', '年龄', '出生日期', '诊断名称', '诊病时间', '科室名称',
                '总费用', '药品费', '西药费', '中成药费', '中草药费', '挂号费', '诊察费', '检查费', '治疗费', '手术费', '化验费',
                '其他费', '医保统筹支付', "医保账户支付", '自付费用']:
        if not var in vars:
            intg = False
            missing.append(var)
        else:
            intg = True
    if not intg:
        print("data missing some columns: {}".format(missing))
    else:
        print("Passed")
    return data


def testicd(data):
    """测试icd是否完成"""
    icd = data['诊断代码'].isna()
    if False in icd:
        print("NaN in data")


def standarizedate(date):
    """标准化日期格式"""
    import pandas as pd, re, datetime, numpy as np
    if isinstance(date, pd.Timestamp):
        y, m, d = (date.year,date.month,date.day)
    elif isinstance(date, datetime.datetime) or isinstance(date, datetime.date):
        y, m, d = (date.year,date.month,date.day)
    elif isinstance(date, str):
        date = re.sub("\s",'',date)
        [y, m, d] = re.split(r"[/\-\\]", date)
    elif np.isnan(date):
        return np.nan
    else:
        try:
            birth = [x for x in re.split(r'\s', date) if x != np.nan][0]
            [y, monthdate] = re.split("年", birth)
            [m, dateraw] = re.split('月', monthdate)
            d = re.split("日", dateraw)[0]
            for i, var2 in zip(range(3), re.split(r'\w', birth)):
                [y, m, d][i] = int(var2)
        except:
            return IndexError  # no birth in data
    return datetime.date(int(y), int(m), int(d))


def date2age(birth):
    """年龄计算功能，将出生日期转换成年龄"""
    import re, numpy as np, pandas as pd, datetime
    if isinstance(birth, pd.Timestamp):
        return birth.to_julian_date()  # 将符合日期格式的格式直接输出
    elif isinstance(birth, datetime.datetime) or isinstance(birth, datetime.date):
        year, month, date = birth.isocalendar()
        return round((year * 365 + month * 30.4167 + date) / 365.25, 1)
    elif isinstance(birth, str):
        if "/" in birth:
            birth = [x for x in re.split(r'\s', birth) if x != np.nan][0]
            [year, month, date] = re.split("/", birth.strip())
            [year, month, date] = [int(x) for x in [year, month, date]]
            return round((year * 365 + month * 30.4167 + date) / 365.25, 1)  # 将"1995 /11/ 10 '等日期换成天再算成年龄
        elif "-" in birth:
            birth = [x for x in re.split(r'\s', birth) if x != np.nan][0]
            [year, month, date] = re.split("\-", birth.strip())
            [year, month, date] = [int(x) for x in [year, month, date]]
            return round((year * 365 + month * 30.4167 + date) / 365.25, 1)  # 将"1995 -11- 10 '等日期换成天再算成年龄
        else:
            try:
                birth = re.sub('[\s+零又个]', '', birth)
                year = any([x in birth for x in ["年", "岁"]])
                month = "月" in birth
                date = any([x in birth for x in ['天' or "日"]])
                if year:
                    [year, monthdate] = re.split("[年岁]", birth)
                else:
                    year = 0;
                    monthdate = birth
                if month:
                    [month, dateraw] = re.split('月', monthdate)
                else:
                    month = 0;
                    dateraw = monthdate
                if date:
                    date = re.split("[日天]", dateraw)[0]
                else:
                    date = 0
                [year, month, date] = [float(x) for x in [year, month, date]]
                return round((year * 365 + month * 30.4167 + date) / 365.25, 1)  # 将'1995年11月10日换成年龄
            except:
                return IndexError  # no birth in data
    elif isinstance(birth, int):
        return birth
    else:
        return np.nan


def get_age(data):
    """标准化年龄格式"""
    if "年龄" in list(data.columns):
        data['age'] = data["年龄"].copy()
        if data["age"].dtype == "int64":
            pass
        else:
            for i in range(len(data)):
                if any([x in data.loc[i, "age"] for x in ["天", '年', '月', "岁", "/", "-"]]):
                    data.loc[i, "age"] = date2age(data.loc[i, "age"])
                if "岁" in str(data.loc[i, 'age']):
                    data.loc[i, 'age'] = data.loc[i, '年龄'][:-1]
    elif "出生日期" in list(data.columns):
        for diagnosis_date in ["入院日期", "就诊日期", "诊病日期", "诊病时间"]:  # 将所有日期变成julian_date
            if diagnosis_date in list(data.columns):
                break
        data['age'] = data[diagnosis_date].copy().apply(date2age) - data["出生日期"].copy().apply(date2age)  # 日期-日期
    else:
        data['age'] = "there aren't any data available for age calculation"
    try:
        data['age'] = data['age'].astype("float")
        print("年龄: 转化成功")
    except ValueError:
        print('年龄: 转化失败')


def test_gender(data):
    import pandas as pd
    return len(pd.Categorical(data['性别']).categories) == 2


def standarizegender(data):
    import pandas as pd
    cats = pd.Categorical(data['性别']).categories
    male = []
    female = []
    for cat in cats:
        if '男' in cat:
            male.append(cat)
        elif '女' in cat:
            female.append(cat)
    for m in male:
        data.loc[data.eval("性别==@m"),"性别"] = "男"
    for f in female:
        data.loc[data.eval("性别==@f"),"性别"] = "女"
    data.loc[data.eval("性别!='男' and 性别!='女'"), "性别"] = '不支持的数据格式'
    if test_gender(data):
        print('性别标准化成功')
    else:
        print("性别标准化失败")


def cleanit(data):
    data = testint(data)
    if intg == True:
        testthe(data)  # 检查费用核算是否准确
    else:
        return print("数据完成/格式出错")
    try:
        data['诊病时间'] = data['诊病时间'].apply(standarizedate)
        print("诊病日期: 标准化成功")
    except:print("诊病日期: 标准化出错")
    try:
        data['出生日期'] = data['出生日期'].apply(standarizedate)
        print("出生日期: 标准化成功")
    except:print("出生日期: 标准化出错")
    get_age(data)
    standarizegender(data)
    return data


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
