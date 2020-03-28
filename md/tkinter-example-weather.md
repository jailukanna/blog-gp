---
title: tkinter example weather (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'weather'

Functions in program: 
* `def get_weather(city):`
* `def format_response(weather):`

Modules used in program: 
* `import tkinter.messagebox`
* `import requests`
* `import tkinter as tk`

## python weather

Python tkinter example: weather

```python
#import tkinter library
import tkinter as tk
import requests
import tkinter.messagebox

#set the height for the canvas
HEIGHT = 500
WIDTH = 600

#A function to display the weather
def format_response(weather):
	try:
		name = weather['name']
		desc = weather['weather'][0]['description']
		temp = weather['main']['temp']
				

		details_str = 'City: %s \nConditions: %s \nTemperature (°F): %s' % (name, desc, temp)				
	    
	except:
		details_str = 'Sorry, it seems we cannot find that city. \nCheck if you typed the correct city name.'
	return details_str

# a function to get weather acording to the cities. Note, we get the API key from openweathermap
def get_weather(city):
	weather_key = '5a835f35f03c205b1d1fcdec14ce7fec'
	url = 'https://api.openweathermap.org/data/2.5/weather'
	params = {'APPID': weather_key, 'q': city, 'units': 'imperial'}
	response = requests.get(url, params=params)
	weather = response.json()
	

	label['text'] = format_response(weather)

	Conditions = weather['weather'][0]['description']

	if Conditions == "cloudy":
		tkinter.messagebox.showinfo("Weather", "Carry an umbrella and keep warm")
	elif Conditions == "few clouds":
		tkinter.messagebox.showinfo("Weather", "Carry an umbrella just incase it rains")
	elif Conditions == "scattered clouds":
		tkinter.messagebox.showinfo("Weather", "Good weather, you can have fun")
	elif Conditions == "overcast clouds":
		tkinter.messagebox.showinfo("Weather", "Enjoy yourself")
	elif Conditions == "clear sky":
		tkinter.messagebox.showinfo("Weather", "Brace yourself for high temperatures")
	
root = tk.Tk()
root.title('My Weather App')
root.iconbitmap('weatherBg.png')

#format our display by setting the size, background color and fonts
canvas = tk.Canvas(root, height=HEIGHT, width=WIDTH)
canvas.pack()

background_image = tk.PhotoImage(file='weatherBg.png')
background_label = tk.Label(root, image=background_image)
background_label.place(relwidth=1, relheight=1)

frame = tk.Frame(root, bg='#036C08', bd=5)
frame.place(relx=0.5, rely=0.1, relwidth=0.75, relheight=0.1, anchor='n')

entry = tk.Entry(frame, font=('valera', 15))
entry.place(relwidth=0.70, relheight=1)

button = tk.Button(frame, text="Check Weather", bg='#036C08', font=('valera', 12),  fg='white', command=lambda: get_weather(entry.get()))
button.place(relx=0.7, relheight=1, relwidth=0.3)

display_frame = tk.Frame(root, bg='#036C08', bd=10)
display_frame.place(relx=0.5, rely=0.25, relwidth=0.75, relheight=0.5, anchor='n')

label = tk.Label(display_frame, font=('valera', 18), anchor ='nw', justify ='left', bd=4)
label.place(relwidth=1, relheight=1)



root.mainloop() 


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
