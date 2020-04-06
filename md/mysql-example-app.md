---
title: mysql example app (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'app'

Functions in program: 
* `def get_predict_prometheus():`
* `def get_api_traces():`
* `def get_prometheus_data():`
* `def load_agged_trace(start_ts: int, end_ts: int, sql=None):`
* `def get_prome_result(args):`
* `def hello_world():`

Modules used in program: 
* `import requests`
* `import pymysql`
* `import re`
* `import random`
* `import json`
* `import click`

## python app

Python mysql example: app

```python
import click

click.disable_unicode_literals_warning = True

import json
import random
import re
from concurrent.futures import ThreadPoolExecutor, as_completed

# from flaskext.mysql import MySQL
import pymysql
import requests
from flask import Flask, request, jsonify
from tqdm import tqdm

app = Flask(__name__)


# mysql = MySQL()
# mysql.init_app(app)


class Database:
    def __init__(self):
        host = "106.75.2.46"
        port = 4000
        user = "root"
        db = "mysql"

        self.con = pymysql.connect(host=host,
                                   port=port,
                                   user=user,
                                   db=db,
                                   cursorclass=pymysql.cursors.DictCursor)

        self.cursor = self.con.cursor()

    def save_metrics(self, metrics):
        for item in metrics:
            self.cursor.execute(
                'insert into my_metrics (timestamps, is_sys, data) values (%s, %s, %s)', (item[0], 1, json.dumps(item))
            )
            self.con.commit()


# db = Database()


@app.route('/')
def hello_world():
    return 'Hello World!'


def get_prome_result(args):
    start_time = args.get('start_time', 0)
    end_time = args.get('end_time', 0)
    query = args.get('query', "")

    res = requests.get(
        f'http://106.75.2.46:9090/api/v1/query_range?query={query}&start={start_time}&end={end_time}&step=14'
    )

    response_json = res.json()
    value_list = response_json['data']['result']
    return value_list


def load_agged_trace(start_ts: int, end_ts: int, sql=None):
    prometheus_result = get_prome_result(
        {'start_time': start_ts,
         'end_time': end_ts,
         'query': '100 - (avg by (instance) (irate(node_cpu_seconds_total{job="node_exporter",mode="idle"}[30s])) * 100)'
         }
    )

    traces = requests.get('http://106.75.2.46:16686/api/traces', params={
        'start': int(start_ts) * 1000000,
        'end': int(end_ts) * 1000000,
        'limit': 40,
        'lookback': 'custom',
        'maxDuration': '',
        'minDuration': '',
        'service': 'TiDB',
        'operation': 'session.runStmt',
    }).json()['data']

    new_traces = []
    for trace in traces:
        trace_ = {}
        trace_['sql'] = [s for s in trace['spans'] if s['operationName']
                         == 'session.runStmt'][0]['logs'][0]['fields'][0]['value']
        if sql is None:
            sql = trace_['sql']

        t = 0
        if re.search('select id from my_user where id=(.*)', sql):
            t = 1
        elif re.search('select count\(\*\) from my_user where age=(.*)', sql):
            t = 2
        elif re.search('insert (.*)', sql):
            t = 3
        trace_['sql_type'] = t
        for s in trace['spans']:
            trace_[s['operationName']] = s['duration']
        new_traces.append(trace_)

    def agg_traces(traces, sql_type):
        traces = [t for t in new_traces if t['sql_type'] == sql_type]
        traces_ = {
            'sql_type': sql_type,
            'sample': random.choice(traces),
        }
        for k in traces[0]:
            if type(traces[0][k]) == int:
                l = [t.get(k, 0) for t in traces]
                traces_[k] = sum(l) / len(l)

        traces_['cpu'] = float(prometheus_result[0]['values'][0][1])
        return traces_

    return agg_traces(new_traces, 1)


@app.route('/get_prometheus_data')
def get_prometheus_data():
    args = request.args.to_dict()
    return jsonify(get_prome_result(args))


@app.route('/api/traces')
def get_api_traces():
    args = request.args.to_dict()
    traces = requests.get('http://106.75.2.46:16686/api/traces', params=args).json()
    return jsonify(traces)


@app.route('/get_predict_prometheus')
def get_predict_prometheus():
    args = request.args.to_dict()
    start_time: int = int(args.get('start_time', 0))
    # end_time = args.get('end_time', 0)
    query = args.get('type', 'tracing')
    metric_type = args.get('metric_type', 1)
    sql = args.get('sql', '')

    STEPS = 10
    INTERVAL = 10
    result = [None for x in range(STEPS)]
    timeseries = [start_time + i * 10 for i in range(STEPS)]

    with ThreadPoolExecutor(max_workers=10) as executor:
        # Start the load operations and mark each future with its URL
        future_to_url = {executor.submit(
            load_agged_trace, start_time + i * INTERVAL, start_time + i * INTERVAL + INTERVAL, sql): i for i in
                         range(STEPS)}
        for future in tqdm(as_completed(future_to_url)):
            i = future_to_url[future]
            try:
                result[i] = future.result()
            except Exception as exc:
                print(exc)
                pass

    fields = []
    if metric_type == '0':
        fields = ['cpu', 'session.runStmt', 'session.getTxnFuture', 'session.ParseSQL', 'executor.Compile',
                  'session.CommitTxn',
                  'pdclient.processTSORequests', 'session.Execute', 'GetTSAsync', 'executor.Next',
                  'server.dispatch']
    elif metric_type == '1':
        fields = ['cpu', 'session.runStmt', 'session.getTxnFuture', 'session.ParseSQL', 'executor.Compile',
                  'session.CommitTxn', 'pdclient.processTSORequests', 'session.Execute', 'GetTSAsync',
                  'executor.Next', 'server.dispatch', '/tikvpb.Tikv/Coprocessor', 'tableReader.Next',
                  'streamAgg.Next']
    elif metric_type == '2':
        fields = ['cpu', 'session.runStmt', 'session.getTxnFuture', 'session.ParseSQL', 'executor.Compile',
                  'session.CommitTxn', 'pdclient.processTSORequests', 'session.Execute', 'GetTSAsync',
                  'executor.Next', 'server.dispatch', '/tikvpb.Tikv/Coprocessor', 'tableReader.Next',
                  'streamAgg.Next', 'executor.handleNoDelayExecutor', '/tikvpb.Tikv/KvCommit',
                  '/tikvpb.Tikv/KvPrewrite']

    res = []

    for field in fields:
        field_result = [item.get(field, []) for item in result if item is not None]
        res.append(field_result)

    return jsonify({'origin': res, 'prediction': [], 'fields': fields, 'timeseries': timeseries})


if __name__ == '__main__':
    app.run()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
