---
title: mysql example xmlRip (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'xmlRip'


Modules used in program: 
* `import mysql.connector`

## python xmlRip

Python mysql example: xmlRip

```python
from xml.dom import minidom
from xml.dom.minidom import parse
import mysql.connector

config = {
	'user': 'trueviewhub',
	'password': 'noPasswordForYou',
	'host': 'localhost',
	'database': 'trueviewhub'
	}

conn = mysql.connector.connect(**config)
global cursor
cursor = conn.cursor();

xmldoc = minidom.parse('SwitchRm.xml')
print(xmldoc.childNodes)
#after initial xml tag, everything is within one parent element for the xml doc called the workbook... 
workbook = xmldoc.childNodes[1]
worksheet = workbook.getElementsByTagName('Worksheet')
print("---000----")
#separate each of the worksheets. 
row = worksheet[0]
rack = worksheet[1]
fmd = worksheet[2]
pdu = worksheet[3]
rpp = worksheet[4]
panel = worksheet[5]
card = worksheet[6]
bladeServer = worksheet[7]
pduToPs = worksheet[8]
psToPanel = worksheet[9]

#get the data from the row worksheet. 
rowList = row.getElementsByTagName('Row')
for x in range(1, len(rowList)):
	cell = rowList[x].getElementsByTagName("Cell")
	print("---new row ----")
	rowItemArray = []
	for y in range(0, len(cell)):
		if(cell[y].getAttribute("ss:Index")):
			blankValues = int(cell[y].getAttribute("ss:Index"))-y
			for i in range(1, blankValues):
				rowItemArray.append("")
		data = cell[y].getElementsByTagName("Data");
		if len(data) > 0:
			rowItemArray.append(data[0].firstChild.nodeValue)
	sql = "INSERT INTO row(name, location, site, floor, equipmentRoom) values('%s', '%s', '%s', '%s', '%s')" % (rowItemArray[0], rowItemArray[1], rowItemArray[2], rowItemArray[3], rowItemArray[4])
	print(sql)
	cursor.execute(sql)
	conn.commit()

#get data for the rack worksheet
rowList = rack.getElementsByTagName('Row')
for x in range(1, len(rowList)):
	cell = rowList[x].getElementsByTagName("Cell")
	print("---new row ----")
	rowItemArray = []
	for y in range(0, len(cell)):
		if(cell[y].getAttribute("ss:Index")):
			blankValues = int(cell[y].getAttribute("ss:Index"))-y
			for i in range(1, blankValues):
				rowItemArray.append("")
		data = cell[y].getElementsByTagName("Data");
		if len(data) > 0:
			rowItemArray.append(data[0].firstChild.nodeValue)
	#print(rowItemArray)
	sql = "INSERT INTO rack(rackName, location, site, floor, equipmentRoom, rowName, parent, createFrom, rowPosition, assetId, lineOfBusiness, datastore1, datastore2, serialNum, comments, gridPosition, rotation, offsetX, offsetY) values('%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s','%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s')" % (rowItemArray[0], rowItemArray[1], rowItemArray[2], rowItemArray[3], rowItemArray[4], rowItemArray[5], rowItemArray[6], rowItemArray[7], rowItemArray[8], rowItemArray[9], rowItemArray[10], rowItemArray[11], rowItemArray[12], rowItemArray[13], rowItemArray[14], rowItemArray[15], rowItemArray[16], rowItemArray[17], rowItemArray[18])
	print(sql)
	cursor.execute(sql)
	conn.commit()

rowList = fmd.getElementsByTagName('Row')
for x in range(1, len(rowList)):
	cell = rowList[x].getElementsByTagName("Cell")
	print("---new row ----")
	rowItemArray = []
	for y in range(0, len(cell)):
		if(cell[y].getAttribute("ss:Index")):
			blankValues = int(cell[y].getAttribute("ss:Index"))-y
			for i in range(1, blankValues):
				rowItemArray.append("")
		data = cell[y].getElementsByTagName("Data");
		if len(data) > 0:
			rowItemArray.append(data[0].firstChild.nodeValue)
	#print(rowItemArray)
	sql = "INSERT INTO fmd(name, location, site, floor, equipmentRoom, rowName, parent, createFrom, posInRow, assetId, lineOfBusiness, datastore1, datastore2, serialNum, comments, gridPosition, rotation, offsetX, offsetY) values('%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s','%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s')" % (rowItemArray[0], rowItemArray[1], rowItemArray[2], rowItemArray[3], rowItemArray[4], rowItemArray[5], rowItemArray[6], rowItemArray[7], rowItemArray[8], rowItemArray[9], rowItemArray[10], rowItemArray[11], rowItemArray[12], rowItemArray[13], rowItemArray[14], rowItemArray[15], rowItemArray[16], rowItemArray[17], rowItemArray[18])
	print(sql)
	cursor.execute(sql)
	conn.commit()

rowList = pdu.getElementsByTagName('Row')
for x in range(1, len(rowList)):
	cell = rowList[x].getElementsByTagName("Cell")
	print("---new row ----")
	rowItemArray = []
	for y in range(0, len(cell)):
		if(cell[y].getAttribute("ss:Index")):
			blankValues = int(cell[y].getAttribute("ss:Index"))-y
			for i in range(1, blankValues):
				rowItemArray.append("")
		data = cell[y].getElementsByTagName("Data");
		if len(data) > 0:
			rowItemArray.append(data[0].firstChild.nodeValue)
	#print(rowItemArray)
	sql = "INSERT INTO fmd(name, location, site, floor, equipmentRoom, rowName, parent, createFrom, posInRow, assetId, lineOfBusiness, datastore1, datastore2, serialNum, comments, gridPosition, rotation, offsetX, offsetY) values('%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s','%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s')" % (rowItemArray[0], rowItemArray[1], rowItemArray[2], rowItemArray[3], rowItemArray[4], rowItemArray[5], rowItemArray[6], rowItemArray[7], rowItemArray[8], rowItemArray[9], rowItemArray[10], rowItemArray[11], rowItemArray[12], rowItemArray[13], rowItemArray[14], rowItemArray[15], rowItemArray[16], rowItemArray[17], rowItemArray[18])
	print(sql)
	cursor.execute(sql)
	conn.commit()

rowList = rpp.getElementsByTagName('Row')
for x in range(1, len(rowList)):
	cell = rowList[x].getElementsByTagName("Cell")
	print("---new row ----")
	rowItemArray = []
	for y in range(0, len(cell)):
		if(cell[y].getAttribute("ss:Index")):
			blankValues = int(cell[y].getAttribute("ss:Index"))-y
			for i in range(1, blankValues):
				rowItemArray.append("")
		data = cell[y].getElementsByTagName("Data");
		if len(data) > 0:
			rowItemArray.append(data[0].firstChild.nodeValue)
	#print(rowItemArray)
	sql = "INSERT INTO fmd(name, location, site, floor, equipmentRoom, rowName, parent, createFrom, posInRow, assetId, lineOfBusiness, datastore1, datastore2, serialNum, comments, gridPosition, rotation, offsetX, offsetY) values('%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s','%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s')" % (rowItemArray[0], rowItemArray[1], rowItemArray[2], rowItemArray[3], rowItemArray[4], rowItemArray[5], rowItemArray[6], rowItemArray[7], rowItemArray[8], rowItemArray[9], rowItemArray[10], rowItemArray[11], rowItemArray[12], rowItemArray[13], rowItemArray[14], rowItemArray[15], rowItemArray[16], rowItemArray[17], rowItemArray[18])
	print(sql)
	cursor.execute(sql)
	conn.commit()

rowList = panel.getElementsByTagName('Row')
for x in range(1, len(rowList)):
	cell = rowList[x].getElementsByTagName("Cell")
	print("---new row ----")
	rowItemArray = []
	for y in range(0, len(cell)):
		if(cell[y].getAttribute("ss:Index")):
			blankValues = int(cell[y].getAttribute("ss:Index"))-y
			for i in range(1, blankValues):
				rowItemArray.append("")
		data = cell[y].getElementsByTagName("Data");
		if len(data) > 0:
			rowItemArray.append(data[0].firstChild.nodeValue)
	#print(rowItemArray)
	if(len(rowItemArray) <23):
		j = 23 - len(rowItemArray)
		rowItemArray.append('')
	sql = "INSERT INTO panel(panelName, location, site, floor, equipmentRoom, rowName, enclosure, parent, createFrom, uPosition, mount, assetId, lineOfBusiness, serialNum, comments, iTracsClass, mfgName, model, width, height, depth, horiontalAlign, horiontalOffset) values('%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s')" % (rowItemArray[0], rowItemArray[1], rowItemArray[2], rowItemArray[3], rowItemArray[4], rowItemArray[5], rowItemArray[6], rowItemArray[7], rowItemArray[8], rowItemArray[9], rowItemArray[10], rowItemArray[11], rowItemArray[12], rowItemArray[13], rowItemArray[14], rowItemArray[15], rowItemArray[16], rowItemArray[17], rowItemArray[18], rowItemArray[19], rowItemArray[20], rowItemArray[21], rowItemArray[22])
	print(sql)
	cursor.execute(sql)
	conn.commit()


rowList = card.getElementsByTagName('Row')
for x in range(1, len(rowList)):
	cell = rowList[x].getElementsByTagName("Cell")
	print("---new row ----")
	rowItemArray = []
	for y in range(0, len(cell)):
		if(cell[y].getAttribute("ss:Index")):
			blankValues = int(cell[y].getAttribute("ss:Index"))-y
			for i in range(1, blankValues):
				rowItemArray.append("")
		data = cell[y].getElementsByTagName("Data");
		if len(data) > 0:
			rowItemArray.append(data[0].firstChild.nodeValue)
	#print(rowItemArray)
	if(len(rowItemArray) <24):
		j = 24 - len(rowItemArray)
		print("----j---")
		print(j)
		for k in range(1, j):
			print("---k---")
			print(k)
			rowItemArray.append('')
	sql = "INSERT INTO card(deviceName, location, site, floor, equipmentRoom, rowName, enclosure, panel, parent, createFrom, slotName, assetId, lineOfBusiness, serialNum, comments, side, iTracsClass, mfgName, model, width, height, depth, xPosition, yPosition) values('%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s','%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s')" % (rowItemArray[0], rowItemArray[1], rowItemArray[2], rowItemArray[3], rowItemArray[4], rowItemArray[5], rowItemArray[6], rowItemArray[7], rowItemArray[8], rowItemArray[9], rowItemArray[10], rowItemArray[11], rowItemArray[12], rowItemArray[13], rowItemArray[14], rowItemArray[15], rowItemArray[16], rowItemArray[17], rowItemArray[18], rowItemArray[19], rowItemArray[20], rowItemArray[21], rowItemArray[22], rowItemArray[23])
	print(sql)
	cursor.execute(sql)
	conn.commit()

rowList = pduToPs.getElementsByTagName('Row')
for x in range(1, len(rowList)):
	cell = rowList[x].getElementsByTagName("Cell")
	print("---new row ----")
	rowItemArray = []
	for y in range(0, len(cell)):
		if(cell[y].getAttribute("ss:Index")):
			blankValues = int(cell[y].getAttribute("ss:Index"))-y
			for i in range(1, blankValues):
				rowItemArray.append("")
		data = cell[y].getElementsByTagName("Data");
		if len(data) > 0:
			rowItemArray.append(data[0].firstChild.nodeValue)
	#print(rowItemArray)
	if(len(rowItemArray) <27):
		j = 27 - len(rowItemArray)
		print("----j---")
		print(j)
		for k in range(1, j):
			print("---k---")
			print(k)
			rowItemArray.append('')
	sql = "INSERT INTO pduToPS(fromLocation, fromSite, fromFloor, fromEquipmentroom, fromRow, fromEnclosure, fromPower, fromPanel, fromPlugins, fromParent, powerOutputPort, fromEqid, toLocation, toSite, toFloor, toEquipmentroom, toRow, toEnclosure, toPower, toPanel, toPlugins,toParent, powerInputPort, toEqid,connectionClass,connectionName,withStore,comments) values('%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s','%s', '%s', '%s', '%s', '%s','%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s')" % (rowItemArray[0], rowItemArray[1], rowItemArray[2], rowItemArray[3], rowItemArray[4], rowItemArray[5], rowItemArray[6], rowItemArray[7], rowItemArray[8], rowItemArray[9], rowItemArray[10], rowItemArray[11], rowItemArray[12], rowItemArray[13], rowItemArray[14], rowItemArray[15], rowItemArray[16], rowItemArray[17], rowItemArray[18], rowItemArray[19], rowItemArray[20], rowItemArray[21], rowItemArray[22], rowItemArray[23], rowItemArray[24], rowItemArray[25], rowItemArray[26], rowItemArray[27])
	print(sql)
	cursor.execute(sql)
	conn.commit()


rowList = psToPanel.getElementsByTagName('Row')
for x in range(1, len(rowList)):
	cell = rowList[x].getElementsByTagName("Cell")
	print("---new row ----")
	rowItemArray = []
	for y in range(0, len(cell)):
		if(cell[y].getAttribute("ss:Index")):
			blankValues = int(cell[y].getAttribute("ss:Index"))-y
			for i in range(1, blankValues):
				rowItemArray.append("")
		data = cell[y].getElementsByTagName("Data");
		if len(data) > 0:
			rowItemArray.append(data[0].firstChild.nodeValue)
	#print(rowItemArray)
	if(len(rowItemArray) <27):
		j = 27 - len(rowItemArray)
		print("----j---")
		print(j)
		for k in range(1, j):
			print("---k---")
			print(k)
			rowItemArray.append('')
	sql = "INSERT INTO psToPanel(fromLocation, fromSite, fromFloor, fromEquipmentroom, fromRow, fromEnclosure, fromPower, fromPanel, fromPlugins, fromParent, powerOutputPort, fromEqid, toLocation, toSite, toFloor, toEquipmentroom, toRow, toEnclosure, toPower, toPanel, toPlugins,toParent, powerInputPort, toEqid,connectionClass,connectionName,withStore,comments) values('%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s','%s', '%s', '%s', '%s', '%s','%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s')" % (rowItemArray[0], rowItemArray[1], rowItemArray[2], rowItemArray[3], rowItemArray[4], rowItemArray[5], rowItemArray[6], rowItemArray[7], rowItemArray[8], rowItemArray[9], rowItemArray[10], rowItemArray[11], rowItemArray[12], rowItemArray[13], rowItemArray[14], rowItemArray[15], rowItemArray[16], rowItemArray[17], rowItemArray[18], rowItemArray[19], rowItemArray[20], rowItemArray[21], rowItemArray[22], rowItemArray[23], rowItemArray[24], rowItemArray[25], rowItemArray[26], rowItemArray[27])
	print(sql)
	cursor.execute(sql)
	conn.commit()


cursor.close()
conn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
