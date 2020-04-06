---
title: mysql example wai (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'wai'


Modules used in program: 
* `import sys,os`

## python wai

Python mysql example: wai

```python
#!/usr/bin/env python
# -*- coding: utf8 -*- 


# ========= EDIT HERE ======================================================
config = {
  # Set the save path
	'save_path' : "C:\\VertrigoServ\\www",
	'folder_name' : 'dezvoltare',
	'title' : 'Dezvoltare',

	# Wordpress Admin
	'wp' : {
		'username' : 'admin',
		'password' : 'admin'
	},

	#mysql
	'mysql' : {
		'host'     : 'localhost',
		'database' : 'dezvoltare',
		'username' : 'root',
		'password' : 'vertrigo',
		'prefix'   : 'dzv_'
	}, 

	# Download Url
	'download' : "http://wordpress.org/latest.zip",

	# Install url
	'install' : 'http://localhost/%s'
}

# STOP EDITING HERE ==========================================================

import sys,os

try:
	import mysql.connector
except ImportError as e:
	print("")
	print("Failed to find MySQL Connector")
	print("Please download and install it from:")
	print("")
	print("http://dev.mysql.com/downloads/connector/python/")
	exit()

try:
	import shutil
except ImportError as e:
	print("Couldn't import shutil module")
	exit()

try:
	import zipfile
except ImportError as e:
	print("Can not import ZipFile module")
	exit()

try:
	import urllib2
	import urllib
except ImportError as e:
	print("Can not import urllib2 or urllib module")
	exit()


class PyPo:
	def __init__(self):
		self.prepare()
		self.download()
		self.unzip()
		self.createDatabase()
		self.moveFiles()
		self.checkServerActive()
		self.installWP()

		print("")
		print("")
		print("DONE")
		print("")
		print("If you see this message, that means everything should have worked")
		print("Check your site at:")
		print(config['install'] % config['folder_name'])


	def prepare(self):

		if not os.path.exists( os.path.join( config['save_path'], config['folder_name']) ):
			os.mkdir( os.path.join (config['save_path'], config['folder_name']) )

	def checkServerActive(self):
			try:
				# test for readme.html, other files trigger 500 error
				urllib2.urlopen( config['install'] % config['folder_name'] + "/readme.html" ) 
			except urllib2.HTTPError as err:
				print(err)
				print("Probably, the web server is not active")
				exit()

	def download(self):
		if not os.path.exists( os.path.join("." , "tmp") ):
			os.mkdir( os.path.join(".", "tmp"))

		os.chdir( os.path.join(".", "tmp"))

		filename = config['download'].split("/")[-1]
		wp = urllib2.urlopen(config['download'])
		wpzip = open( filename, 'wb')
		wpzip.write(  wp.read() )
		wpzip.close()

		self.zipfilename = filename

	def unzip(self):
		file = zipfile.ZipFile( self.zipfilename  , "r" )
		file.extractall( "." )
		file.close()

	def moveFiles(self):
		path = os.path.join(".", "wordpress")
		dest = os.path.join(config['save_path'], config['folder_name'])

		for file in os.listdir(path):
			src_file = os.path.join(path, file)
			dst_file = os.path.join(dest, file)
			try:
				shutil.move(src_file, dst_file)
			except Exception as e:
				print("")
				print("FAILED: " + str(e))
				print("")
	def createDatabase(self):
		self.cnx = mysql.connector.connect( user = config['mysql']['username'], password=config['mysql']['password'], host=config['mysql']['host'])
		self.cursor = self.cnx.cursor()
		try:
			self.cursor.execute("CREATE DATABASE {} DEFAULT CHARACTER SET 'utf8'".format(config['mysql']['database']))
		except mysql.connector.Error as e:
			print("Failed creating database: {}".format(e))
			exit()

	def installWP(self):

		# Setup Info
		post = {
			"dbname" : config['mysql']['database'],
			'uname'  : config['mysql']['username'],
			'pwd'    : config['mysql']['password'],
			'dbhost' : config['mysql']['host'],
			'prefix' : config['mysql']['prefix'],
			'submit' : "Submit"

		}
		print("Sending Database Information")
		request = urllib2.Request( config['install'] % config['folder_name'] + "/wp-admin/setup-config.php?step=2", None)
		response = urllib2.urlopen(request, urllib.urlencode(post) ) 
		#setup-config.php?step=2

		# Send the second request
		print("Sending Web-Site Information")
		post = {
			'weblog_title'   	: config['title'],
			'user_name' 		: config['wp']['username'],
			'admin_password'    : config['wp']['password'],
			'admin_password2'   : config['wp']['password'],
			'admin_email'       : 'info@localhost.com',
			'blog_public'       : 1,
			'submit' 			: 'Submit'
		}
		request = urllib2.Request( config['install'] % config['folder_name'] + "/wp-admin/install.php?step=2", None)
		response = urllib2.urlopen(request, urllib.urlencode(post) )

if __name__ == "__main__":
	app = PyPo()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
