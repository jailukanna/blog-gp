---
title: mysql example crash report handler (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'crash report handler'

Functions in program: 
* `def main():`
* `def update_all_derived_data():`
* `def update_derived_data(report_uuid):`
* `def get_dd_callstack(report_uuid):`
* `def get_dd_source_context(report_uuid):`
* `def get_dd_source_file(report_uuid):`
* `def get_dd_computer(report_uuid):`
* `def get_dd_user(report_uuid):`
* `def get_dd_game_version(report_uuid):`
* `def get_first_match_from_file(report_uuid, filename, regex, flags=re.MULTILINE):`
* `def get_file_by_name(report_uuid, filename):`
* `def get_filenames(report_uuid):`
* `def get_report_by_id(report_uuid):`
* `def get_all_report_ids():`
* `def get_file_id(file, report_uuid):`
* `def save_file(file, report_uuid):`
* `def load_file_from_storage(file, report_uuid):`
* `def save_file_to_storage(file, report_uuid):`
* `def save_report(report):`
* `def create_report(report):`
* `def report_fields():`
* `def on_crash_created(raw_data):`
* `def human_bytes(num, suffix='B'):`
* `def read_uploaded_files(compressed_data):`
* `def read_compressed_file(data):`

Modules used in program: 
* `import sys`
* `import os`
* `import re`
* `import logging`
* `import struct`
* `import io`
* `import zlib`
* `import base64`
* `import json`
* `import mysql.connector`
* `import pprint`
* `import redis`

## python crash report handler

Python mysql example: crash report handler

```python

import redis
from daemon import DaemonContext
import pprint
import mysql.connector
import json
import base64
import zlib
import io
import struct
import logging
import re
import os
import sys


file_root = '/opt/crash-handler/files'

def read_compressed_file(data):
	current_file_index = struct.unpack("i", data.read(4))[0]
	#print(current_file_index)
	filename_len = struct.unpack("i", data.read(4))[0]
	#print(filename_len)
	fmt = "{}s".format(filename_len)
	filename = struct.unpack(fmt, data.read(filename_len))[0].split(b'\0', 1)[0]
	#print(filename)
	contents_len = struct.unpack("i", data.read(4))[0]
	#print(contents_len)
	fmt = "{}s".format(contents_len)
	contents = struct.unpack(fmt, data.read(contents_len))[0]

	#if filename[-3:] in ['wer', 'log', 'xml', 'txt']:
	#	print(">>>> {}".format(filename))
	#	print(contents)
	#	print("<<<<")

	#print("read {}".format(filename))

	file = {
		'filename' : filename,
		'contents' : contents,
	}
	return file

def read_uploaded_files(compressed_data):
	uncompressed_data = zlib.decompress(compressed_data)
	print("decompressed {} to {}".format(human_bytes(len(compressed_data)), human_bytes(len(uncompressed_data))))

	files = []
	data = io.BytesIO(uncompressed_data)
	while data.tell() < len(uncompressed_data):
		files.extend([read_compressed_file(data)])

	return files

def human_bytes(num, suffix='B'):
    for unit in ['','Ki','Mi','Gi','Ti','Pi','Ei','Zi']:
        if abs(num) < 1024.0:
            return "%3.1f%s%s" % (num, unit, suffix)
        num /= 1024.0
    return "%.1f%s%s" % (num, 'Yi', suffix)

def on_crash_created(raw_data):
	try:
		report = json.loads(raw_data)
		# make sure id is clean by only keeping hexadecimal characters
		report['uuid'] = re.sub(r'[^0-9a-f]', '', report['id'])
		report['id'] = None # we call it uuid
		decoded_data = base64.b64decode(report['payload'])
		files = read_uploaded_files(decoded_data)
		if create_report(report):
			for file in files:
				save_file(file, report['uuid'])
		update_derived_data(report['uuid'])
	except Exception, e:
		print(str(e))
		pass

def report_fields():
	return [
		'uuid',
		'reported_at',
		'reported_by',
		'user',
		'computer',
		'source_file',
		'source_context',
		'callstack',
		'game_version',
	]

def create_report(report):
	try:
		conn = mysql.connector.connect(user='root',
			password='supersecret',
			host='localhost',
			database='crash',
			connection_timeout=5)
		cursor = conn.cursor()
		fields = filter(lambda x : x in report, report_fields())
		query = ("INSERT INTO reports "
				"({}) "
				"VALUES ({})".format(
					", ".join(fields),
					", ".join(map(lambda x: "%({})s".format(x), fields))))
		print(query)
		cursor.execute(query, report)
		conn.commit()
		cursor.close()
		conn.close()

		print("report saved")
		return True
	except Exception, e:
		logging.exception('report not saved')
		if cursor:
			print(cursor.statement or 'no statement')
		return False

def save_report(report):
	try:
		conn = mysql.connector.connect(user='crash',
			password='supersecret',
			host='localhost',
			database='crash',
			connection_timeout=5)
		cursor = conn.cursor()
		fields = report_fields()
		query = ("UPDATE reports "
				"SET {} "
				"WHERE uuid = %(uuid)s "
				"LIMIT 1".format(
					", ".join(map(lambda x: "{0}=%({0})s".format(x), fields))))
		cursor.execute(query, report)
		conn.commit()
		cursor.close()
		conn.close()

		print("report saved")
		return True
	except Exception, e:
		logging.exception('report not saved')
		if cursor:
			print(cursor.statement or 'no statement')
		return False

def save_file_to_storage(file, report_uuid):
	root = file_root
	d = os.path.join(root, report_uuid)
	if not os.path.exists(d):
		os.mkdir(d)
	print("dir is {}".format(d))
	filepath = os.path.join(d, file['filename'])
	if not os.path.realpath(filepath).startswith(d):
		raise Exception("Path {} is not in directory {}, will not continue".format(filepath, d))
	with open(filepath, 'wb') as f:
		f.write(file['contents'])
	print("wrote {} to {}".format(human_bytes(len(file['filename'])), os.path.realpath(filepath)))

def load_file_from_storage(file, report_uuid):
	root = file_root
	filepath = os.path.join(root, report_uuid, file['filename'])
	if not os.path.exists(filepath):
		return None
	with open(filepath, 'rb') as f:
		file['contents'] = f.read()
	return file

def save_file(file, report_uuid):
	try:
		save_file_to_storage(file, report_uuid)

		conn = mysql.connector.connect(user='crash', password='supersecret', host='localhost', database='crash')
		cursor = conn.cursor()
		existing_id = get_file_id(file, report_uuid)
		if not existing_id:
			query = ("INSERT INTO files "
					"(report_uuid, filename, size) "
					"VALUES (%(report_uuid)s, %(filename)s, %(size)s)")
			data = {
				'report_uuid': report_uuid,
				'filename': file['filename'],
				'size': len(file['contents']),
			}
			cursor.execute(query, data)
			conn.commit()
		else:
			# file entry already exists, update it
			query = ("UPDATE files "
					"SET size = %(size)s "
					"WHERE id = %(id)s LIMIT 1")
			data = {
				'size': len(file['contents']),
				'id': existing_id,
			}
			cursor.execute(query, data)
			conn.commit()
		cursor.close()
		conn.close()

		print("file {} saved".format(file['filename']))
		return True
	except Exception, e:
		logging.exception('file not saved')
		if cursor:
			print(cursor.statement or 'no statement')
		return False

def get_file_id(file, report_uuid):
	"""returns row id if already exists in db"""
	try:
		conn = mysql.connector.connect(user='crash', password='supersecret', host='localhost', database='crash')
		cursor = conn.cursor()
		query = ("SELECT id FROM files "
				"WHERE report_uuid = %(report_uuid)s AND filename = %(filename)s")
		data = {
			'report_uuid': report_uuid,
			'filename': file['filename'],
		}
		cursor.execute(query, data)
		found_id = None
		row = cursor.fetchone()
		if row:
			found_id = row[0]
			print("{} for {} exists as id {}".format(file['filename'], report_uuid, found_id))
		cursor.close()
		conn.close()
		return found_id
	except Exception, e:
		logging.exception('unable to find file in db')
		return None

def get_all_report_ids():
	conn = mysql.connector.connect(user='crash', password='supersecret', host='localhost', database='crash')
	cursor = conn.cursor()
	ids = []
	try:
		query = ("SELECT uuid FROM reports "
				"ORDER BY reported_at DESC")
		cursor.execute(query)
		for (uuid,) in cursor:
			ids.extend([uuid])
	except Exception, e:
		logging.exception(e)
	finally:
		cursor.close()
		conn.close()
	return ids

def get_report_by_id(report_uuid):
	conn = mysql.connector.connect(user='crash', password='supersecret', host='localhost', database='crash')
	cursor = conn.cursor()
	report = None
	try:
		query = ("SELECT uuid, reported_by, reported_at, user, computer, source_file, source_context, callstack FROM reports "
				"WHERE uuid = %(uuid)s LIMIT 1")
		data = {'uuid': report_uuid}
		cursor.execute(query, data)
		row = cursor.fetchone()
		if row:
			(uuid, reported_by, reported_at, user, computer, source_file, source_context, callstack) = row
			report = {
				'uuid': uuid,
				'reported_by': reported_by,
				'reported_at': reported_at,
				'user': user,
				'computer': computer,
				'source_file': source_file,
				'source_context': source_context,
				'callstack': callstack,
			}
	except Exception, e:
		logging.exception(e)
	finally:
		cursor.close()
		conn.close()
	return report

def get_filenames(report_uuid):
	conn = mysql.connector.connect(user='crash', password='supersecret', host='localhost', database='crash')
	cursor = conn.cursor()
	filenames = []
	try:
		query = ("SELECT filename FROM files "
				"WHERE report_uuid = %(report_uuid)s ")
		data = {
			'report_uuid': report_uuid
		}
		cursor.execute(query, data)
		for (filename,) in cursor:
			filenames.extend([filename])
	except Exception, e:
		logging.exception(e)
	finally:
		cursor.close()
		conn.close()
	return filenames

def get_file_by_name(report_uuid, filename):
	filenames = get_filenames(report_uuid)
	for name in filenames:
		if name == filename:
			file = {'filename': name}
			file = load_file_from_storage(file, report_uuid)
			if file:
				return file['contents']
			else:
				return None
	return None

def get_first_match_from_file(report_uuid, filename, regex, flags=re.MULTILINE):
	file = get_file_by_name(report_uuid, filename)
	if file:
		m = re.search(regex, file, flags)
		if m and m.group(1):
			return m.group(1).strip()
	return None

def get_dd_game_version(report_uuid):
	return get_first_match_from_file(report_uuid, 'Diagnostics.txt', r'^Game version (.*)$')

def get_dd_user(report_uuid):
	return get_first_match_from_file(report_uuid, 'MyGame.log', r'.*LogInit: User: ([\w\\]*)')

def get_dd_computer(report_uuid):
	return get_first_match_from_file(report_uuid, 'MyGame.log', r'.*LogInit: Computer: ([\w\\]*)')

def get_dd_source_file(report_uuid):
	return get_first_match_from_file(report_uuid, 'Diagnostics.txt', r'^Source context from "(.*)"')

def get_dd_source_context(report_uuid):
	return get_first_match_from_file(report_uuid, 'Diagnostics.txt', r'<SOURCE START>(.*)<SOURCE END>', re.MULTILINE|re.DOTALL)

def get_dd_callstack(report_uuid):
	return get_first_match_from_file(report_uuid, 'Diagnostics.txt', r'<CALLSTACK START>(.*)<CALLSTACK END>', re.MULTILINE|re.DOTALL)

def update_derived_data(report_uuid):
	"""updates data saved in db but read from files, can be run again to refresh"""
	report = get_report_by_id(report_uuid)
	if not report:
		raise Exception("Unable to update derived data for uuid {}, report not found".format(report_uuid))
	report['game_version'] = get_dd_game_version(report_uuid)
	report['user'] = get_dd_user(report_uuid)
	report['computer'] = get_dd_computer(report_uuid)
	report['source_file'] = get_dd_source_file(report_uuid)
	report['source_context'] = get_dd_source_context(report_uuid)
	report['callstack'] = get_dd_callstack(report_uuid)
	save_report(report)

def update_all_derived_data():
	ids = get_all_report_ids()
	for id in ids:
		update_derived_data(id)

def main():
	r = redis.Redis(host='redis.example.com')
	pubsub = r.pubsub()
	try:
		pubsub.subscribe(['crash.created'])
		for item in pubsub.listen():
			if item['type'] == 'message':
				print("data size: {}".format(human_bytes(len(item['data']))))
				on_crash_created(item['data'])
	finally:
		pubsub.close()

if __name__ == '__main__':
	#with DaemonContext():
	if len(sys.argv) > 1:
		cmd = sys.argv[1]
		if cmd == 'update_all_derived_data':
			update_all_derived_data()
		else:
			print('unknown command {}'.format(cmd))
	else:
		main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
