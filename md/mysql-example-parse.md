---
title: mysql example parse (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'parse'


Modules used in program: 
* `import datetime`
* `import mysql.connector`
* `import json`
* `import requests`
* `import os`
* `import sys`

## python parse

Python mysql example: parse

```python
import sys
import os
import requests
import json
import mysql.connector
import datetime

# grab environmental config and functions
from config import mysql_op
from parse_functions import uploadToS3, clearExpiredLinks, clearAssociatedPosts, stashTaggedUsers

##
## !! TODO: find a more graceful solution to update/replace/etc instead of clearAssociatedPosts() every time
##

# Connect with the MySQL Server
cnx = mysql.connector.connect(user=mysql_op['user'], password=mysql_op['password'],
                              host=mysql_op['host'],
                              port=mysql_op['port'],
                              database=mysql_op['database'])
cursor = cnx.cursor()
# Delete urls older than 36hrs
clearExpiredLinks(cursor, cnx)
# Get urls to process from the planolyurls table
select_urls = ("SELECT id as url_id, internal_owner_id, instagram_account_handle, planoly_api_key FROM planolyurls WHERE status = 0")
cursor.execute(select_urls)

for  (url_id, internal_owner_id, instagram_account_handle, planoly_api_key) in cursor:
    # Delete existing posts (probably should handles this better)
    clearAssociatedPosts(internal_owner_id, instagram_account_handle, cursor, cnx)

    # grab json data from planoly api and save to disk
    r = requests.get('https://api.planoly.com/report/schedules?code=' + planoly_api_key + '&status=1&tz=America/Los_Angeles')
    json_data = r.json()
    with open(planoly_api_key + '.json', 'w') as outfile:
        json.dump(json_data, outfile)

    # load from json file (assumed dict format)
    with open(planoly_api_key + '.json', 'r') as raw_data:
        parsed = json.load(raw_data)
        embedded = parsed["_embedded"]
        for items in embedded.values():
            for item in items:
                # don't touch multi, drafts or stories
                ismulti = item['isMulti']
                isdraft = item['isDraft']
                isstory = item['isStory']
                if (not ismulti and
                    not isdraft and
                    not isstory):
                    # tagged users may come in caption using ** as a deliminator. clean those, store those.
                    raw_caption = item['caption']
                    caption = raw_caption.split('** ')[0]
                    # list of users to tag
                    users_to_tag = raw_caption.split('** ')[1].replace(' ', '').replace('@', '').split(',')

                    schedule_date = datetime.datetime.strptime(item['scheduleDate'], "%Y-%m-%dT%H:%M:%S-%f") # 2017-06-27T08:30:00-0500 -> # 2017-06-27T08:30:00.050000
                    formatted_schedule_date = schedule_date.strftime('%Y-%m-%d %H:%M:%S')
                    image_name = item['cloudinaryId']
                    asset_pull_path = item['originalUrl']

                    print("\n" + caption + "\n")
                    # make sure we have this account already
                    #testAccount(internal_owner_id, planoly_id)

                    # insert into database
                    insert_post = ("INSERT INTO posts SET internal_owner_id = %s, instagram_account_handle = %s, schedule_date = DATE_SUB(%s, INTERVAL 2 HOUR), caption = %s, image_name = %s, created_at = NOW()")
                    
                    cursor.execute(insert_post, (internal_owner_id, instagram_account_handle, formatted_schedule_date, caption, image_name))
                    cnx.commit()

                    # now that we know the post id we can stash users to be tagged
                    post_id = cursor.lastrowid
                    stashTaggedUsers(internal_owner_id, instagram_account_handle, post_id, users_to_tag, cursor, cnx)

                    print("inserting...")
                    
                    # test if asset exists locally else download asset
                    if not os.path.isfile('../tmp/' + image_name):
                        print("downloading...")
                        img_data = requests.get(asset_pull_path).content
                        with open('../tmp/' + image_name, 'wb') as handler:
                            handler.write(img_data)
                        # send to S3
                        uploadToS3(internal_owner_id, image_name)

                        # do error handling here to ensure we actually grabbed an image
                        # if not os.path.isfile('image_storage/' + image_name):
                        #     update_post_error = ()
                    else:
                        print("already have a copy...")
    
    # Mark url status as processed

    update_status = ("UPDATE planolyurls SET status = 1 WHERE id = %s")
    cursor.execute(update_status, (url_id,))
    cnx.commit()

    cursor.close()
    cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
