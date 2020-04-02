---
title: pil example expressive (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'expressive'

Functions in program: 
* `def capture():`
* `def plot_polar(values, gender):`
* `def input_JP_text(name, img, text, x, y, size, color, display):`
* `def scoring():`

Modules used in program: 
* `import shutil`
* `import os`
* `import cv2`
* `import collections`
* `import numpy as np`
* `import japanize_matplotlib`
* `import matplotlib.pyplot as plt`
* `import cognitive_face as CF`

## python expressive

Python pil example: expressive

```python
from PIL import ImageFont, ImageDraw, Image
import cognitive_face as CF
import matplotlib.pyplot as plt
import japanize_matplotlib
import numpy as np
import collections
import cv2
import os
import shutil

KEY = '********************************'
BASE_URL = 'https://japaneast.api.cognitive.microsoft.com/face/v1.0'

CF.Key.set(KEY)
CF.BaseUrl.set(BASE_URL)

cascade_file = '/usr/local/share/OpenCV/lbpcascades/lbpcascade_frontalface.xml'
cascade = cv2.CascadeClassifier(cascade_file)

img_url = 'photo.png'

# 設定パラメータ
ESC_KEY = 27
ENTER_KEY = 13
WINDOW_NAME = "Facial muscle test"
WINDOW_WIDTH = 1024
WINDOW_HEIGHT = 768
GUIDE_WINDOW_NAME = "Guide"
GUIDE_WINDOW_WIDTH = 728
GUIDE_WINDOW_HEIGHT = 800
ANIMATION_FRAME = 40


# ベース画像の準備
base = np.zeros((GUIDE_WINDOW_HEIGHT, GUIDE_WINDOW_WIDTH, 3), dtype=np.uint8)
base.fill(255)

dark = np.zeros((WINDOW_HEIGHT, WINDOW_WIDTH, 3), dtype=np.uint8)
dark.fill(156)


def scoring():
	age = 0
	gender = []
	score = []

	# FaceAPIで取得する感情
	attributes = ['anger', 'contempt', 'disgust', 'fear', 'smile', 'happiness', 'neutral', 'sadness', 'surprise']

	# 感情を提示し、それに応じた写真を送信
	for face in attributes:
		print(face)

		faces = []
		while faces == []:
			# ガイドの画像を表示
			cv2.imshow(GUIDE_WINDOW_NAME, cv2.resize(cv2.imread('img/{}.png'.format(face)), (GUIDE_WINDOW_WIDTH, GUIDE_WINDOW_HEIGHT)))
			
			# カメラ起動
			capture()

			# FaceAPIに投げる
			faces = CF.face.detect(img_url, face_id=True, landmarks=False, attributes='age,gender,smile,emotion')

			# 顔を認識できなかったときはやり直す
			if faces == []:
				cv2.imshow(GUIDE_WINDOW_NAME, fail)
				cv2.waitKey(0)


		# 顔を2つ以上認識したとき
		if len(faces) >= 2:
			face_width = 0
			for kao in faces:
				if kao['faceRectangle']['width'] > face_width:
					face_width = kao['faceRectangle']['width']
					temp_face = kao

			# 写真内で一番大きい顔を選択
			try:
				faces = [temp_face]
				print('choice')
			except:
				print('error')
				face = [faces[0]]

		
		# 笑顔のときだけ別処理
		if face == 'smile':
			score.append(round(faces[0]['faceAttributes']['smile'] * 100))
			print('{}\n'.format(faces[0]['faceAttributes']['smile']))
		else:
			score.append(round(faces[0]['faceAttributes']['emotion'][face] * 100))
			print('{}\n'.format(faces[0]['faceAttributes']['emotion']))

		age += faces[0]['faceAttributes']['age']
		gender.append(faces[0]['faceAttributes']['gender'])


	# 顔写真を削除
	os.remove('photo.png')

	# 精度向上の為に9回の平均値を取る
	age = round(age // 9)

	# 精度向上の為に9回で最も多かった性別を選択する
	gender = collections.Counter(gender).most_common()[0][0]

	return score, age, gender


def input_JP_text(name, img, text, x, y, size, color, display):
	fontpath = 'font/APJapanesefont.ttf'
	font = ImageFont.truetype(fontpath, size)

	img_pil = Image.fromarray(img)
	draw = ImageDraw.Draw(img_pil)

	draw.text((x, y), text, font=font, fill=color)
	output_img = np.array(img_pil)

	if display == 1:
		cv2.imshow(name, output_img)
		cv2.waitKey(1)

	return output_img


def plot_polar(values, gender):
	input_JP_text(WINDOW_NAME, dark, '採点中', 210, 270, 200, (0, 0, 0), 1)
	cv2.imshow(GUIDE_WINDOW_NAME, progress)
	cv2.waitKey(1)

	animation = []

	labels = ['怒り', '軽蔑', '嫌気', '恐怖', '笑顔', '幸福', '真顔', '悲しみ', '驚き']
	dummy_labels = ['', '', '', '', '', '', '', '', '']
	
	angles = np.linspace(0, 2 * np.pi, len(labels) + 1, endpoint=True)
	values = np.concatenate((values, [values[0]]))
	fig = plt.figure(figsize=(10.24, 7.68), dpi=100)
	ax = fig.add_subplot(111, polar=True)

	ax.set_thetagrids(angles[:-1] * 180 / np.pi, dummy_labels)
	ax.set_rgrids(np.linspace(0, 100, 6), size=20, color='gray')


	# ラベルの描画
	for idx, label in enumerate(labels):
		ax.text(angles[idx], 125, label, size=30, color='black', verticalalignment='center', horizontalalignment='center')


	# アニメーション作成
	for i in range(1, ANIMATION_FRAME+1):

		# 男性は青、女性は赤
		if gender == 'male':
			ax.fill(angles, values/ANIMATION_FRAME*i, 'b', alpha=0.05)
		elif gender == 'female':
			ax.fill(angles, values/ANIMATION_FRAME*i, 'm', alpha=0.05)


		# アニメーション用の画像を保存
		fig.savefig('anime/{}.png'.format(i))


		# 進行度を描画
		input_JP_text(GUIDE_WINDOW_NAME, progress, '{}%'.format(round(i/ANIMATION_FRAME*100)), 340, 430, 40, (0, 0, 0), 1)

	
	# 性別に応じて外枠を描画
	if gender == 'male':
		ax.plot(angles, values, 'bo-', linewidth=4)
	elif gender == 'female':
		ax.plot(angles, values, 'mo-', linewidth=4)


	# 点数を描画
	for idx, num in enumerate(values):
		ax.text(angles[idx], num+10, num, size=20, color='black', verticalalignment='center', horizontalalignment='center')


	# 完成したレーダーチャートを保存
	fig.savefig('rader.png')

	
	cv2.imshow(GUIDE_WINDOW_NAME, result)
	cv2.waitKey(1)
	plt.close(fig)


def capture():
	end_flag, c_frame = cap.read()

	while end_flag == True:
		img = c_frame
		img_gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
		face_list = cascade.detectMultiScale(img_gray, minSize=(100, 100))


		# 顔部分を赤枠で囲む
		for (x, y, w, h) in face_list:
			color = (0, 0, 225)
			pen_w = 3
			cv2.rectangle(c_frame, (x, y), (x+w, y+h), color, thickness = pen_w)


		# そのまま描画すると左右逆で混乱するので反転する
		c_frame = cv2.resize(cv2.flip(c_frame, 1), (WINDOW_WIDTH, WINDOW_HEIGHT))
		cv2.imshow(WINDOW_NAME, c_frame)
		key = cv2.waitKey(1)


		# Escでプログラム終了
		if key == ESC_KEY:
			cv2.destroyAllWindows()
			cap.release()
			print('exit')
			exit(0)


		# エンターキーで写真撮影
		elif key == ENTER_KEY:
			cv2.imwrite(img_url, c_frame)
			break

		end_flag, c_frame = cap.read()

	# 写真撮影から抜けるときはカメラを暗くする
	blended = cv2.addWeighted(c_frame, 0.4, dark, 0.3, 0)

	cv2.imshow(WINDOW_NAME, blended)
	cv2.imshow(GUIDE_WINDOW_NAME, progress)
	cv2.waitKey(1)

	

if __name__ == '__main__':
	# カメラとガイドの準備
	cap = cv2.VideoCapture(0)
	cv2.namedWindow(WINDOW_NAME)
	cv2.moveWindow(WINDOW_NAME, 0, 0)
	cv2.namedWindow(GUIDE_WINDOW_NAME)
	cv2.moveWindow(GUIDE_WINDOW_NAME, 1024, 0)

	
	# レーダーチャート検証用
	gender = 'male'
	score = [85, 78, 66, 100, 88, 64, 73, 82, 91]
	
	
	# ガイド用画像の準備
	progress = input_JP_text(GUIDE_WINDOW_NAME, base, '少々お待ちください。', 110, 330, 70, (0, 0, 0), 0)
	start = input_JP_text(GUIDE_WINDOW_NAME, base, '表情筋テスト', 110, 300, 100, (0, 0, 0), 0)
	start = input_JP_text(GUIDE_WINDOW_NAME, start, 'ボタンを押してください', 220, 420, 35, (0, 0, 0), 0)
	result = input_JP_text(GUIDE_WINDOW_NAME, base, '結果が出ました！', 150, 330, 70, (0, 0, 0), 0)
	result = input_JP_text(GUIDE_WINDOW_NAME, result, 'ボタンを押すと終了します', 220, 420, 30, (0, 0, 0), 0)
	standby = input_JP_text(WINDOW_NAME, dark, '待機中', 210, 250, 200, (0, 0, 0), 0)
	fail = input_JP_text(GUIDE_WINDOW_NAME, base, '顔を認識できませんでした', 130, 330, 50, (0, 0, 0), 0)
	fail = input_JP_text(GUIDE_WINDOW_NAME, fail, 'ボタンを押してください', 250, 420, 30, (0, 0, 0), 0)


	while True:
		# スタート画面
		cv2.imshow(GUIDE_WINDOW_NAME, start)
		input_JP_text(WINDOW_NAME, standby, 'ボタンを押してください', 290, 470, 50, (0, 0, 0), 1)
		
		key = cv2.waitKey(0)
		
		# Escでプログラム終了
		if key == ESC_KEY:
			break


		# 表情筋テスト開始
		score, age, gender = scoring()		


		# レーダーチャート作成
		plot_polar(score, gender)
		
		for i in range(ANIMATION_FRAME):
			cv2.imshow(WINDOW_NAME, cv2.imread('anime/{}.png'.format(i+1)))
			cv2.waitKey(1)

		cv2.imshow(WINDOW_NAME, cv2.imread('rader.png'))
		cv2.waitKey(0)

		
		# レーダーチャートを連番で保存
		files = os.listdir('rader')
		num = int(sorted(files, reverse=True)[0].replace('.png', ''))+1
		shutil.copy('rader.png', 'rader/{:04}.png'.format(num))
		

	# 終了処理
	cv2.destroyAllWindows()
	cap.release()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
