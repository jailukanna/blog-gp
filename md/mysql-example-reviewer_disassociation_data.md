---
title: mysql example reviewer disassociation data (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'reviewer disassociation data'

Functions in program: 
* `def generateCSV(rlrs, reviewerSettings, managerMap):`
* `def getManagers(companyId):`
* `def getReviewerSettings(entityIds):`
* `def getDisassociatedRLRs(entityIds, companyId):`
* `def getAllValidEntities(companyId):`

Modules used in program: 
* `import csv`
* `import sys`
* `import argparse`
* `import mysql.connector`

## python reviewer disassociation data

Python mysql example: reviewer disassociation data

```python
import mysql.connector
from multiprocessing.pool import ThreadPool as Pool
from couchbase.cluster import Cluster, PasswordAuthenticator, Bucket
from couchbase.exceptions import CouchbaseError, NotFoundError
import argparse
import sys
import csv

# print("Usage: python3 script.py <company-id>")
# solved only for reviewer index 1
companyId = sys.argv[1]
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

def getAllValidEntities(companyId):
	entityIds = []
	query = """SELECT id FROM Entity
             WHERE company_id = %s AND state_value<>%s AND state_value<>%s"""
	# 5 is archived. 6 is deleted
	try:
		mycursor.execute(query, (companyId, "5", "6"))
		result = mycursor.fetchall()
		for row in result:
			entityIds.append(row[0])
	except Exception as e:
		print(e)
		print("could not get entities for company")
	return entityIds

def getDisassociatedRLRs(entityIds, companyId):
	format_strings = ','.join(['%s'] * len(entityIds))
	query = """WITH latest_rlr AS (
	SELECT rlr.*, RANK() OVER (PARTITION BY user_id, entity_id, company_id, reviewer_index ORDER BY state ASC) AS rn
	FROM ReviewerLearnerRelationship AS rlr where company_id = %s
	)
	select distinct latest_rlr.company_id, latest_rlr.user_id, latest_rlr.entity_id, latest_rlr.reviewer_index, latest_rlr.reviewer_id,  latest_rlr.state, 
	-- m.manager_attribute, m.manager_id, m.active,
	   	u_u.state as learner_user_state, u_r.state as reviewer_user_state, l_u.state as learner_learner_state, l_r.state as reviewer_learner_state
	   	from latest_rlr
	   	left join EntityLearner el on el.entity_id = latest_rlr.entity_id and el.user_id = latest_rlr.user_id
    	-- left join Manager m on el.user_id = m.user_id and m.manager_attribute = 'a_1'
    	left join User u_u on latest_rlr.user_id = u_u.id
    	left join User u_r on latest_rlr.reviewer_id = u_r.id
    	left join Learner l_u on latest_rlr.user_id = l_u.user_id
    	left join Learner l_r on latest_rlr.reviewer_id = l_r.user_id
	where
    	el.state != 'DEACTIVATED' and el.company_id = %s
    	and ((latest_rlr.state = 'DISASSOCIATED' and latest_rlr.rn = 1 and latest_rlr.assignment_type in ('AUTO', 'AUTOMATED')) or latest_rlr.reviewer_id is null)
    	and el.entity_id IN (%s) group by latest_rlr.company_id, latest_rlr.user_id, latest_rlr.entity_id, latest_rlr.reviewer_index, latest_rlr.reviewer_id, latest_rlr.state, u_u.state, u_r.state, l_u.state, l_r.state"""
	query = query % ("%s","%s",format_strings)
	try:
		mycursor.execute(query, (companyId, companyId) + (tuple(entityIds)))
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

def getManagers(companyId):
	query = """SELECT user_id, manager_attribute, manager_id, active 
	from Manager where company_id = %s""" % companyId
	managerMap = {}
	try:
		mycursor.execute(query)
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
		writer.writerow(["User Id", "Entity Id", "Reviewer Index", "Reviewer Id", "Association State", "Reviewer Setting", "Common Reviewer", "Manager Key", "Manager Id", "Manager Active", "Learner Active", "Reviewer Active", "Issue"])
		for rlr in rlrs:
			userId = rlr[1]
			entityId = rlr[2]
			reviewerIndex = rlr[3]
			reviewerId = rlr[4]
			associationState = rlr[5]
			reviewerSetting = ""
			commonReviewer = ""
			managerKey = ""
			managerId = ""
			managerActive = ""
			try:
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
			if rlr[6] != "DEACTIVATED" and rlr [8] != "DEACTIVATED":
				learnerActive = True
			if rlr[7] != "DEACTIVATED" and rlr [9] != "DEACTIVATED":
				reviewerActive = True
			issue = False
			if learnerActive and reviewerActive:
 				if (reviewerSetting == "Common" and commonReviewer != "") or (reviewerSetting == "Manager" and managerKey != "" and managerId != "" and managerActive == 1):
 					issue = True
			if issue:
				writer.writerow([userId, entityId, reviewerIndex, reviewerId, associationState, reviewerSetting, commonReviewer, managerKey, managerId, managerActive, learnerActive, reviewerActive, issue])


entityIds = getAllValidEntities(companyId)
# print(entityIds)
rlrs = getDisassociatedRLRs(entityIds, companyId)
# print(rlrs)
reviewerSettings = getReviewerSettings(entityIds)
# print(reviewerSettings)
managerMap = getManagers(companyId)
# print(managerMap)

generateCSV(rlrs, reviewerSettings, managerMap)




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
