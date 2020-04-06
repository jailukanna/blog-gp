---
title: mysql example parse functions (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'parse functions'

Functions in program: 
* `def stashTaggedUsers(internal_owner_id, instagram_account_handle, post_id, users_to_tag, cursor, cnx):`
* `def clearAssociatedPosts(internal_owner_id, instagram_account_handle, cursor, cnx):`
* `def clearExpiredLinks(cursor, cnx):`
* `def uploadToS3(internal_owner_id, image_name):`

## python parse functions

Python mysql example: parse functions

```python
# manage transfer to S3
def uploadToS3(internal_owner_id, image_name):
    import boto3

    s3 = boto3.resource('s3')
    data = open('../tmp/' + image_name, 'rb')
    s3.Bucket('soso.social').put_object(Key=str(internal_owner_id) + '/' + image_name, Body=data, ContentType='image/jpg')

# keep your workplace clean
def clearExpiredLinks(cursor, cnx):
    try:
        delete_expired = ("DELETE FROM planolyurls WHERE created_at < NOW() - INTERVAL 46 HOUR")
        cursor.execute(delete_expired)
        cnx.commit()
    except mysql.connector.Error as err:
        print("Something went wrong: {}".format(err))

# bandaid fix, delete all and reupload fresh
def clearAssociatedPosts(internal_owner_id, instagram_account_handle, cursor, cnx):
    try:
        delete_posts = ("DELETE FROM posts WHERE internal_owner_id = %s AND instagram_account_handle = %s")
        delete_tags = ("DELETE FROM tags WHERE internal_owner_id = %s AND instagram_account_handle = %s")
        cursor.execute(delete_posts, (internal_owner_id, instagram_account_handle,))
        cursor.execute(delete_tags, (internal_owner_id, instagram_account_handle,))
        cnx.commit()
    except mysql.connector.Error as err:
        print("Something went wrong: {}".format(err))


def stashTaggedUsers(internal_owner_id, instagram_account_handle, post_id, users_to_tag, cursor, cnx):
    for instagram_account_handle_to_tag in users_to_tag:
        insert = ("INSERT INTO tags SET internal_owner_id = %s, instagram_account_handle = %s, post_id = %s, instagram_account_handle_to_tag = %s, created_at = NOW()")
        cursor.execute(insert, (internal_owner_id, instagram_account_handle, post_id, instagram_account_handle_to_tag))
        cnx.commit()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
