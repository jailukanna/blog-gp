---
title: turtle example 4 krabice (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example '4 krabice'


## python 4 krabice

Python turtle example: 4 krabice

```python
krabice = "krabice cajovych pytliku"
pytel = "cerstve prazena kava"

for x in pytel, krabice:
    print(x)

print("   --- * --- ")

# tohle neprojde
# for kava, caj in pytel, krabice:
#     print(kava, caj)

# protoze to dela tohle
# for x in pytel, krabice:
#     kava, caj = x

# varianta se seznamy:
krabice = ["cerny caj", "zeleny caj", "mol", "bily caj"]
pytel = ["prvni zrnko", "druhe zrnko", "treti zrnko", "mys"]


print("Procházím dvojice")
for x in zip(pytel, krabice):
    print(x)

print("   --- * --- ")

print("Rozhodím dvojici do dvou proměnných, vypíšu printem.")
for kava, caj in zip(pytel, krabice):
    print(kava, caj, sep=", ")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
