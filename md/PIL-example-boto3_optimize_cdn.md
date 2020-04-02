---
title: PIL example boto3 optimize cdn (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'boto3 optimize cdn'

Functions in program: 
* `def optimize_cdn_objects():`
* `def purge_unwanted_objects(obj):`
* `def upload_local_images(obj):`
* `def create_standard_res_image(obj):`
* `def create_retina_image(item):`
* `def save_images_locally(obj):`
* `def sanitize_object_key(obj):`
* `def get_objects_in_folder(folderpath):`
* `def get_folders():`

Modules used in program: 
* `import PIL`
* `import botocore`
* `import boto3`
* `import json`
* `import os`

## python boto3 optimize cdn

Python PIL example: boto3 optimize cdn

```python
import os
import json
import boto3
from botocore.client import Config
import botocore
from urllib.parse import unquote
import PIL


# Initialize a session using DigitalOcean Spaces.
session = boto3.session.Session()
client = session.client('s3',
                        region_name='nyc3',
                        endpoint_url='https://nyc3.digitaloceanspaces.com',
                        aws_access_key_id=os.environ.get('KEY'),
                        aws_secret_access_key=os.environ.get('SECRET'))


def get_folders():
    """Retrieve all folders within a specified directory.

    1. Set bucket name.
    2. Set delimiter (a character that our target files have in common).
    3. Set folder path to objects using "Prefix" attribute.
    4. Create list of all recursively discovered folder names.
    5. Return list of folders.
    """
    get_folder_objects = client.list_objects_v2(
        Bucket='hackers',
        Delimiter='',
        EncodingType='url',
        MaxKeys=1000,
        Prefix='posts/',
        ContinuationToken='',
        FetchOwner=False,
        StartAfter=''
        )
    folders = [item['Key'] for item in get_folder_objects['Contents']]
    return folders


def get_objects_in_folder(folderpath):
    """List all objects in the provided directory.

    1. Set bucket name.
    2. Leave delimiter blank to fetch all files.
    3. Set folder path to "folderpath" parameter.
    4. Return list of objects in folder.
    """
    objects = client.list_objects_v2(
        Bucket='hackers',
        EncodingType='url',
        MaxKeys=1000,
        Prefix=folderpath,
        ContinuationToken='',
        FetchOwner=False,
        StartAfter=''
        )
    return objects


def sanitize_object_key(obj):
    """Replace character encodings with actual characters."""
    new_key = unquote(unquote(obj))
    return new_key


def save_images_locally(obj):
    """Download target object.

    1. Try downloading the target object.
    2. If image doesn't exist, throw error.
    """
    try:
        client.download_file(Key=obj, Filename=obj, Bucket='hackers')
    except botocore.exceptions.ClientError as e:
        if e.response['Error']['Code'] == "404":
            print("The object does not exist.")
        else:
            raise


def create_retina_image(item):
    """Rename our file to specify that it is a Retina image.

    1. Insert "@2x" at end of filename.
    2. Copy original image with new filename.
    3. Keep both files as per retina.js.
    """
    indx = item.index('.')
    newname = item[:indx] + '@2x' + item[indx:]
    newname = sanitize_object_key(newname)
    client.copy_object(Bucket='hackers',
                       CopySource='hackers/' + item,
                       Key=newname)
    print("created: ", newname)


def create_standard_res_image(obj):
    """Resizes large images to an appropriate size.

    1. Set maximum bounds for standard-def image.
    2. Get the image file type.
    3. Open the local image.
    4. Resize image.
    5. Save the file locally.
    """
    size = 780, 2000
    indx = obj.index('/')
    filename = obj[indx:]
    filetype = filename.split('.')[-1].upper()
    filetype = filetype.replace('JPG', 'JPEG')
    outfile = obj.replace('@2x', '')
    print('created ', outfile, ' locally with filetype ', filetype)
    # Use PIL to resize image
    img = PIL.Image.open(obj)
    img.thumbnail(size, PIL.Image.ANTIALIAS)
    img.save(outfile, filetype, optimize=True, quality=100)


def upload_local_images(obj):
    """Upload standard def images created locally."""
    if '@2x' in obj:
        outfile = obj.replace('@2x', '')
        client.upload_file(Filename=outfile,
                           Bucket='hackers',
                           Key=outfile)
        print('uploaded: ', outfile)


def purge_unwanted_objects(obj):
    """Delete item from bucket if name meets criteria."""
    banned = ['Todds-iMac', 'conflicted', 'Lynx', 'psd', 'lynx']
    if any(x in obj for x in banned):
        client.delete_object(Bucket="hackers", Key=obj)
        return True
    # Determine if image is Lynx post
    filename = obj.split('/')[-1]
    if len(filename) < 7:
        sample = filename[:2]
        if int(sample):
            print(int(sample))
    return False


def optimize_cdn_objects():
    """Perform tasks on objects in our CDN.

    1. Loop through folders in subdirectory.
    2. In each folder, loop through all objects.
    3. Sanitize object key name.
    3. Remove 'garbage' files by recognizing what substrings they have.
    4. If file not deleted, check to see if file is an image (search for '.')
    5. Rename image to be retina compatible.
    6. Save image locally.
    7. Create standard resolution version of image locally.
    8. Upload standard resolution images to CDN.
    """
    for folder in get_folders():
        folderpath = sanitize_object_key(folder)
        objects = get_objects_in_folder(folderpath)
        for obj in objects['Contents']:
            item = sanitize_object_key(obj['Key'])
            purged = purge_unwanted_objects(item)
            if not purged:
                if '.' in item:
                    create_standard_res_image(item)
                    save_images_locally(item)
                    create_retina_image(item)
                    upload_local_images(item)


optimize_cdn_objects()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
