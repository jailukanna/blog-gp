---
title: mysql example flask app (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'flask app'

Functions in program: 
* `def generateDebugGPSReading():`
* `def selectLatestGPSReadings(TrackerIDs, serverTimestamp=0.0):`
* `def selectGPSReadings(TrackerIDs, limit=100, serverTimestamp=0.0):`
* `def insertGPSReading(trackerID, lat, lng, gpss, gsms, bl, ps):`
* `def api_getLatestGPSReadings():`
* `def api_getGPSReadings():`
* `def api_addNewGPSReading():`
* `def submit_message():`
* `def hello_world():`

Modules used in program: 
* `import json`
* `import time, math, random`

## python flask app

Python mysql example: flask app

```python
# CREATE TABLE GPSReadings(GPSReadingID INTEGER PRIMARY KEY AUTO_INCREMENT, TrackerID VARCHAR(32), serverTimestamp REAL, Latitude REAL, Longitude REAL, GSMSignal INTEGER, GPSSignal INTEGER, BatteryLevel INTEGER, PowerStatus INTEGER);
# A very simple Flask Hello World app for you to get started with...
import time, math, random
import json
from pymysql import cursors
from flask import Flask, request, redirect
from flaskext.mysql import MySQL

app = Flask(__name__)
app.config["DEBUG"] = True


mysql = MySQL()

#mysql config
app.config['MYSQL_DATABASE_USER'] = 'dobrovv'
app.config['MYSQL_DATABASE_PASSWORD'] = 'mysql12345'
app.config['MYSQL_DATABASE_DB'] = 'dobrovv$default'
app.config['MYSQL_DATABASE_HOST'] = 'dobrovv.mysql.pythonanywhere-services.com'
mysql.init_app(app)

@app.route('/')
def hello_world():
    conn = mysql.connect()
    cursor = conn.cursor()
    cursor.execute("SELECT * from Messages")
    result = cursor.fetchall()
    cursor.close()

    homepage = 'Hello from <b>engr-390-r</b> project site<br/>'
    homepage += '<br/><form action="/" method="post"> New Message: <input type="text" name="newMessage" value=""><br><input type="submit" value="Submit"></form>'

    for message in result:
        homepage += f'<br/><b>Message {message[0]} </b>: {message[1]}'

    return homepage;

@app.route('/', methods=['post'])
def submit_message():
    newMessage = str(request.form['newMessage'])
    conn = mysql.connect()
    cursor = conn.cursor()
    cursor.execute(f"insert into Messages(msg) values (\"{newMessage}\");")
    conn.commit()
    cursor.close()
    return redirect("/", code=302)


# /addNewGPSReading?TrackerID=<TrackerID>&lat=<lat>&lng=<lng>
@app.route('/addNewGPSReading', methods=['get'])
def api_addNewGPSReading():
    result = "[]"
    trackerID = request.args.get('TrackerID')

    if trackerID is None or trackerID=="":
        return result

    try:
        lat = float(request.args.get('la'))
    except (ValueError, TypeError):
        lat = 0.0

    try:
        lng = float(request.args.get('lo'))
    except (ValueError, TypeError):
        lng = 0.0

    try:
        gpss = int(request.args.get('gpss'))
    except (ValueError, TypeError):
        gpss = 0

    try:
        gsms = int(request.args.get('gsms'))
    except (ValueError, TypeError):
        gsms = 0


    try:
        bl = int(request.args.get('bl'))
    except (ValueError, TypeError):
        bl = 0

    try:
        ps = int(request.args.get('ps'))
    except (ValueError, TypeError):
        ps = 0

    try:
        numRowsChanged = insertGPSReading(trackerID, lat, lng, gpss, gsms, bl, ps)
    except Exception:
        return result;

    try:
        result = selectLatestGPSReadings((trackerID,))
        result = json.dumps(result);
    except Exception:
        pass

    return result;


# /getGPSReadings?TrackerID=<trackerId1>&TrackerID=<trackerId2>...&limit=100&serverTimestamp=<timestamp>
@app.route('/getGPSReadings', methods=['get'])
def api_getGPSReadings():
    trackerIDs = request.args.getlist('TrackerID')
    try:
        limit = int(request.args.get('limit'))
        limit = 1 if limit <= 0 else limit
    except (ValueError, TypeError):
        limit = 100
    try:
        serverTimestamp = float(request.args.get('serverTimestamp'))
    except (ValueError, TypeError):
        serverTimestamp = 0.0

    try:
        result = selectGPSReadings(trackerIDs, limit, serverTimestamp)
        result = json.dumps(result);
    except Exception:
        result = "[]"

    return result;


@app.route('/getLatestGPSReadings', methods=['get'])
def api_getLatestGPSReadings():
    trackerIDs = request.args.getlist('TrackerID')
    try:
        limit = int(request.args.get('limit'))
        limit = 1 if limit == 0 else limit
    except (ValueError, TypeError):
        limit = 100
    try:
        serverTimestamp = float(request.args.getlist('serverTimestamp'))
    except (ValueError, TypeError):
        serverTimestamp = 0.0

    try:
        generateDebugGPSReading()
    except Exception:
        pass

    try:
        result = selectLatestGPSReadings(trackerIDs, serverTimestamp)
        result = json.dumps(result);
    except Exception:
        result = "[]"

    return result;


def insertGPSReading(trackerID, lat, lng, gpss, gsms, bl, ps):
    connection = mysql.connect()
    numRowsChanged = 0
    try:
        with connection.cursor() as cursor:
            sql = "INSERT INTO GPSReadings (TrackerID, serverTimestamp, Latitude, Longitude, GSMSignal, GPSSignal, BatteryLevel, PowerStatus) VALUES (%s, %s, %s, %s, %s, %s, %s, %s)"
            numRowsChanged = cursor.execute(sql, (trackerID, time.time(), lat, lng, gsms, gpss, bl, ps))
        connection.commit()
    finally:
        connection.close()

    return numRowsChanged


def selectGPSReadings(TrackerIDs, limit=100, serverTimestamp=0.0):
    connection = mysql.connect()
    try:
        with connection.cursor(cursors.DictCursor) as cursor:
            sql = "SELECT * FROM GPSReadings WHERE (TrackerID IN %s) AND (serverTimestamp > %s) ORDER BY serverTimestamp DESC LIMIT %s";
            cursor.execute(sql, (TrackerIDs, serverTimestamp, limit))
            result = cursor.fetchall()
        connection.commit()
    finally:
        connection.close()
    return result


def selectLatestGPSReadings(TrackerIDs, serverTimestamp=0.0):
    connection = mysql.connect()
    try:
        with connection.cursor(cursors.DictCursor) as cursor:
            sql = """SELECT * FROM GPSReadings
                WHERE TrackerID IN %s
                AND GPSReadingID IN (SELECT MAX(GPSReadingID) FROM GPSReadings GROUP BY TrackerID)
                AND serverTimestamp > %s;"""
            cursor.execute(sql, (TrackerIDs, serverTimestamp))
            result = cursor.fetchall()
        connection.commit()
    finally:
        connection.close()
    return result

DebugTrackers = { # latOfset lngOfset speed
    'debug1': [1e-3,  1e-3, 1e-5],
    'debug2': [1e-3, -1e-3, 1e-5],
    'debug3': [-1e-3, 1e-3, 1e-5],
    'debug4': [-1e-3, -1e-3, 1e-5],
    'debug5': [0.0,  1.1e-3, 1e-5],
}

def generateDebugGPSReading():
    trackerIDs = request.args.getlist('TrackerID')

    try:
        androidLat = float(request.args.get('androidLat'))
        androidLng = float(request.args.get('androidLng'))
    except (ValueError, TypeError):
        androidLat = 0.0
        androidLng = 0.0

    debugIDs   = set(trackerIDs).intersection(DebugTrackers.keys())

    if len(debugIDs) == 0:
        return

    did = random.choice(list(debugIDs))
    alpha = 2 * math.pi * random.random()
    DebugTrackers[did][0] += DebugTrackers[did][2] * math.sin(alpha)
    DebugTrackers[did][1] += DebugTrackers[did][2] * math.cos(alpha)
    lat = DebugTrackers[did][0] + androidLat
    lng = DebugTrackers[did][1] + androidLng

    try:
        insertGPSReading(did, lat, lng, random.randint(0,1), random.randint(0,30), random.randint(0,100), random.randint(0,1))
    except:
        pass




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
