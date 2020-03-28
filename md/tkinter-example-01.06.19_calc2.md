---
title: tkinter example 01.06.19 calc2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example '01.06.19 calc2'

Functions in program: 
* `def ce():`
* `def equal_def():`
* `def press(num):`
* `def press0():`

Modules used in program: 
* `import tkinter as tk`

## python 01.06.19 calc2

Python tkinter example: 01.06.19 calc2

```python
import tkinter as tk

# Переменная text отвечает за буфер информации в который мы добавляем нажатые цифры и знаки
text = ""


# Весь код написан используя lambda, но я решил оставить один пример без ее использования. На 64 строке мы обращаемся к
# этой переменной, которая вызывает press() передавая 0 в значении num (костыль)
def press0():
    press(0)


# Обращаясь к данной функции, мы добавляем в наш буфер text num, то есть нажатую цифру, либо знак.
# Командой text_var.set(text) мы переносим информацию из буфера в переменную, которая используеться как textvariable
# для text_field (Entry, в котром записываються все вычисления)
def press(num):
    global text
    text = text + str(num)
    text_var.set(text)


# Пытаеться сложить вычесления написаное в буфере text, используя функцию eval, которая получает из text пример
# 1+5 (например) и решает его внутри себя, выводя в переменную total, очищая затем text. Если выражение неправльное,
# (1+/5 к примеру) выводит на text_field 'error' и очищает text.
def equal_def():
    try:
        global text
        total = str(eval(text))
        text_var.set(total)
        text = ""
    except:
        text_var.set("error")
        text = ""


# Очищает text и выводит на text_field, через text_var
def ce():
    global text
    text = ""
    text_var.set("")


# Начало кода для исполнения
if __name__ == "__main__":
    root = tk.Tk()
    # Меняем заголовок окна
    root.title("Calculator")
    # Можно конечно и через text_field.config(text="") и параметром text вместо textvariable, но пусть будет так
    text_var = tk.StringVar()
    # Создаем поле для ввода text_field и textvariable присваиваем переменную text_var
    text_field = tk.Entry(root, textvariable=text_var)
    # Размещаем на сетке grid
    text_field.grid(column=0, row=0, columnspan=4)
    # Очищаем поле для ввода
    text_var.set('')

    # Создаем остальные кнопки
    button_equal = tk.Button(root, text="=", width=8, height=2, command=equal_def)
    button_ce = tk.Button(root, text="CE", width=8, height=2, command=ce)
    button_division = tk.Button(root, text="/", width=8, height=2, command=lambda: press("/"))
    button_multiply = tk.Button(root, text="*", width=8, height=2, command=lambda: press("*"))
    button_subtract = tk.Button(root, text="-", width=8, height=2, command=lambda: press("-"))
    button_addition = tk.Button(root, text="+", width=8, height=2, command=lambda: press("+"))
    button_0 = tk.Button(root, text="0", width=8, height=2, command=press0)
    button_1 = tk.Button(root, text="1", width=8, height=2, command=lambda: press("1"))
    button_2 = tk.Button(root, text="2", width=8, height=2, command=lambda: press("2"))
    button_3 = tk.Button(root, text="3", width=8, height=2, command=lambda: press("3"))
    button_4 = tk.Button(root, text="4", width=8, height=2, command=lambda: press("4"))
    button_5 = tk.Button(root, text="5", width=8, height=2, command=lambda: press("5"))
    button_6 = tk.Button(root, text="6", width=8, height=2, command=lambda: press("6"))
    button_7 = tk.Button(root, text="7", width=8, height=2, command=lambda: press("7"))
    button_8 = tk.Button(root, text="8", width=8, height=2, command=lambda: press("8"))
    button_9 = tk.Button(root, text="9", width=8, height=2, command=lambda: press("9"))

    # Размещаем их на сетке grid
    button_7.grid(column=0, row=1)
    button_8.grid(column=1, row=1)
    button_9.grid(column=2, row=1)
    button_addition.grid(column=3, row=1)
    button_4.grid(column=0, row=2)
    button_5.grid(column=1, row=2)
    button_6.grid(column=2, row=2)
    button_subtract.grid(column=3, row=2)
    button_1.grid(column=0, row=3)
    button_2.grid(column=1, row=3)
    button_3.grid(column=2, row=3)
    button_multiply.grid(column=3, row=3)
    button_ce.grid(column=0, row=4)
    button_0.grid(column=1, row=4)
    button_equal.grid(column=2, row=4)
    button_division.grid(column=3, row=4)

    # Шоб не вырубалось после начала исполнения
    root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
