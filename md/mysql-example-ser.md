---
title: mysql example ser (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ser'

Functions in program: 
* `def api_get_sal():`
* `def api_get():`
* `def rank_convert(rank):`
* `def retrieve_cic(cic):`
* `def retrieve_med_loc(code):`
* `def retrieve_med_in(code):`
* `def retrieve_med_code(code):`
* `def retrieve_status(status):`
* `def normalize_label(label):`
* `def strip_it(s):`

Modules used in program: 
* `import numpy as np`
* `import pandas as pd`
* `import requests`
* `import mysql.connector`

## python ser

Python mysql example: ser

```python
from flask import Flask, url_for
from flask import request
from flask import json
from flask import Response
from flask import jsonify

from gevent.pywsgi import WSGIServer

import mysql.connector
from mysql.connector import Error
from mysql.connector import errorcode
import requests

# import dataset and deploying encoders

from numpy import array
import pandas as pd
import numpy as np
from sklearn import preprocessing
from sklearn.preprocessing import quantile_transform
from xgboost import XGBClassifier

df = pd.read_csv("new1.txt",delimiter="|", names=["phone","arpu","home","arpu2","age","is_prepaid","id","cic_status","num_top_friend","salary","rank_id","action_avg","comment_avg","diff_day","freq_post","friend_education_level_0","friend_education_level_1","friend_education_level_2","friend_education_level_3","friend_pay_first","friend_pay_later","likes_count","mode_time_frame","post_count","reaction_avg","credit_score","active","age","end_date","package"]);
df.replace("empty", np.nan , inplace=True)
df.replace(-1, np.nan , inplace=True)
df.iloc[:,11:28].astype(float,inplace=True)
df.fillna(0, inplace=True)
df.drop(columns=["phone"], inplace=True)

# Normalize Home
def strip_it(s):
    return s.strip()

le_home = preprocessing.LabelEncoder()
df.home = df.home.astype(str).apply(strip_it)
le_home.fit(df.home)

df.home = le_home.transform(df.home)

# Normalize Package
le_p = preprocessing.LabelEncoder()
df.package = df.package.astype(str).apply(strip_it)
le_p.fit(df.package)

df.package = le_p.transform(df.package)

df.arpu = df.arpu.astype(float).rank(pct=True) * 10
df = df.astype(float)

def normalize_label(label):
    if(label > 0):
        return 1
    return 0

# Preparing Data For Training
from sklearn.model_selection import train_test_split
from sklearn.feature_selection import VarianceThreshold

X = df.iloc[:,[0,1,14,15,16,17,18,24,26,27]].values
y = df.iloc[:, 9].apply(normalize_label).astype(int).values

print(df.iloc[:,[0,1,14,15,16,17,18,24,26,27]].columns)

sel = VarianceThreshold(threshold=(.8 * (1 - .8)))
sel.fit_transform(X)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)

regressor = XGBClassifier(n_estimators=100, random_state=0,min_samples_leaf = 3,min_samples_split = 8)
regressor.fit(X_train, y_train)

#  MODEL FOR SALARY

df = pd.read_csv("74k-sal.csv", encoding = "utf-8", sep = ",")
df.replace("empty", np.nan , inplace=True)
df.replace(-1, np.nan , inplace=True)
df.fillna(0, inplace=True)
df.drop(columns=["phone","user_id","dt"], inplace=True)

df_sal = pd.read_csv("all-predicted",delimiter="|", names=["phone","sal"])

# preprocessing Data

def retrieve_status(status):
    if(status == "0"):
        return 0
    else:
        return 1

df['status'] = df.avayStatus.apply(retrieve_status)

def retrieve_med_code(code):
    try:
        return code[:2]
    except:
        return "0"
def retrieve_med_in(code):
    try:
        return int(code[2:3])
    except:
        return 0

def retrieve_med_loc(code):
    try:
        return int(code[5:7])
    except:
        return 0

df['med_code'] = df.medicalInsuranceId.apply(retrieve_med_code)
df['med_in'] = df.medicalInsuranceId.apply(retrieve_med_in)
df['med_loc'] = df.medicalInsuranceId.apply(retrieve_med_loc)


def retrieve_cic(cic):
    if("cb" in cic):
        return 2
    if("cy" in cic):
        return 1
    else:
        return 0

df['cic'] = df.cic_status.apply(retrieve_cic)

feats = ["home","fbxPackage","fbxIsActive","med_code"]
le_l = preprocessing.LabelEncoder()
for feat in feats:
    le_x = preprocessing.LabelEncoder()
    df[feat] = df[feat].astype(str)
    le_x.fit(df[feat])
    df[feat] = le_x.transform(df[feat])
    le_l = le_x

df.drop(columns=["cic_status","avayStatus","medicalInsuranceId","family_info"],inplace=True)
df.astype(float,inplace=True)

df0 = df[(df.med_code > 0)]
df1 = df[(df.education_level > 0)]

from sklearn.model_selection import train_test_split
from sklearn.feature_selection import VarianceThreshold
from sklearn import metrics
from sklearn.metrics import mean_absolute_error
from xgboost import XGBRegressor

regressor0 = XGBRegressor(n_estimators=100, random_state=0,min_samples_leaf = 3,min_samples_split = 8)
regressor1 = XGBRegressor(n_estimators=100, random_state=0,min_samples_leaf = 3,min_samples_split = 8)

# MODEL 1

X = df0.drop(columns=["sal"]).loc[:,['med_code']].values
y = df0.sal.astype(float).values

print(len(X))

sel = VarianceThreshold(threshold=(.8 * (1 - .8)))
sel.fit_transform(X)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.1, random_state=0)
regressor0.fit(X_train, y_train)
y_pred = regressor0.predict(X_test)
print(mean_absolute_error(y_test, y_pred))

# MODEL 2

X = df1.drop(columns=["sal"]).loc[:,['comment_avg']].values
y = df1.sal.astype(float).values

print(len(X))

sel = VarianceThreshold(threshold=(.8 * (1 - .8)))
sel.fit_transform(X)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.1, random_state=0)

regressor1.fit(X_train, y_train)
y_pred = regressor1.predict(X_test)
print(mean_absolute_error(y_test, y_pred))

app = Flask(__name__)

connection = mysql.connector.connect(host='178.128.85.52',database='db_dcredit',user='dcre',password='dcredit@2019')

def rank_convert(rank):
    r = float(rank)
    rank_number = 0
    if(r > 0.18):
        rank_number = 9
    if(r > 0.16 and r < 0.18):
        rank_number = 8
    if(r > 0.14 and r < 0.16):
        rank_number = 7
    if(r > 0.13 and r < 0.14):
        rank_number = 6
    if(r > 0.11 and r < 0.13):
        rank_number = 5
    if(r > 0.09 and r < 0.11):
        rank_number = 4
    if(r > 0.07 and r < 0.09):
        rank_number = 3
    if(r > 0.06 and r < 0.07):
        rank_number = 2
    if(r > 0.04 and r < 0.06):
        rank_number = 1
    if(r > 0.01 and r < 0.04):
        rank_number = 0
    return rank_number
    # 0	 : 0.015222915448248386
    # 1	 : 0.048723580315709114
    # 2	 : 0.06371929943561555
    # 3	 : 0.07859582751989364
    # 4	 : 0.09529682248830795
    # 5	 : 0.11306775361299515
    # 6	 : 0.13043687045574187
    # 7	 : 0.14534296691417692
    # 8	 : 0.16037331521511078
    # 9	 : 0.1815473824739456

@app.route('/get_score/')
def api_get():
    global connection
    try:
        args_dict  = request.args.to_dict()
        print(args_dict)
        id = ""
        mob  = args_dict["mobile"]
        url = 'http://178.128.100.101:9010/profile_data/get?id='+id+'&mobile='+mob
        # url = 'http://178.128.100.101:9010/profile_data/get?id=13229814&mobile=841639401222'
        headers = {'content-type': 'application/json','token': 'kalapa'}
        r = requests.get(url,headers=headers,timeout=5)
        data = r.json()
        # print(data)

        #['arpu', 'home', 'friend_education_level_0', 'friend_education_level_1',
        #   'friend_education_level_2', 'friend_education_level_3',
        #   'friend_pay_first', 'credit_score', 'age.1', 'end_date']

        arpu = float(data["arp"])
        home = float(le_home.transform([data["home"]]))
        f0 = float(data["score"]["friend_education_level_0"])
        f1 = float(data["score"]["friend_education_level_1"])
        f2 = float(data["score"]["friend_education_level_2"])
        f3 = float(data["score"]["friend_education_level_3"])
        fp = float(data["score"]["friend_pay_first"])
        cs = float(data["score"]["credit_score"])
        age = float(data["age"])
        end_date = float(2019 - int(data["subInfo"]["register_date"][:4]) )

        X = array( [arpu,home,f0,f1,f2,f3,fp,cs,age,end_date] )
        pred = regressor.predict_proba([X])[0]

        # print(pred)

        #handle  data, encode it and pass to predictor regressor

        headers = request.headers
        auth = headers.get("token")

        data = {
            'score'  : str(pred[1]),
            'rank' : str(rank_convert(pred[1]))
        }

        js = json.dumps(data)

        if auth == 'kalapa':
            resp = jsonify(data)
            resp.status_code = 200
        else:
            resp = jsonify({"message": "ERROR: Unauthorized"})
            resp.status_code = 401

        try:
            sql_insert_query = """ INSERT INTO `tasks` (`id`, `score`,`start_date`) VALUES ('%s','%s',CURDATE())"""
            cursor = connection.cursor()
            result  = cursor.execute(sql_insert_query % (id,pred[1]))
            connection.commit()
            print(("Record inserted successfully into table"))
        except Exception as e:
            print(e)
            print(("RECONNECT MYSQL"))
            connection = mysql.connector.connect(host='localhost',database='db_dcredit',user='dcre',password='dcredit@2019')
            sql_insert_query = """ INSERT INTO `tasks` (`id`, `score`,`start_date`) VALUES ('%s','%s',CURDATE())"""
            cursor = connection.cursor()
            result  = cursor.execute(sql_insert_query % (id,pred[1]))
            connection.commit()
            print(("Record inserted successfully into table"))

        return resp
    except Exception as e:
        data = {
            'score'  : str(-1),
            'rank' : str(-1)
        }
        resp = jsonify(data)
        resp.status_code = 200
        return resp


@app.route('/get_sal/')
def api_get_sal():
    global connection
    try:
        args_dict  = request.args.to_dict()
        print(args_dict)
        id = args_dict["id"]
        mob  = args_dict["mobile"]
        url = 'http://178.128.100.101:9010/profile_data/get?id='+id+'&mobile='+mob
        headers = {'content-type': 'application/json','token': 'kalapa'}
        r = requests.get(url,headers=headers,timeout=5)
        data = r.json()

        comment_avg = float(0)
        med_code = float(0)

        sal_main = 0
        sal_by_fb = 0
        sal_by_id = 0

        def phone_convert(phone):
            p = phone
            if(phone.startswith("9") or phone.startswith("3")):
                p = "84" + phone
            if(phone.startswith("03")):
                p = phone.replace("03","8416",1)
            if(phone.startswith("09")):
                p = phone.replace("09","849",1)
            return p

        try:
            sal_main = int(df_sal[df_sal["phone"] == int(phone_convert(mob))]["sal"])
            # sal_by_fb = sal_main
        except Exception as e:
            print(e)

        if(1 == 1):
            try:
                comment_avg = float(data["score"]["comment_avg"])
                X = array( [comment_avg] )
                sal_by_fb = regressor1.predict([X])[0]
            except Exception as e:
                print(e)

            try:
                med_code = float(le_l.transform([data["careerStatus"]["medicalInsurance"][:2]]))
                X = array( [med_code] )
                sal_by_id = regressor0.predict([X])[0]
            except Exception as e:
                print(e)


        headers = request.headers
        auth = headers.get("token")

        data = {
            'sal_main' : str(sal_main),
            'sal_by_id'  : str(sal_by_id),
            'sal_by_fb' : str(sal_by_fb)
        }

        js = json.dumps(data)

        if auth == 'kalapa':
            resp = jsonify(data)
            resp.status_code = 200
        else:
            resp = jsonify({"message": "ERROR: Unauthorized"})
            resp.status_code = 401
        #
        # try:
        #     sql_insert_query = """ INSERT INTO `tasks` (`id`, `score`,`start_date`) VALUES ('%s','%s',CURDATE())"""
        #     cursor = connection.cursor()
        #     result  = cursor.execute(sql_insert_query % (id,pred[1]))
        #     connection.commit()
        #     print(("Record inserted successfully into table"))
        # except Exception as e:
        #     print(e)
        #     print(("RECONNECT MYSQL"))
        #     connection = mysql.connector.connect(host='localhost',database='db_dcredit',user='dcre',password='dcredit@2019')
        #     sql_insert_query = """ INSERT INTO `tasks` (`id`, `score`,`start_date`) VALUES ('%s','%s',CURDATE())"""
        #     cursor = connection.cursor()
        #     result  = cursor.execute(sql_insert_query % (id,pred[1]))
        #     connection.commit()
        #     print(("Record inserted successfully into table"))

        return resp
    except Exception as e:
        print(e)
        print("Something went wrong")


if __name__ == '__main__':
    http_server = WSGIServer(('', 5000), app)
    http_server.serve_forever()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
