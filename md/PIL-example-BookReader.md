---
title: PIL example BookReader (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'BookReader'

Functions in program: 
* `def get_text_from_return(res_json):`
* `def google_cloud_vision (image_content):`

Modules used in program: 
* `import speech`
* `import photos`
* `import console`
* `import Image`
* `import io`
* `import base64`
* `import json`
* `import requests`

## python BookReader

Python PIL example: BookReader

```python
import requests
import json
import base64
import io
import Image
import console
import photos
import speech

GOOGLE_CLOUD_VISION_API_URL = 'https://vision.googleapis.com/v1/images:annotate?key='
API_KEY = "AIzaSyDXY3DpMvWawvOd133LJR0iqOXD7Vx1T5Y"

def google_cloud_vision (image_content):
	api_url = GOOGLE_CLOUD_VISION_API_URL + API_KEY
	req_body = json.dumps({
		"requests": [{
			"image":{
				"content": image_content
			},
			"features":[{
				"type":"TEXT_DETECTION",
				"maxResults":10
			}]
		}]
	})
	res = requests.post(api_url, data=req_body)
	return  res.json()
	
#jsonからtext部分を取り出す
def get_text_from_return(res_json):
	text = res_json["responses"][0]["fullTextAnnotation"]["text"]
	return text
	
console.alert("ページを写して","","OK!")
pil_img = photos.capture_image()

size = (int(pil_img.size[0]*0.2), int(pil_img.size[1]*0.2))
pil_img = pil_img.resize(size)

#空のインスタンスに保存し、バイナリデータを取得する
img = io.BytesIO()
pil_img.save(img, "JPEG")
img_byte = img.getvalue()

img_str = str(base64.b64encode(img_byte), "utf-8")

#cloud vision apiで文字を認識させてtextデータを取得する
res_json = google_cloud_vision(img_str)
text_json = get_text_from_return(res_json)

##内容の確認と読み上げ
print(text_json)
speech.say(text_json)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
