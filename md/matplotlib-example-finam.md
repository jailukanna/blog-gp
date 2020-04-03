---
title: matplotlib example finam (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'finam'


Modules used in program: 
* `import matplotlib.pyplot as plt #подключаем из большой библиотеки модуль pyplot, который является аналогом МАТЛАБА. МАТЛАБ - это язык программирования и набор программ для математиков. Этот модуль питона пытается её заменить. Присваиваем ему короткое имя plt.`
* `import matplotlib #подключаем библиотеку matplotlib целиком`

## python finam

Python matplotlib example: finam

```python
# -*- coding: utf-8 -*-
#скрипт умеет работать только с доллар-рублём. Как смотреть акции, смотрите в моей статье на Смарт Лабе "Качаем котировки с Финама" https://smart-lab.ru/blog/514941.php
import matplotlib #подключаем библиотеку matplotlib целиком
import matplotlib.pyplot as plt #подключаем из большой библиотеки модуль pyplot, который является аналогом МАТЛАБА. МАТЛАБ - это язык программирования и набор программ для математиков. Этот модуль питона пытается её заменить. Присваиваем ему короткое имя plt.
from datetime import datetime #подключаем функцию для преобразования дат в нужный формат.
from urllib.parse import urlencode #urlencode требуется для формирования строки, которая улетит на Финам в качестве запроса.
from urllib.request import urlopen #с помощью urlopen будем отсылать запрос на Финам и получать текстовый ответ.
print("Подключили библиотеки")
symbol='USD000UTSTOM' #Тикер желаемого инструмента. В данном случае это доллар-рубль на валютном рынке Московской биржи.
period=8 #Период графика. 8 - это дневки. Один кирпичик данных описывает картину 1 дня: открытие, закрытие, максимум, минимум. Другие варианты: {'tick': 1, 'min': 2, '5min': 3, '10min': 4, '15min': 5, '30min': 6, 'hour': 7, 'daily': 8, 'week': 9, 'month': 10}
start_date_str = "01.01.2010" #с какой даты качаем данные
end_date_str = datetime.today().strftime('%d.%m.%Y') #по какую дату качаем данные. Здесь указано: "по сегодня"
#выполняем преобразование дат, чтобы их понял Финам.
start_date = datetime.strptime(start_date_str, "%d.%m.%Y").date() 
start_date_rev=datetime.strptime(start_date_str, '%d.%m.%Y').strftime('%Y%m%d')
end_date = datetime.strptime(end_date_str, "%d.%m.%Y").date()
end_date_rev=datetime.strptime(end_date_str, '%d.%m.%Y').strftime('%Y%m%d')
print("Строим график с "+ start_date_rev+" по "+end_date_rev)
#Все параметры упаковываем в единую структуру. Эти данные улетят от нас на Финам.
#Как сделать тонкую настройку этого запроса, смотрите в моей статье на Смарт Лабе "Качаем котировки с Финама" https://smart-lab.ru/blog/514941.php
params = urlencode([('market', 0), ('em', 182400), ('code', symbol), ('apply',0), ('df', start_date.day), ('mf', start_date.month - 1), ('yf', start_date.year), ('from', start_date_str), ('dt', end_date.day), ('mt', end_date.month - 1), ('yt', end_date.year), ('to', end_date_str), ('p', period), ('f', symbol+"_" + start_date_rev + "_" + end_date_rev), ('e', ".csv"), ('cn', symbol), ('dtf', 1), ('tmf', 1), ('MSOR', 0), ('mstime', "on"), ('mstimever', 1), ('sep', 1), ('sep2', 1), ('datf', 1), ('at', 0)])
#итоговый урл (строка), который улетит на сервер Финама:
url = "http://export.finam.ru/" + symbol+"_" + start_date_rev + "_" + end_date_rev + ".csv?" + params
print("Этот урл сформировали мы. Он улетает на Финам!")
print(url)
txt=urlopen(url, timeout=10).readlines() #На сайт Финама улетел урл. Оттуда прилетел ответ и записался в переменную txt
x=[] #Здесь будут даты на горизонтальной оси.
y=[] #Здесь будут цены на вертикальной оси.
for line in txt: #бегаем в цикле по прилетевшим значениям. Разносим их по x и y 
	tmp=str(line).split(",") #читаем строчку за строчкой и выбираем из неё данные (значения разделены запятой)
	date=tmp[2] #дата - это третье поле в строке
	x.append(matplotlib.dates.date2num(datetime.strptime(date, '%Y%m%d'))) #запишем дату в понятном для библиотеки matplotlib виде (она станет числом)
	y.append((float(tmp[5])+float(tmp[6])+float(tmp[7]))/3) #посчитаем типическую цену за день и добавим в chart_y. Типическая цена=(цена закрытия+максимум+минимум)/3. 
print("Оси x и y заполнены! На них "+str(len(x))+" значений!")
print("Приступаем к построению графика!")
fig, ax = plt.subplots() #начинаем работать с библиотекой matplotlib. Создаём фигуру.
ymax = max(y) #находим максимальное значение, до которого доходил доллар.
xmax = x[y.index(ymax)] #находим дату максимального значения.
ymax=round(ymax,2)#округляем максимум до копеек.
print("Максимум по доллару был "+str(ymax))
ax.annotate('MAX:'+str(ymax), #на график поместим аннотацию: максимальное значение доллара.
			xy=(xmax, ymax*(1.005)), #место куда поместим аннотацию: визуально чуть-чуть повыше максимума. 
			horizontalalignment='center', #выровняем метку максимума по центру.
            )
ax.plot(x, y, color="g") #наносим график доллара: оси x и y. Цвет зелёный.
plt.title("USD/RUB", fontsize=20)
ax.xaxis.set_major_locator(matplotlib.dates.YearLocator(1)) #делаем так, чтобы на оси дат были не числа типа 10.12.2018, а только годы
ax.xaxis.set_major_formatter(matplotlib.dates.DateFormatter('%Y')) #формат оси x - годы.
plt.grid() #наносим сетку.
plt.show() #показываем график!

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
