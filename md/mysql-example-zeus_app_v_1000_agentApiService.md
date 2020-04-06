---
title: mysql example zeus app v 1000 agentApiService (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'zeus app v 1000 agentApiService'


Modules used in program: 
* `import json`
* `import hashlib`

## python zeus app v 1000 agentApiService

Python mysql example: zeus app v 1000 agentApiService

```python
# -*- coding:utf-8 -*-
import hashlib
import json

from flask.ext import restful
from flask.ext.restful import reqparse

from .errorCode import CustomFlaskErr, ErrorCode
from app_v_1000.DBUtilsTools import DBPool, DBSingle

dbPoolTool = DBPool()
errorCode = ErrorCode()
conn = dbPoolTool.getConnection()


class CommonApiService(restful.Resource):

    def __init__(self):
        self.getHandler = GetHandler()

    def get(self):
        """
        解析请求参数
        funName
        parameters
        """
        parser = reqparse.RequestParser()
        parser.add_argument('funName', type=str, required=True, help='Agent monitor mysql funName is must required')
        parser.add_argument('parameters', type=str, required=True, help='Agent monitor mysql parameters is must required')
        args = parser.parse_args()
        print("args="+str(args))
        funnName = args.get('funName')
        parameters = eval(args.get('parameters'))

        if funnName == 'getMysqlConf':
            ip = parameters['mysql_ip']
            port = parameters['mysql_port']
            print(str("解析请求API的参数:" + str(args)))

            apiData = self.getHandler.getMysqlConf(ip,port)
            print("apiData="+str(apiData))
        elif funnName == 'getMysqlManageInfo':
            ip = parameters['mysql_ip']
            port = parameters['mysql_port']
            print(str("解析请求API的参数:" + str(args)))

            apiData = self.getHandler.getMysqlManageInfo(ip, port)
            print("apiData=" + str(apiData))


        if apiData:
            jsonData = json.dumps(apiData)
            print(str("slowLogAgentApi返回数据:" + str(apiData)))
            return apiData, 200
        else:
            raise CustomFlaskErr(errorCode.MYSQL_NOT_CONF_IN_ZEUS['return_code'],
                                 errorCode.MYSQL_NOT_CONF_IN_ZEUS['status_code'])



class SlowLogApiService(restful.Resource):

    def __init__(self):
        self.getHandler = GetHandler()

    def post(self):
        parser = reqparse.RequestParser()
        parser.add_argument('post_data')
        parser.add_argument('instance_id', type=str)
        parser.add_argument('dataType',type=str)
        args = parser.parse_args()

        id = args.get('instance_id')
        data = args.get('post_data')
        dataType = args.get('dataType')
        dataList = json.loads(data, encoding='utf-8')
        print(id)
        print(dataType)
        print(dataList)
        for dict in dataList:
            self.getHandler.handler_fingerprint(conn, dict['fingerprint'], int(id))
            self.getHandler.insertSlowLog(conn, dict, id)

class PerformanceQuotaApiService(restful.Resource):

    def __init__(self):
        self.getHandler = GetHandler()

    def post(self):
        parser = reqparse.RequestParser()
        parser.add_argument('post_data')
        parser.add_argument('instance_id', type=str)
        parser.add_argument('dataType',type=str)
        args = parser.parse_args()

        instance_id = args.get('instance_id')
        data = args.get('post_data')
        dataType = args.get('dataType')
        dataDict = json.loads(data, encoding='utf-8')
        print(id)
        print(dataType)
        print(dataDict)
        #处理请求写入数据库
        self.getHandler.insertPerformanceQuota(conn, dataDict, instance_id)


class GetHandler:

    def getMysqlConf(self,ip,port):
        conf = {}
        poolCursor = conn.cursor()
        getMysqlIdSql="select id,mysql_host,mysql_port,manage_user,manage_password from mysql_instances where mysql_host='%s' and mysql_port=%s;" % (ip,port)
        print(getMysqlIdSql)
        poolCursor.execute(getMysqlIdSql)
        #获取 ID 和 管理账户
        result = poolCursor.fetchone()
        print(result)
        print(str("获取mysql id 结果集:"+str(result)))
        if result:
            mysqlId = result[0]
            mysqlHost = result[1]
            mysqlPort = result[2]
            manageUser = result[3]
            managePassword = result[4]

            connSingle = DBSingle(mysqlHost, int(mysqlPort), manageUser, managePassword)
            singleConn = connSingle.getConnection()
            singleCursor = singleConn.cursor()
            getSlowQueryLogSql = "show variables like 'slow_query_log';"
            singleCursor.execute(getSlowQueryLogSql)
            result = singleCursor.fetchone()
            print(str("获取mysql slow_query_log 结果集:" + str(result)))
            conf['slow_query_log'] = result[1]

            getSlowQueryLogFileSql = "show variables like 'slow_query_log_file';"
            singleCursor.execute(getSlowQueryLogFileSql)
            result = singleCursor.fetchone()
            print(str("获取mysql slow_query_log_file 结果集:" + str(result)))
            conf['slow_query_log_file'] = result[1]
            conf['id'] = mysqlId

            print(conf)
            return conf
        else:
            print("conf="+str(conf))
            return conf

    def md5string(self, fingerprint):
        m2 = hashlib.md5()
        m2.update(fingerprint)
        return m2.hexdigest()

    def handler_fingerprint(self, conn, fingerprint, instance_id):
        cursor = conn.cursor()
        md5string = self.md5string(fingerprint)
        # 通过md5值查看sql是否在fingerprint表中，如果在 直接写入slowlog表 不存在 先写入 fingerprint表
        cursor.execute("select id,instance_id,md5code from slow_query_fingerprint(where instance_id=" + str()
            instance_id) + " and md5code='" + md5string + "';")
        result = cursor.fetchone()
        if result:
            # 已存在 处理数据并返回TRUE
            cursor.execute(
                "update slow_query_fingerprint(set last_apper_time=now(),total_number=total_number+1 where id=" + str()
                    result[0]))
            cursor.execute("commit;")
            cursor.close()
        else:
            # 不存在
            #判断instance_id的mysql是否开启新增慢查询报警
            #if self.check_alter(self,conn,instance_id)['alter_slowlog']:
                #self.alter_slowlog_find_first(self, instance_id, schema, found_sql, conn)

            cursor.execute(
                "insert slow_query_fingerprint(instance_id,md5code,sql_fingerprint,first_apper_time,last_apper_time,total_number) values(" + str(
                    instance_id) + ",'" + str(md5string) + "','" + str(fingerprint) + "',now(),now(),1)")
            cursor.execute("commit;")
            cursor.close()

    def insertSlowLog(self, conn, event, instance_id):
        fingerprint_md5 = self.md5string(event['fingerprint'])
        cursor = conn.cursor()
        qtime = ''
        if event.has_key('time'):
            qtime = event['time']

        insert_sql = "insert slow_log(instance_id,query_time,last_errno,rows_examined,rows_sent," \
                     "log_timestamp,fingerprint_md5,bytes_sent,lock_time,killed," \
                     "cmd_user,query_sql,cmd_ip,query_date,cmd_schema) values" \
                     "(" + str(instance_id) + ",'" + str(event['query_time']) + "','" + str(
            event['last_errno']) + "','" + str(event['rows_examined']) + "'" \
                                                                         "," + str(event['rows_sent']) + "," + str(
            event['timestamp']) + ",'" + str(fingerprint_md5) + "'," + str(event['bytes_sent']) + "" \
                                                                                                  ",'" + str(
            event['lock_time']) + "'," + str(event['killed']) + ",'" + str(event['user']) + "','" + str(
            event['sql']) + "'" \
                            ",'" + str(event['ip']) + "','" + str(qtime) + "','" + str(
            event['schema']) + "')"
        print( insert_sql)
        cursor.execute(insert_sql)
        cursor.execute("commit;")
        cursor.close

    def getMysqlManageInfo(self,ip,port):
        conf = {}
        poolCursor = conn.cursor()
        getMysqlIdSql = "select id,mysql_host,mysql_port,manage_user,manage_password from mysql_instances where mysql_host='%s' and mysql_port=%s;" % (
        ip, port)
        print(getMysqlIdSql)
        poolCursor.execute(getMysqlIdSql)
        # 获取 ID 和 管理账户
        result = poolCursor.fetchone()
        print(str("获取mysql id 结果集:" + str(result)))
        if result:
            conf['mysqlId'] = result[0]
            conf['mysqlHost'] = result[1]
            conf['mysqlPort'] = result[2]
            conf['manageUser'] = result[3]
            conf['managePassword'] = result[4]
            return conf
        return conf

    def insertPerformanceQuota(self,conn,data,instance_id):
        insertSQL = "insert mysql_performance_quota(instance_id,create_time,com_select,com_delete,questions,com_insert,com_commit,com_rollback,com_update,qps,tps) values("\
                    +str(instance_id)+",'"+str(data['create_time'])+"',"+str(data['Com_select'])+","+str(data['Com_delete'])+","\
                    +str(data['Questions'])+","+str(data['Com_insert'])+","+str(data['Com_commit'])+","+str(data['Com_rollback'])+","\
                    +str(data['Com_update'])+","+str(data['Questions'])+","+str(data['Com_commit']+data['Com_rollback'])+");"
        print(insertSQL)
        cursor = conn.cursor()
        cursor.execute(insertSQL)
        cursor.execute("commit")
        cursor.close










if __name__ == '__main__':

    a = GetHandler()
    a.alter_slowlog_find_first(2,'zeus','select 1',conn)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
