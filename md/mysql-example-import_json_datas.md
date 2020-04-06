---
title: mysql example import json datas (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'import json datas'

Functions in program: 
* `def Import_Mysql(target_list,cursor,db):`
* `def Collect_List(target_file):`
* `def Connect_Mysql():`
* `def Move_File(log_dir,target_file):`
* `def Collect_Station_Log(log_dir):`
* `def Find_Date_Dir():`

Modules used in program: 
* `import datetime`
* `import pymysql`
* `import shutil`
* `import time`
* `import json`
* `import os`

## python import json datas

Python mysql example: import json datas

```python
#Sherlock

import os
import json
import time
import shutil
import pymysql
import datetime


def Find_Date_Dir():
     target_date_dir = []
     target_dir = os.path.expanduser('~') + '\\' + r'Desktop\QT'
     for sub_d in os.listdir(target_dir):
          sub_date = os.path.join(target_dir,sub_d)
          if not os.path.isdir(sub_date):
               pass
          else:
               sub_dir = os.path.join(target_dir,sub_date)
               target_date_dir.append(sub_dir)
     #print(target_date_dir)
     return target_date_dir

def Collect_Station_Log(log_dir):
     try:
          for sub_ in log_dir:
               for i in os.listdir(sub_):
                    target_log_dir = os.path.join(sub_,i)
                    if os.path.isfile(target_log_dir):
                         pass
                    elif os.path.isdir(target_log_dir):
                         for sub_log in os.listdir(target_log_dir):
                              sub_log_path = os.path.join(target_log_dir,sub_log)
                              shutil.move(sub_log_path,sub_)
                              print('Move file[{0}] to dir[{1}] is success!'.format(sub_log,sub_))
                    else:
                         print('There is an unknown type of file!Break program!')
                         break
                    try:
                         if os.path.exists(target_log_dir):
                              if os.path.isdir(target_log_dir):
                                   if len(os.listdir(target_log_dir)) == 0:
                                        os.rmdir(target_log_dir)
                    except Exception as e:
                         print(str(e))

     except Exception as e:
          print(str(e))

def Move_File(log_dir,target_file):
     try:
          for sub_ in log_dir:
               for i in os.listdir(sub_):
                    try:
                         sub_log = os.path.join(sub_,i)
                         fo = open(sub_log,'r+')
                         data_j = json.load(fo)
                         dut_id = data_j['dut_id']
                         station_id = data_j['station_id']
                         start_time = data_j['start_time_millis']
                         end_time = data_j['end_time_millis']
                         slot = data_j['slot']
                         outcome_phase = data_j['outcome']
                         project = data_j['metadata']['project']
                         build_phase = data_j['metadata']['build_phase']
                         try:
                              fi = open(target_file,'a+')
                              seq = [dut_id,' ',station_id,' ',start_time,' ',end_time,' ',slot,' ',outcome_phase,' ',project,' ',build_phase,'\n']
                              fi.writelines(seq)
                              print('Write data[{0}] to file[{1}] is success!'.format(sub_log,target_file))
                              fi.close()
                         except Exception as e:
                              print('Write data to file is failed!FailMsg[{0}]!'.format(str(e)))
                         fo.close()
                    except:
                         print('Jump over log[{0}]!'.format(sub_log))
                         fo.close()
                         pass
                    finally:
                         fo.close()
               print('************************************************************************************')
               print('************************************************************************************')
               print('Write data[{0}] to target file is success!'.format(sub_))
               print('************************************************************************************')
               print('************************************************************************************')
               print('Begin to remove path[{0}]!'.format(sub_))
               print('************************************************************************************')
               print('************************************************************************************')
               try:
                    if os.path.exists(sub_):
                         shutil.rmtree(sub_)
                         print('Remove file[{0}] is down!'.format(sub_))
               except Exception as e:
                    print('Remove file is failed!FailMsg[{0}]!'.format(str(e)))
          print('************************************************************************************')
          print('----------------------------Remove all file is success!----------------------------')
          print('************************************************************************************')
          print('************************************************************************************')
     except Exception as e:
          print(str(e))

def Connect_Mysql():
     config = {
     'host':'127.0.0.1',
     'port':3306,
     'user':'root',
     'password':'cbb19931119',
     'db':'sherlock',
     'charset':'utf8mb4',
     }
     db = pymysql.connect(**config)
     cursor = db.cursor()
     return db,cursor

def Collect_List(target_file):
     target_list = []
     try:
          fo = open(target_file,'r+')
          target_lines = fo.readlines()
          for sub_lines in target_lines:
               sub_line = sub_lines.split('\n')[0]
               target_list.append(sub_line)
     except Exception as e:
          print('Error append to list!')
     return target_list

def Import_Mysql(target_list,cursor,db):
     try:
          for sub_str in target_list:
               c_dut_id = sub_str.split(' ')[0]
               c_station_id = sub_str.split(' ')[1]
               c_start_time = sub_str.split(' ')[2]
               c_end_time = sub_str.split(' ')[3]
               c_slot = sub_str.split(' ')[4]
               c_outcome_phase = sub_str.split(' ')[5]
               c_project = sub_str.split(' ')[6]
               c_build_phase = sub_str.split(' ')[7]
               sql = "INSERT INTO datas (dut_id,station_id,start_time,end_time,slot,outcome_phase,project,build_phase)\
                      VALUES ('{0}','{1}','{2}','{3}','{4}','{5}','{6}','{7}')".format(c_dut_id,c_station_id,c_start_time,c_end_time,c_slot,c_outcome_phase,c_project,c_build_phase)
               try:
                    cursor.execute(sql)
                    db.commit()
                    print('Insert data[{0}] in mysql is success!'.format(sub_str))
               except Exception as e:
                    print('Insert data[{0}] in mysql is failed!ErrorMsg[{1}]!Now rollback!'.format(sub_str,str(e)))
                    db.rollback()
          cursor.close()
          db.close()
          print('----------------------------Import datas to mysql is success!----------------------------')
     except Exception as e:
          print('************************************************************************************')
          print('************************************************************************************')
          print('************************************************************************************')
          print('Insert datas to mysql is failed!ErrorMsg[{0}]'.format(str(e)))


if __name__ == '__main__':
     text_file_path = os.path.expanduser('~') + '\\' + r'Desktop\QT' + '\\' + 'flash.txt'
     l_dir = Find_Date_Dir()
     Collect_Station_Log(l_dir)
     Move_File(l_dir,text_file_path)
     db,cursor = Connect_Mysql()
     target_data = Collect_List(text_file_path)
     Import_Mysql(target_data,cursor,db)
     time.sleep(2)
     try:
          if os.path.exists(text_file_path):
               if os.path.isfile(text_file_path):
                    os.remove(text_file_path)
               else:
                    pass
          else:
               print('The file is not exists!')
     except Exception as e:
          print(str(e))
     time.sleep(1)
     print('************************************************************************************')
     print('************************************************************************************')
     print('----------------------------The program is finish!----------------------------')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
