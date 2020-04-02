---
title: pil example images2product (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'images2product'


Modules used in program: 
* `import time`
* `import sys`
* `import os.path`
* `import PIL.Image`

## python images2product

Python pil example: images2product

```python
#!/usr/bin/env python2.7

"""
Usage:
images2product.py  [Node Id] [delta] [image1] [image2] ... 
The sql script (fills ...) is written to the standard output.

~/images2product.py 361 363 0 $IMAGEDIR/* >test.sql
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


        def sql(self,nodeId,delta):
                sql = """
insert into files (uid,filename,filepath,filemime,filesize,status,timestamp) 
values('1','%s','sites/default/files/%s','%s','%s','1','%s');

set @fid = last_insert_id();

insert into filefield_meta(fid,width,height,duration,audio_format,audio_sample_rate,audio_channel_mode,audio_bitrate,audio_bitrate_mode,tags) 
values (@fid,%s,%s,0,'',0,'',0,'','a:0:{}');

INSERT INTO `content_field_image_cache`(vid,nid,delta,
    field_image_cache_fid,
    field_image_cache_list,
    field_image_cache_data)
 VALUES ((select max(vid) from node_revisions where nid=%d),%s,%s,@fid,1,concat('a:13:{s:11:\"description\";s:0:\"\";s:3:\"fid\";s:',length(@fid),':\"',@fid,'\";s:5:\"width\";i:%s;s:6:\"height\";i:%s;s:8:\"duration\";i:0;s:12:\"audio_format\";s:0:\"\";s:17:\"audio_sample_rate\";i:0;s:18:\"audio_channel_mode\";s:0:\"\";s:13:\"audio_bitrate\";i:0;s:18:\"audio_bitrate_mode\";s:0:\"\";s:4:\"tags\";a:0:{}s:3:\"alt\";s:0:\"\";s:5:\"title\";s:0:\"\";}'));
""" % (self.filename,self.filename,self.mime,self.filesize,int(time.time()),\
                        self.width,self.height,\
                        nodeId,nodeId,delta,self.width,self.height)
                
		return sql

if __name__ == '__main__':

	if len(sys.argv)<4:
                print(__doc__)
                exit(1)
        
        try:
                
                nodeId = int(sys.argv[1])
		delta = int(sys.argv[2])
		
        except ValueError:
                raise RuntimeError('nodeId, revisionId, delta must be integer')

        
        print('SET NAMES utf8;')
	print("""delete from content_field_image_cache)
	where nid=%d 
	and field_image_cache_fid IS NULL 
	and field_image_cache_list IS NULL 
	and field_image_cache_data IS NULL;
	""" % (nodeId,)

        for x in range(3,len(sys.argv)):
                try:
                        img = Img(sys.argv[x])
                except IOError:
                        raise RuntimeError("Can't open file "+sys.argv[x])

                print(img.sql(nodeId,delta))
                delta+=1



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
