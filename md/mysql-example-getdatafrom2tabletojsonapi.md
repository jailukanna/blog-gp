---
title: mysql example getdatafrom2tabletojsonapi (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'getdatafrom2tabletojsonapi'


Modules used in program: 
* `import mysql.connector`
* `import json`

## python getdatafrom2tabletojsonapi

Python mysql example: getdatafrom2tabletojsonapi

```python
#!flask/bin/python
# coding=utf-8
from flask import Flask, jsonify
import json
import mysql.connector
from mysql.connector import Error

app = Flask(__name__)

try:
    connection = mysql.connector.connect(host='127.0.0.1',database='bms_1',user='root',password='xxxxxx')
    if connection.is_connected():
       # ======object result======#
        object_sql = "select id,guid,tag,path,name,node_type,device_type,point_type,unit,link, alarm_level,alarm_type,percentage,abs_value,time from object;;"
        object_cursor = connection.cursor()
        object_cursor.execute(object_sql)
        object_result_list = object_cursor.fetchall()  # return sql result
        print("fields fetch result-->", object_result_list)  # is s list type, need to be a dict
        object_fields_list = object_cursor.description  # sql key name
        print("fields result -->", object_fields_list)
        object_cursor.close()

    # main part
    object_column_list = []
    for i in object_fields_list:
        object_column_list.append(i[0])
    print("object_colume_list-->",object_column_list)

    object_jsonData_list = []
    for objectrow in object_result_list:
        object_data_dict = {}
        for i in range(len(object_column_list)):
            object_data_dict[object_column_list[i]] = objectrow[i]
            #print("data_disc-->", data_dict)
        object_jsonData_list.append(object_data_dict)

    # ======space result======#
        space_sql = "select id,guid,tag,path,name,node_type,space_type,parent_guid,time from space;"
        space_cursor = connection.cursor()
        space_cursor.execute(space_sql)
        space_result_list = space_cursor.fetchall()
        space_fields_list = space_cursor.description
        space_cursor.close()

    space_column_list = []
    for space_header in space_fields_list:
        space_column_list.append(space_header[0])
    print("space clolume list -->",space_column_list)

    space_jsonData_list = []
    for spacerow in space_result_list:
        space_data_dict = {}
        for space_header in range(len(space_column_list)):
            space_data_dict[space_column_list[space_header]] = spacerow[space_header]
        print("space_data_dict-->", space_data_dict)
        space_jsonData_list.append(space_data_dict)


    @app.route('/north/getAll', methods=['GET'])
    def get_all():
        return jsonify({'space': space_jsonData_list, 'object': object_jsonData_list})


    @app.route('/north/getSpace', methods=['GET'])
    def get_space():
        return jsonify({'space': space_jsonData_list})

    @app.route('/north/getObject', methods=['GET'])
    def get_object():
        return jsonify({'object': object_jsonData_list})

    if __name__ == '__main__':
        app.run(host='127.0.0.1', port=5001, debug=True)


except Error as e:
    print("Error while connection to Mysql", e)
finally:
    connection.close()
    print("==== mysql closed===")













```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
