---
title: PIL example 2galleriasql (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example '2galleriasql'


Modules used in program: 
* `import time`
* `import sys`
* `import os.path`
* `import PIL.Image`

## python 2galleriasql

Python PIL example: 2galleriasql

```python
#!/usr/bin/env python2.7

"""
Usage:
files2galleriasql.py [Node Id] [Revision Id] [delta] [image1] [image2] ... 
The sql script (fills Drupal Galleria field) is written to the standard output.

Obtaining revision id from nid (node id):
select * from node_revisions where nid=[nid]\G

Obtaining revision id and last delta:
select vid,max(delta) from content_field_galleria where nid=[nid]\G

Empty galleria field: 
delta = 0, field_galleria_fid = NULL, field_galleria_list = NULL, field_galleria_data = NULL

Requires: Python Imaging Library (PIL) http://www.pythonware.com/products/pil/ (package python-imaging on Ubuntu)

Drupal path to the images: sites/default/files/
Drupal image cache:  sites/default/files/{imagefield_thumbs,imagecache/node-gallery-display} (generated automatically)



###############
Pre-generating imagecache (preset) images (galleria_teaser gallery_assist-default-thumbnail-100 node-gallery-display  medium nail imagefield_thumbs teaser_sticky)
###############

See attached Z Shell .zshrc file.

# imagefield_thumbs = gallery_assist-default-thumbnail-100
export IMAGEDIR=name_of_new_image_directory

mkdir "/var/www/sites/default/files/$IMAGEDIR"

cp *.JPG *.jpg *.png "/var/www/sites/default/files/$IMAGEDIR/"

for x in  galleria_teaser gallery_assist-default-thumbnail-100 node-gallery-display  medium nail imagefield_thumbs teaser_sticky ; do mkdir "$x" ; mkdir "$x/$IMAGEDIR" ; done

for x in galleria_teaser gallery_assist-default-thumbnail-100 node-gallery-display  medium nail teaser_sticky ; do for f in *.JPG *.jpg *.png ; do wget -O "$x/$IMAGEDIR/$f"  "http://localhost/sites/default/files/imagecache/$x/$IMAGEDIR/$f"; done; done

cp -r gallery_assist-default-thumbnail-100/*  imagefield_thumbs/

for x in  galleria_teaser gallery_assist-default-thumbnail-100 node-gallery-display  medium nail imagefield_thumbs teaser_sticky ; do ls -1 "$x/$IMAGEDIR" | wc --lines ; done

mkdir imagecache

mv galler* medium/ nail/ node-gallery-display/ teaser_sticky/ imagecache/

mkdir "$IMAGEDIR"

mv *.JPG *.jpg *.png "$IMAGEDIR"

~/files2galleriasql.py 361 363 0 $IMAGEDIR/* >test.sql

# Test environment: upload images and directories to the production environment (directory sites/default/files/)
"""

import PIL.Image
import os.path
import sys
import time


class Img():
	def __init__(self,f):
		self.image=PIL.Image.open(f)
		self.mime='image/'+self.image.format.lower()
		(self.width,self.height) = self.image.size

                self.image.fp.seek(0,2)
                self.filesize=self.image.fp.tell()

                self.filename = f


        def sql(self,nodeId,revisionId,delta):
                sql = """
insert into files (uid,filename,filepath,filemime,filesize,status,timestamp) 
values('1','%s','sites/default/files/%s','%s','%s','1','%s');

set @fid = last_insert_id();

insert into filefield_meta(fid,width,height,duration,audio_format,audio_sample_rate,audio_channel_mode,audio_bitrate,audio_bitrate_mode,tags) 
values (@fid,%s,%s,0,'',0,'',0,'','a:0:{}');

INSERT INTO `content_field_galleria`(vid,nid,delta,field_galleria_fid,field_galleria_list,field_galleria_data)
 VALUES (%s,%s,%s,@fid,1,concat('a:13:{s:11:\"description\";s:0:\"\";s:3:\"fid\";s:',length(@fid),':\"',@fid,'\";s:5:\"width\";i:%s;s:6:\"height\";i:%s;s:8:\"duration\";i:0;s:12:\"audio_format\";s:0:\"\";s:17:\"audio_sample_rate\";i:0;s:18:\"audio_channel_mode\";s:0:\"\";s:13:\"audio_bitrate\";i:0;s:18:\"audio_bitrate_mode\";s:0:\"\";s:4:\"tags\";a:0:{}s:3:\"alt\";s:0:\"\";s:5:\"title\";s:0:\"\";}'));
""" % (self.filename,self.filename,self.mime,self.filesize,int(time.time()),\
                        self.width,self.height,\
                        revisionId,nodeId,delta,self.width,self.height)
                
		return sql

if __name__ == '__main__':

	if len(sys.argv)<5:
                print(__doc__)
                exit(1)
        
        try:
                
                nodeId = int(sys.argv[1])
		revisionId = int(sys.argv[2])
		delta = int(sys.argv[3])
		
        except ValueError:
                raise RuntimeError('nodeId, revisionId, delta must be integer')

        
        print('SET NAMES utf8;')
	print("""delete from content_field_galleria )
	where vid=%s and nid=%s 
	and field_galleria_fid IS NULL 
	and field_galleria_list IS NULL 
	and field_galleria_data IS NULL;
	""" % (revisionId,nodeId)

        for x in range(4,len(sys.argv)):
                try:
                        img = Img(sys.argv[x])
                except IOError:
                        raise RuntimeError("Can't open file "+sys.argv[x])

                print(img.sql(nodeId,revisionId,delta))
                delta+=1



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
