---
title: PIL example calc (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'calc'

Functions in program: 
* `def rumus(bil1,bil2,pil):`
* `def inputbil():`
* `def ulang():`
* `def goto(tujuan):`
* `def keluar():`
* `def clrscr():`

Modules used in program: 
* `import os`
* `import sys`

## python calc

Python PIL example: calc

```python
#===========================================#
# Kalkulator Sederhana Dengan Fungsi :	    #
# 1. Tambah	      			    #
# 2. Kurang				    #
# 3. Kali				    #
# 4. Bagi				    #
# 5. Kuadrat				    #
#					    #
# Programmed by : Mydisha		    #
# Blog : www.diasinside.com		    #
# Mail : diastaufik@gmail.com		    #
# Universitas Gunadarma			    #
#===========================================#

import sys
import os

def clrscr():
	os.system('cls')

def keluar():
	sys.exit()

def goto(tujuan):
	global nomor
	nomor = tujuan

def ulang():
	ulang = raw_input("Apakah ingin mengulang ? [y/n] : ")
	if ulang == "y" or ulang == "Y":
		goto(0)
		clrscr()
	elif ulang == "n" or ulang == "N":
		clrscr()
		keluar()
	else:
		print("Pilihan yang anda masukkan tidak tercantum")

def inputbil():
	global bil1,bil2
	bil1 = int(raw_input("Masukkan bilangan ke- 1 : "))
	bil2 = int(raw_input("Masukkan bilangan ke- 2 : "))
	print("")

def rumus(bil1,bil2,pil):
	if pil == 1:
		hitung = bil1 + bil2
		return hitung
	elif pil == 2:
		hitung = bil1 - bil2
		return hitung
	elif pil == 3:
		hitung = bil1 * bil2
		return hitung
	elif pil == 4:
		hitung = bil1 / bil2
		return hitung
	elif pil == 5:
		hitung = bil1 ** bil2
		return hitung
	else:
		print("Pilihan tidak tersedia")

nomor = 0

while True:

	if nomor == 0:
		clrscr()
		print(("---------------------------"))
		print(("Kalkulator Sederhana Python"))
		print(("---------------------------"))
		print(("1. Tambah"))
		print(("2. Kurang"))
		print(("3. Kali"))
		print(("4. Bagi"))
		print(("5. Kuadrat"))
		inputan = int(raw_input("Masukkan operasi pilihan anda [ 1 - 5 ] : "))
		inputbil()
		if inputan == 1:
			goto(1)
		elif inputan == 2:
			goto(2)
		elif inputan == 3:
			goto(3)
		elif inputan == 4:
			goto(4)
		elif inputan == 5:
			goto(5)
		elif inputan == 6:
			goto(6)
		else:
			goto(0)

	elif nomor == 1:
		print(bil1, ("Ditambah"), bil2, ("Adalah :"), rumus(bil1,bil2,nomor))
		ulang()
	elif nomor == 2:
		print(bil1, ("Dikurang"), bil2, ("Adalah :"), rumus(bil1,bil2,nomor))
		ulang()
	elif nomor == 3:
		print(bil1, ("Dikali"), bil2, ("Adalah :"), rumus(bil1,bil2,nomor))
		ulang()
	elif nomor == 4:
		print(bil1, ("Dibagi"), bil2, ("Adalah :"), rumus(bil1,bil2,nomor))
		ulang()
	elif nomor == 5:
		print(bil1, ("Dipangkat"), bil2, ("Adalah :"), rumus(bil1,bil2,nomor))
		ulang()
	else:
		keluar()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
