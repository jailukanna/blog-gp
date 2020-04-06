---
title: mysql example series data automation (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'series data automation'

Functions in program: 
* `def getResultRow(rlr, reviewerSettings, managerMap):`
* `def doAutomation(rlrs, reviewerSettings, managerMap):`
* `def generateCSV(rlrs, reviewerSettings, managerMap):`
* `def getManagers(companyIds):`
* `def getReviewerSettings(entityIds):`
* `def getDisassociatedRLRs(entityIdTuples):`
* `def getAllValidEntityIdTuples(inputs):`

Modules used in program: 
* `import base64`
* `import requests`
* `import urllib3`
* `import csv`
* `import sys`
* `import argparse`
* `import mysql.connector`
* `import yaml`

## python series data automation

Python mysql example: series data automation

```python
# Yaml file
# cname|series|user
# inputs:
#  - '775641624999276910|839529383773754046|3e3dacb02579eb99'

import yaml
import mysql.connector
from multiprocessing.pool import ThreadPool as Pool
from couchbase.cluster import Cluster, PasswordAuthenticator, Bucket
from couchbase.exceptions import CouchbaseError, NotFoundError
import argparse
import sys
import csv
import urllib3
import requests
import base64

http = urllib3.PoolManager()

inputs = {}
with open("series_data.yaml", 'r') as stream:
	try:
		inputs = yaml.safe_load(stream)['inputs']
	except yaml.YAMLError as exc:
		print(exc)


mydb = mysql.connector.connect(
  host="10.0.4.26",
  user="admin",
  passwd="admin123",
  database="coaching"
)

mycursor = mydb.cursor()

cbUrl = "cb5-node-1.internal.mindtickle.com:8091"
class Cb5Client:
    def __init__(self, cbUrl, bucket_name, username, password):
        cluster = Cluster('couchbase://' + cbUrl)
        authenticator = PasswordAuthenticator(username, password)
        cluster.authenticate(authenticator)
        self.cb = cluster.open_bucket(bucket_name)
    def getDocument(self, key):
        try:
            return self.cb.get(key).value
        except Exception as e:
            # print(e)
            return None

cb = Cb5Client(cbUrl, "ce", "couchbase1", "couchbase1")


def getAllValidEntityIdTuples(inputs):
	format_strings = ','.join(['%s'] * len(inputs))
	entityIds = []
	query = """SELECT distinct e.id, e.company_id from EntityLearner el join Entity e on e.id = el.entity_id 
	where e.state_value not in (5,6) and (e.company_id, el.series_id) in (%s)"""
	query = query % (format_strings)
	# print(query)
	tuples = []
	for input in inputs:
		txtSplit = input.split("|")
		t = (txtSplit[0],txtSplit[1])
		tuples.append(t)
	
	query = query % tuple(tuples)
	# print(query)
	# 5 is archived. 6 is deleted
	try:
		# mycursor.execute(query, (companyId, series_id))
		mycursor.execute(query)
		result = mycursor.fetchall()
		for row in result:
			entityIds.append((row[0],row[1]))
	except Exception as e:
		print(e)
		print("could not get entities for company")
	return entityIds

def getDisassociatedRLRs(entityIdTuples):
	format_strings = ','.join(['%s'] * len(entityIdTuples))
	query = """WITH latest_rlr AS (
	SELECT rlr.*, RANK() OVER (PARTITION BY user_id, entity_id, company_id, reviewer_index ORDER BY state ASC) AS rn
	FROM ReviewerLearnerRelationship AS rlr where (entity_id, company_id) IN (%s)
	)
	select distinct el.company_id, el.user_id, el.entity_id, el.series_id, latest_rlr.reviewer_index, latest_rlr.reviewer_id,  latest_rlr.state,
	-- m.manager_attribute, m.manager_id, m.active,
	   	u_u.state as learner_user_state, u_r.state as reviewer_user_state, l_u.state as learner_learner_state, l_r.state as reviewer_learner_state
	   	from EntityLearner el
	   	left join latest_rlr on el.entity_id = latest_rlr.entity_id and el.user_id = latest_rlr.user_id
    	-- left join Manager m on el.user_id = m.user_id and m.manager_attribute = 'a_1'
    	left join User u_u on latest_rlr.user_id = u_u.id
    	left join User u_r on latest_rlr.reviewer_id = u_r.id
    	left join Learner l_u on latest_rlr.user_id = l_u.user_id
    	left join Learner l_r on latest_rlr.reviewer_id = l_r.user_id
	where
    	el.state != 'DEACTIVATED' and (el.entity_id, el.company_id) IN (%s)
    	and ((latest_rlr.state = 'DISASSOCIATED' and latest_rlr.rn = 1 and latest_rlr.assignment_type in ('AUTO', 'AUTOMATED')) or latest_rlr.reviewer_id is null)
    	group by el.company_id, el.user_id, el.entity_id, el.series_id, latest_rlr.reviewer_index, latest_rlr.reviewer_id, latest_rlr.state, u_u.state, u_r.state, l_u.state, l_r.state"""
	query = query % (format_strings, format_strings)
	query = query % (tuple(entityIdTuples) + tuple(entityIdTuples))

	# print(query)
	try:
		mycursor.execute(query)
		result = mycursor.fetchall()
		# print(result)
		return result
	except Exception as e:
		print(e)
		print("could not get RLRs")


def getReviewerSettings(entityIds):
	response = {}
	for entityId in entityIds:
		cbResponse = cb.getDocument(entityId)
		if cbResponse is not None:
			reviewerSettings = cbResponse["reviewerSettings"]["reviewers"]
			for idx, reviewerSetting in enumerate(reviewerSettings):
				if reviewerSetting["assignmentRule"]["type"] == 3:
					response[(entityId, idx+1)] = ("Manager", reviewerSetting["assignmentRule"]["managerId"])
				elif reviewerSetting["assignmentRule"]["type"] == 2:
					response[(entityId, idx+1)] = ("Common", reviewerSetting["assignmentRule"]["commonReviewer"]["id"])
				else:
					response[(entityId, idx+1)] = ("AdHoc", "")
	return response

def getManagers(companyIds):
	format_strings = ','.join(['%s'] * len(companyIds))
	query = """SELECT user_id, manager_attribute, manager_id, active 
	from Manager where company_id IN (%s)"""
	query = query % (format_strings)
	# print(query)

	managerMap = {}
	try:
		mycursor.execute(query, (tuple(companyIds)))
		result = mycursor.fetchall()
		for row in result:
			managerMap[(row[0], row[1])] = (row[2], row[3])
		return managerMap
	except Exception as e:
		print((e))
		print("could not get managers")

def generateCSV(rlrs, reviewerSettings, managerMap):
	with open('output.csv', 'w', newline='') as file:
		writer = csv.writer(file)	
		writer.writerow(["Company Id", "User Id", "Entity Id", "Series Id", "Reviewer Index", "Reviewer Id", "Association State", "Reviewer Setting", "Common Reviewer", "Manager Key", "Manager Id", "Manager Active", "Learner Active", "Reviewer Active", "Issue"])
		for rlr in rlrs:
			row = getResultRow(rlr, reviewerSettings, managerMap)
			if row[14]: # issue
				writer.writerow(row)

def doAutomation(rlrs, reviewerSettings, managerMap):
	for rlr in rlrs:
		row = getResultRow(rlr, reviewerSettings, managerMap)
		if row[14]: # issue
			url = "http://cag.internal.prod-singapore.mindtickle.com/api/v1/cag/{companyId}/series/{seriesId}/entity/{entityId}/learner/{learnerId}".format(companyId=row[0], seriesId=row[3], entityId=row[2], learnerId=row[1])
			headers = '{"userId":"migrate", "companyId":"migrate", "orgId": "migrate","learnerId":"migrate","_req":"migrate","authId":"migrate"}'
			headerBytes = headers.encode('utf-8')
			headerBase64 = base64.b64encode(headerBytes)

			print(url)
			response = requests.get(url, headers={'X-TOKEN': headerBase64})

			json_response = response.json()
			print(json_response)


def getResultRow(rlr, reviewerSettings, managerMap):
	companyId = rlr[0]
	userId = rlr[1]
	entityId = rlr[2]
	seriesId = rlr[3]
	reviewerIndex = rlr[4]
	reviewerId = rlr[5]
	associationState = rlr[6]
	reviewerSetting = ""
	commonReviewer = ""
	managerKey = ""
	managerId = ""
	managerActive = ""
	try:
		if reviewerIndex is None:
			reviewerIndex = 1
		completeReviewerSetting = reviewerSettings[(entityId, reviewerIndex)]
		reviewerSetting = completeReviewerSetting[0]
		if reviewerSetting == "Common":
			commonReviewer = completeReviewerSetting[1]
		elif reviewerSetting == "Manager":
			managerKey = completeReviewerSetting[1]
			try:
				manager = managerMap[(userId, managerKey)]
				managerId = manager[0]
				managerActive = manager[1]
			except Exception as e:
				pass
	except Exception as e:
		pass
	learnerActive = False
	reviewerActive = False
	if rlr[7] != "DEACTIVATED" and rlr [8] != "DEACTIVATED":
		learnerActive = True
	if rlr[8] != "DEACTIVATED" and rlr [9] != "DEACTIVATED":
		reviewerActive = True
	issue = False
	if learnerActive and reviewerActive:
		if (reviewerSetting == "Common" and commonReviewer != "") or (reviewerSetting == "Manager" and managerKey != "" and managerId != "" and managerActive == 1):
			issue = True
	return [companyId, userId, entityId, seriesId, reviewerIndex, reviewerId, associationState, reviewerSetting, commonReviewer, managerKey, managerId, managerActive, learnerActive, reviewerActive, issue]
	# writer.writerow([companyId, userId, entityId, seriesId, reviewerIndex, reviewerId, associationState, reviewerSetting, commonReviewer, managerKey, managerId, managerActive, learnerActive, reviewerActive, issue])

entityIdTuples = getAllValidEntityIdTuples(inputs)
rlrs = getDisassociatedRLRs(entityIdTuples)

entityIds = []
companyIds = []
for entityIdTuple in entityIdTuples:
	entityIds.append(entityIdTuple[0])

for entityIdTuple in entityIdTuples:
	companyIds.append(entityIdTuple[1])

reviewerSettings = getReviewerSettings(entityIds)
managerMap = getManagers(companyIds)
# print(managerMap)


# generateCSV(rlrs, reviewerSettings, managerMap)
doAutomation(rlrs, reviewerSettings, managerMap)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
