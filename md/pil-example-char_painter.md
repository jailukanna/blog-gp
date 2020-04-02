---
title: pil example char painter (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'char painter'


Modules used in program: 
* `import matplotlib.font_manager as fm`
* `import matplotlib.pyplot as plot`
* `import matplotlib as mpl`
* `import platform`
* `import numpy as np`
* `import sys`

## python char painter

Python pil example: char painter

```python
import sys
import numpy as np
import platform

import matplotlib as mpl
mpl.use('Agg')
import matplotlib.pyplot as plot
import matplotlib.font_manager as fm

from PIL import Image, ImageFont, ImageDraw

class Painter():
	
	def __init__(self):

		self.platform_name = platform.system()

	def HELP_install_ttf_in_linux(self):
		
		sys.stderr.write('*** install ttf in linux\n\n')
		sys.stderr.write('1. cd /usr/share/fonts\n')
		sys.stderr.write('2. sudo wget http://cdn.naver.com/naver/NanumFont/fontfiles/NanumFont_TTF_ALL.zip\n')
		sys.stderr.write('3. sudo unzip NanumFont_TTF_ALL.zip -d truetype/NanumFont\n')
		sys.stderr.write('4. sudo rm -f NanumFont_TTF_ALL.zip\n')
		sys.stderr.write('\n')
		
	def softmax(self, x):
		return np.exp(x) / np.sum(np.exp(x),axis=0)

	def get_colormap(self, theme = 'red'):
		colormap_data = np.arange(0,1,0.1)
		colormap = painter.sentence_draw(colormap_data.astype('<U5'), colormap_data, color_theme=theme)
		painter.image_save(colormap[0], outputfile = 'colormap.png')

	def set_font(self, fontname='NanumGothic', fontsize=12):

		# 그래프에서 마이너스 폰트 깨지는 문제에 대한 대처
		mpl.rcParams['axes.unicode_minus']=False

		if self.platform_name == 'Linux':
			font_list = fm.findSystemFonts(fontpaths=None, fontext='ttf')
		elif self.platform_name == 'Drawin':
			font_list = fm.OSXInstalledFonts() # OSX에서 설치된 폰트 목록 가져오기
		font_list = [(f.name, f.fname) for f in fm.fontManager.ttflist if fontname in f.name]
		if len(font_list)==0:
			sys.stderr.write('ERROR: No such font name "%s".\n'%(fontname))
			if self.platform_name == 'Linux':
				self.HELP_install_ttf_in_linux()
			exit(1)

		mpl.rcParams['font.family'] = font_list[0][0]
		mpl.rcParams['font.size'] = fontsize

		return ImageFont.truetype(font_list[0][1], fontsize, encoding='utf-8')

	def image_save(self, image, outputfile='image.png'):
		'''
			image
				<PIL.Image.Image image mode=RGB size=(width)x(height) at 0x7F3361BB4710>
		'''
		image.save(outputfile)
		image.close()

	def vconcat(self, image_list, line_space_size = 2, background_color = 'white'):

		'''
			image_list
				self.Image's list
				= [<PIL.Image.Image image mode=RGB size=(width)x(height) at 0x7F3361BB4710>]
		'''
		
		full_width = 0
		full_height = 0
		
		for image in image_list:
			width,height = image.size
			if full_width < width: full_width = width
			full_height += height
		full_height += line_space_size * len(image_list)
		
		output = Image.new('RGB', (full_width, full_height), background_color)
		output_width = 0
		output_height = 0
		for image in image_list:
			width, height = image.size
			output.paste(image,(output_width, output_height))
			output_height += height + line_space_size
			image.close()

		return output

	def hconcat(self, image_list, word_space_size = 5, background_color = 'white'):

		'''
			image_list
				Image's list 
				= [<PIL.Image.Image image mode=RGB size=(width)x(height) at 0x7F3361BB4710>]
		'''
		
		full_width = 0
		full_height = 0
		
		for image in image_list:
			width,height = image.size
			if full_height < height: full_height = height
			full_width += width
		full_width += word_space_size * len(image_list)
		
		output = Image.new('RGB', (full_width, full_height), background_color)
		output_width = 0
		output_height = 0
		for image in image_list:
			width, height = image.size
			output.paste(image,(output_width, output_height))
			output_width += width + word_space_size
			image.close()

		return output
			

	def value_cleaning(self, value_list, option='original'):
		'''
			value_list
				[1,2,3] or ['white','black'], ...
			option
				original(default): return original value list
				softmax          : return softmax result
		'''
		
		value_list = np.array(value_list)
		if 'U' in str(value_list.dtype):
			result = list()
			for value in value_list:
				if value == 'black':
					result.append(255)
				elif value == 'white':
					result.append(0)
				else:
					sys.stderr.write(value+'\n')
					sys.stderr.write("ERROR: Color must be numeric or in ['black','white'], but given %s.\n"%value)
		else:
			if option=='softmax':
				result = self.softmax(value_list.astype('float'))
			else: result = value_list
			
		return result
		


	def sentence_draw(self, char_list, value_list, font_size = 50, color_theme = 'red', word_space_size = 5):

		'''
			char_list
				['테','스','트']
			value_list
				[1, 2, 3] or ['white', 'black'], ...
		'''
		
		value_list = self.value_cleaning(value_list)
			

		font = self.set_font('NanumGothic', font_size)

		figsize = plot.rcParams['figure.figsize']

		if len(char_list) != len(value_list):
			sys.stderr.write('ERROR: Sentence length is not equal with the number of values.\n')
			exit(1)
		

		full_width = 0
		image_list = list()
		for index in range(len(char_list)):
			
			plot.figure(figsize=(font_size, font_size))
			width, height = font.getsize(char_list[index])
			width += word_space_size
			height += word_space_size

			color_value = (1-value_list[index])*255
			if color_theme == 'red':
				back_color = (5*color_value, color_value, color_value)
			elif color_theme == 'green':
				back_color = (color_value, 5*color_value, color_value)
			elif color_theme == 'blue':
				back_color = (color_value, color_value, 5*color_value)
			elif color_theme == 'magenta':
				back_color = (250*color_value, color_value, 40*color_value)
			elif color_theme == 'cyan':
				back_color = (color_value, 40*color_value, 250*color_value)
			elif color_theme == 'yellow':
				back_color = (40*color_value, color_value, 250*color_value)
			else:
				sys.stderr.write("ERROR: Color theme must be in ['red', 'green', 'blue', 'magenta', 'cyan', 'yellow'], but given %s.\n"%color_theme)
				exit(1)
			back_color = tuple(int(c) for c in back_color)

			if color_value < 100: font_color = 'white'
			else: font_color = 'black'
			
			img = Image.new('RGB',(width,height), color= back_color)
			drawPad = ImageDraw.Draw(img)

			drawPad.text((0,0), char_list[index], font=font, fill = font_color)
			
			image_list.append(img)
			full_width += width
		full_width += word_space_size * len(char_list)
		total_image = Image.new('RGB', (full_width, height), 'white')
		output_width = 0
		for img in image_list:
			width, height = img.size
			total_image.paste(img, (output_width, 0))
			output_width += width + word_space_size
			img.close()
		
		return [total_image]


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
