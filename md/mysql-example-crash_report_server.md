---
title: mysql example crash report server (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'crash report server'

Functions in program: 
* `def signal_term_handler(signal, frame):`

Modules used in program: 
* `import logging`
* `import sys`
* `import signal`
* `import urlparse`
* `import datetime`
* `import base64`
* `import json`
* `import redis`
* `import os`
* `import io`
* `import struct`
* `import pprint`
* `import SocketServer`
* `import BaseHTTPServer`

## python crash report server

Python mysql example: crash report server

```python
import BaseHTTPServer
import SocketServer
import pprint
import struct
import io
import os
import redis
import json
import base64
import datetime
import urlparse
import signal
import sys
import logging

port = 8000
httpd = None

logger = logging.getLogger('crash_report_server')
logger.setLevel(logging.DEBUG)

ch = logging.FileHandler('/var/log/crash-report-server/crash-report-server.log')
ch.setLevel(logging.INFO)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
ch.setFormatter(formatter)
logger.addHandler(ch)

class CrashReport:

        def __init__(self, payload, reported_by, reported_at):
                self.payload = payload
                self.reported_by = reported_by
                self.reported_at = reported_at


class CrashReportJSONEncoder(json.JSONEncoder):

        def default(self, obj):
                if isinstance(obj, CrashReport):
                        return {
                                 'payload' : obj.payload,
                                 'reported_by' : obj.reported_by,
                                 'reported_at' : obj.reported_at,
                        }
                return json.JSONEncoder.default(self, obj)


class CrashReportRequestHandler(BaseHTTPServer.BaseHTTPRequestHandler):

        def __init__(self, request, client_address, server):
                BaseHTTPServer.BaseHTTPRequestHandler.__init__(self, request, client_address, server)

        def do_GET(self):
                self.send_response(200)
                self.send_header('Content-Length', 0)
                self.end_headers()

        def do_POST(self):
                path = urlparse.urlparse(self.path).path
                if path == '/CrashReporter/CheckReport':
                        self.handle_CheckReport()
                elif path == '/CrashReporter/CheckReportDetail':
                        self.handle_CheckReport()
                elif path == '/CrashReporter/UploadReportFile':
                        self.handle_UploadReportFile()
                else:
                        logger.info("unhandled request, responding with 200 to be nice")
                        self.send_response(200)
                        self.send_header('Content-Length', 0)
                        self.end_headers()

        def handle_CheckReport(self):
                # no actual checking going on
                # we need to read the body, otherwise we respond before whole request has been sent, confusing the client
                body = self.rfile.read(int(self.headers.getheader('Content-Length')))
                self.crashreporter_respond_success()

        def handle_UploadReportFile(self):
                compressed_data = self.rfile.read(int(self.headers.getheader('FileLength')))
                report = CrashReport(base64.b64encode(compressed_data), self.client_address[0], datetime.datetime.utcnow().isoformat())
                r = redis.client.StrictRedis(host='localhost', port=30000, socket_timeout=5)
                r.publish('crash.created', json.dumps(report, separators=(',', ':'), cls=CrashReportJSONEncoder))

                self.crashreporter_respond_success()

def signal_term_handler(signal, frame):
        try:
                logger.info('got sigterm, exiting...')
                if httpd:
                        httpd.server_close()
        except Exception:
                pass
        sys.exit(0)

if __name__ == '__main__':
        signal.signal(signal.SIGTERM, signal_term_handler)
        httpd = SocketServer.TCPServer(("", port), CrashReportRequestHandler)
        try:
                httpd.serve_forever()
        except KeyboardInterrupt:
                pass
        except Exception, e:
                logger.exception(e)
        httpd.server_close()
        logger.info("bye")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
