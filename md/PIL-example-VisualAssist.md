---
title: PIL example VisualAssist (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'VisualAssist'

Functions in program: 
* `def to_cv(pil_image):`
* `def to_png(image):`
* `def to_pil(image):`
* `def highlight_faces(image, faces):`
* `def get_vision_service():`
* `def detect_face(image, max_results=4):`
* `def pitchyaw_to_xy(pitch, yaw):`
* `def rpy(self):`
* `def say(stuff):`

Modules used in program: 
* `import numpy`
* `import requests`
* `import math`
* `import base64`
* `import httplib2`
* `import cv2`

## python VisualAssist

Python PIL example: VisualAssist

```python
from __future__ import print_function
import cv2
from googleapiclient import discovery
import httplib2
from oauth2client.client import GoogleCredentials
import base64
from PIL import Image
from PIL import ImageDraw
from matplotlib import pyplot as plt
from os import system
from time import sleep
from myo import init, Hub, DeviceListener, Pose
import math
import requests
import numpy


def say(stuff):
    system('say -v Susan -r 160 ' + stuff)

MIN_MATCH_COUNT = 10

armx, army = None, None
call_next = False
sync_next = False

np = numpy

DISCOVERY_URL = 'https://{api}.googleapis.com/$discovery/rest?version={apiVersion}'
ANNOTATION_URL = 'https://vision.googleapis.com/v1/images:annotate?key=AIzaSyA4Vqgr14vsHx-XTEHyE1elOz4__5MnX80'


def rpy(self):
    """ Calculates the Roll, Pitch and Yaw of the Quaternion. """

    x, y, z, w = self.x, self.y, self.z, self.w
    roll = math.atan2(2 * y * w - 2 * x * z, 1 - 2 * y * y - 2 * z * z)
    pitch = math.atan2(2 * x * w - 2 * y * z, 1 - 2 * x * x - 2 * z * z)
    yaw = math.asin(2 * x * y + 2 * z * w)
    return (roll, pitch, yaw)


def pitchyaw_to_xy(pitch, yaw):
    return math.tan(yaw) * 500, math.tan(pitch) * 160

ref = None


class Listener(DeviceListener):

    def on_pair(self, myo, timestamp, firmware_version):
        print("Hello, Myo!")

    def on_unpair(self, myo, timestamp):
        print("Goodbye, Myo!")

    def on_orientation_data(self, myo, timestamp, quat):
        global ref, armx, army, sync_next
        if ref is None or sync_next:
            ref = rpy(quat)
            sync_next = False

        pitch, _, yaw = rpy(quat)
        dp, dy = pitch - ref[1], yaw - ref[2]
        armx, army = pitchyaw_to_xy(dp, dy)
        # print("Orientation:", quat.x, quat.y, quat.z, quat.w)

    def on_pose(self, myo, timestamp, pose):
        global call_next, sync_next
        if pose == Pose.double_tap:
            call_next = True
        if pose == Pose.wave_out:
            sync_next = True
        print(pose)
init('/Users/andy/Downloads/sdk/myo.framework')
listener = Listener()
hub = Hub()
hub.run(1000, listener)


def detect_face(image, max_results=4):
    """Uses the Vision API to detect faces in the given file.

    Args:
        face_file: A file-like object containing an image with faces.

    Returns:
        An array of dicts with information about the faces in the picture.
        {
                'type': 'FACE_DETECTION',
                'maxResults': max_results,
            },
    """
    # image_content = face_file.read()
    image_content = to_png(image)
    batch_request = {
        "requests": [{
            'image': {
                'content': base64.b64encode(image_content)
            },
            'features': [{
                "type": "LABEL_DETECTION",
                "maxResults": max_results
            }]
        }]
    }

    response = requests.post(ANNOTATION_URL, json=batch_request).json()
    ret = {}
    if response['responses'][0]:
        if 'faceAnnotations' in response['responses'][0]:
            ret["faces"] = response['responses'][0]['faceAnnotations']
        if 'labelAnnotations' in response['responses'][0]:
            ret["labels"] = response['responses'][0]['labelAnnotations']
        return ret

    return None


def get_vision_service():
    credentials = GoogleCredentials.get_application_default()
    return discovery.build('vision', 'v1', credentials=credentials,
                           discoveryServiceUrl=DISCOVERY_URL)


def highlight_faces(image, faces):
    """Draws a polygon around the faces, then saves to output_filename.

    Args:
      image: a file containing the image with the faces.
      faces: a list of faces found in the file. This should be in the format
          returned by the Vision API.
      output_filename: the name of the image file to be created,
          where the faces have polygons drawn around them.
    """
    # im = Image.open(image)
    im = to_pil(image)
    draw = ImageDraw.Draw(im)

    for face in faces:
        box = [(v['x'], v['y']) for v in face['fdBoundingPoly']['vertices']]
        draw.line(box + [box[0]], width=5, fill='#00ff00')

    del draw
    return to_cv(im)
# initialize the camera
cam = cv2.VideoCapture(0)   # 0 -> index of camera


def to_pil(image):
    return Image.fromarray(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))


def to_png(image):
    _, buffer = cv2.imencode('.png', image)
    return buffer


def to_cv(pil_image):
    return cv2.cvtColor(numpy.array(pil_image), cv2.COLOR_RGB2BGR)


emotions = ["joyLikelihood"]
loop = True
cv2.namedWindow("cam-test", cv2.CV_WINDOW_AUTOSIZE)
cv2.namedWindow("window", cv2.CV_WINDOW_AUTOSIZE)
dx = 0
dy = 0
speed = 5
while loop:
    _, img = cam.read()
    img = cv2.flip(img, 1)
    small = cv2.resize(img, (960, 540))
    mid = (960 / 2, 540 / 2)
    dx, dy = -int(armx), int(army)
    if dx > 320:
        dx = 320
    if dx < -320:
        dx = -320
    if dy > 150:
        dy = 150
    if dy < -150:
        dy = -150
    dy = 0
    upper_left = (mid[0] - 160 + dx, mid[1] - 270 + dy)
    lower_right = (mid[0] + 160 + dx, mid[1] + 270 + dy)
    sub = small[upper_left[1]:lower_right[1], upper_left[0]:lower_right[0]]
    cv2.imshow("window", sub)
    cv2.rectangle(small, upper_left, lower_right, (0, 255, 0), 3)
    cv2.imshow("cam-test", small)
    key = cv2.waitKey(16)
    # print(key)
    if key in range(63232, 63236):
        if key == 63232:
            dy -= speed
        if key == 63233:
            dy += speed
        if key == 63234:
            dx -= speed
        if key == 63235:
            dx += speed
    if key == ord('m'):
        hub.shutdown()
        exit()
    if key == ord('s'):
        sync_next = True
    if key == ord('c') or call_next:
        ret = detect_face(sub)
        if ret:
            if 'faces' in ret:
                faces = ret['faces']
                print('Found %s face%s' % (len(faces),
                                           '' if len(faces) == 1 else 's'))
                small = highlight_faces(small, faces)
                for face in faces:
                    for emo in emotions:
                        print(emo, face[emo])
            if 'labels' in ret:
                things = []
                for thing in ret['labels']:
                    things.append(thing["description"])
                output = ', or '.join(things)
                say(output)
        call_next = False

cv2.destroyWindow("cam-test")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
