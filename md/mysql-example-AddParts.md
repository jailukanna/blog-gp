---
title: mysql example AddParts (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'AddParts'

Functions in program: 
* `def logIt(text_file, s):`
* `def findCBPM(cpm_id, bpm_id):`
* `def findCLM(cpm_id, lwdm_id):`
* `def findLWDM(gm_num, wd_id):`
* `def findBPM(partNum, gm_num):`
* `def findCPM(gm_num, car_id):`
* `def findGM(partName, parent_id, sub_parent_id):`
* `def findSubCat_id(subCategory, parent_id):`
* `def findCat_id(category):`
* `def findCarId(carName, variant, yoms, yome):`
* `def findX(q):`
* `def findBase_id(car_id):`
* `def createGM(parent_id, sub_parent_id):`
* `def createHalfGM(parent_id, sub_parent_id):`
* `def insertINTOcar_models_new(carName, variant, carBrandId, fuel_type, yoms, yome, image_path):`
* `def insertINTOsub_category(subCategory, parent_id):`
* `def insertINTOcategories(category):`
* `def insertINTOparts_labour(gm_num, parent_id, hsn_code, tax_percent):`
* `def insertINTOcar_parts_mapping(car_id, gm_num, part):`
* `def insertINTOlabours_work_done_mapping(gm_num, wd_id):`
* `def insertINTOcar_labour_mapping(cpm_id, lwdm_id):`
* `def insertINTObrand_part_mapping(partNum, partBrand, gm_num, partType):`
* `def insertINTOcar_brand_part_mapping(cpm_id, bpm_id, m_price):`
* `def runReturnQuery(q):`
* `def runQuery(q):`

Modules used in program: 
* `import os`
* `import mysql.connector`
* `import xlrd`
* `import xlwt`

## python AddParts

Python mysql example: AddParts

```python
#
#scripts are only good untill ~parts_labour.id exceeds 999999 OR ~categories.id exceeds 9999 OR ~sub_category.id exceeds 9999
#
import xlwt
import xlrd
import mysql.connector
from mysql.connector import Error
import os
from xlrd import open_workbook

partBrand = "Maruti Genuine Parts"
partType = "oem"
partWise = [4, 3, 2, 7, 1, 8, 6, 9, 10, 5]
assWise = [11, 12, 13, 14, 15, 16, 17, 18, 4, 3, 2, 7, 1, 8, 9, 5]
common_wd = [19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 18, 24, 32, 33, 34, 32, 35, 36, 37, 38, 39, 40, 41, 42, 43, 43, 44, 45, 46, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 46, 47, 48, 49, 6, 6, 6, 6, 50, 51]
c_ass = ['Wheels', 'Wheels', 'Electricals', 'Electricals', 'Wheels', 'Brake', 'Body', 'Body', 'Body', 'Body', 'Body', 'Transmission', 'Transmission', 'Fuel System', 'Brake', 'Brake', 'Interior', 'Miscellaneous', 'Engine', 'Miscellaneous', 'Wheels', 'Miscellaneous', 'Electricals', 'Cooling System', 'Cooling System', 'Interior', 'Interior', 'Body', 'Body', 'Wheels', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'Body', 'AC', 'Cooling System', 'Brake', 'Steering', 'Miscellaneous', 'Miscellaneous']
c_subAss = ['Wheel', 'Wheel', 'Battery', 'Battery', 'Wheel', 'Disc Brake', 'Chassis', 'Exterior', 'Exterior', 'Exterior', 'Exterior', 'Clutch', 'Clutch', 'CNG', 'Disc Brake', 'Disc Brake', 'Interior', 'Miscellaneous', 'Engine', 'Miscellaneous', 'Wheel', 'Miscellaneous', 'Headlight', 'Hoses', 'Hoses', 'Interior', 'Interior', 'Exterior', 'Exterior', 'Wheel', 'Exterior', 'Exterior', 'Exterior', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Panels', 'Air Conditioner', 'Cooling unit', 'Brake', 'Steering', 'Miscellaneous', 'Miscellaneous']
c_parts = ['Wheel', 'Wheel', 'Battery', 'Battery', 'Bearing', 'Caliper Pin', 'Chassis', 'Full Body', 'Full Body', 'Full Body', 'Full Body', 'Clutch', 'Clutch Fork', 'CNG', 'Brake Pad', 'Disc', 'Interior', 'Key', 'Head', 'Insurance', 'Tyre', 'General', 'Headlight', 'Hose', 'Hose', 'Interior', 'Interior', 'Full Body', 'Underbody', 'Tyre', 'Full Body', 'Full Body', 'Full Body', 'Front Bumper', 'Front Bumper', 'Front Bumper', 'Front Bumper', 'Bonnet', 'Bonnet', 'Bonnet', 'Bonnet', 'Hood', 'Hood', 'Hood', 'Hood', 'Roof', 'Roof', 'Roof', 'Roof', 'Dickey/Trunk', 'Dickey/Trunk', 'Dickey/Trunk', 'Dickey/Trunk', 'Rear Bumper', 'Rear Bumper', 'Rear Bumper', 'Rear Bumper', 'LHS Fender', 'LHS Fender', 'LHS Fender', 'LHS Fender', 'LHS Front Door', 'LHS Front Door', 'LHS Front Door', 'LHS Front Door', 'LHS Rear Door', 'LHS Rear Door', 'LHS Rear Door', 'LHS Rear Door', 'LHS Quarter Panel', 'LHS Quarter Panel', 'LHS Quarter Panel', 'LHS Quarter Panel', 'LHS Running Board', 'LHS Running Board', 'LHS Running Board', 'LHS Running Board', 'LHS Side Mirror', 'LHS Side Mirror', 'LHS Side Mirror', 'LHS Side Mirror', 'RHS Fender', 'RHS Fender', 'RHS Fender', 'RHS Fender', 'RHS Front Door', 'RHS Front Door', 'RHS Front Door', 'RHS Front Door', 'RHS Rear Door', 'RHS Rear Door', 'RHS Rear Door', 'RHS Rear Door', 'RHS Quarter Panel', 'RHS Quarter Panel', 'RHS Quarter Panel', 'RHS Quarter Panel', 'RHS Running Board', 'RHS Running Board', 'RHS Running Board', 'RHS Running Board', 'RHS Side Mirror Paint', 'RHS Side Mirror Paint', 'RHS Side Mirror Paint', 'RHS Side Mirror Paint', 'Pillar', 'Pillar', 'Pillar', 'Pillar', 'Spoiler', 'Spoiler', 'Spoiler', 'Spoiler', 'AC Gas', 'Coolant', 'Brake Oil', 'Steering Oil', 'RSA', 'Car']

#Function for connection to db
def runQuery(q):
	conn = mysql.connector.connect(host='localhost',database='db_demo',user='user',password='password')
	cursor = conn.cursor()
	cursor.execute(q)
	conn.commit()
	cursor.close()
	conn.close()

# Function to run query which returns a list
def runReturnQuery(q):
	conn = mysql.connector.connect(host='localhost',database='db_demo',user='user',password='password')
	cursor = conn.cursor()
	cursor.execute(q)
	temp = cursor.fetchall()
	conn.commit()
	cursor.close()
	conn.close()
	logIt(text_file, q)
	return temp

#insert Functions for car_brand_part_mapping
def insertINTOcar_brand_part_mapping(cpm_id, bpm_id, m_price):
	q = "INSERT INTO `car_brand_part_mapping` (`cpm_id`, `bpnm_id`, `market_price`, `inc_tax`, `is_test`) VALUES ('%s', '%s', '%s', 0, 0);" %(cpm_id, bpm_id, m_price)
	runQuery(q)
	logIt(text_file, q)

#insert Functions for brand_part_mapping
def insertINTObrand_part_mapping(partNum, partBrand, gm_num, partType):
	q = "INSERT INTO `brand_part_mapping` (`part_no`, `brand_name`, `gm_num`, `type`, `is_test`) VALUES ('%s', '%s', '%s', '%s', 0);" %(partNum, partBrand, gm_num, partType)
	runQuery(q)
	logIt(text_file, q)

#insert Functions for car_labour_mapping
def insertINTOcar_labour_mapping(cpm_id, lwdm_id):
	q = "INSERT INTO `car_labour_mapping` (`cpm_id`, `lwdm_id`, `price`, `market_price`) VALUES ('%s', '%s', '0', '0');" %(cpm_id, lwdm_id)
	runQuery(q)
	logIt(text_file, q)

#insert Functions for labours_work_done_mapping
def insertINTOlabours_work_done_mapping(gm_num, wd_id):
	q = "INSERT INTO `labours_work_done_mapping` (`gm_num`, `work_done_id`) VALUES ('%s', '%s');" %(gm_num, wd_id)
	runQuery(q)
	logIt(text_file, q)

#insert Functions for car_parts_mapping
def insertINTOcar_parts_mapping(car_id, gm_num, part):
	q = "INSERT INTO `car_parts_mapping` (`car_id`, `gm_num`, `name`, `is_test`) VALUES ('%s', '%s', '%s', 0);" %(car_id, gm_num, part)
	runQuery(q)
	logIt(text_file, q)

#insert Functions for parts_labour
def insertINTOparts_labour(gm_num, parent_id, hsn_code, tax_percent):
	q = "INSERT INTO `parts_labour` (`gm_num`, `parent_id`, `hsn_code`, `tax_amount`, `is_labour`, `is_test`) VALUES ('%s', '%s', '%s', '%s', 0, 0);" %(gm_num, parent_id, hsn_code, tax_percent)
	runQuery(q)
	logIt(text_file, q)

#insert Functions for categories
def insertINTOcategories(category):
	q = "INSERT INTO `categories` (`name`, `is_child`, `is_test`) VALUES ('%s', '%s', '%s');" %(category, 0, 0)
	runQuery(q)
	logIt(text_file, q)

#insert Functions for sub_category
def insertINTOsub_category(subCategory, parent_id):
	q = "INSERT INTO `sub_category` (`name`, `parent_id`, `is_test`) VALUES ('%s', '%s', '%s');" %(subCategory, parent_id, 0)
	runQuery(q)
	logIt(text_file, q)

#insert Functions for car_models_new
def insertINTOcar_models_new(carName, variant, carBrandId, fuel_type, yoms, yome, image_path):
	q = "INSERT INTO `car_models_new` (`name`, `variant`, `brand_id`, `fuel_type`, `yoms`, `yome`, `image_path`, `is_base`, `status`, `is_test`) VALUES ('%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s', '%s');" %(carName, variant, carBrandId, fuel_type, yoms, yome, image_path, 1, 1, 0)
	runQuery(q)
	logIt(text_file, q)

# Half gm_num = 4 digits of category_id + 4 digits of subcategory_id
# Half gm_num can be used to find if gm_num has already been created or not
def createHalfGM(parent_id, sub_parent_id):
	half_cat_gm = ""
	if (parent_id < 10):
		half_cat_gm = half_cat_gm + "000" + str(parent_id)
	elif (parent_id < 100):
		half_cat_gm = half_cat_gm + "00" + str(parent_id)
	elif (parent_id < 1000):
		half_cat_gm = half_cat_gm +"0" + str(parent_id)
	else:
		half_cat_gm = half_cat_gm + str(parent_id)
	if (sub_parent_id < 10):
		half_cat_gm = half_cat_gm + "000" + str(sub_parent_id)
	elif (sub_parent_id < 100):
		half_cat_gm = half_cat_gm + "00" + str(sub_parent_id)
	elif (sub_parent_id < 1000):
		half_cat_gm = half_cat_gm + "0" + str(sub_parent_id)
	else:
		half_cat_gm = half_cat_gm + str(sub_parent_id)
	return half_cat_gm

# gm_num = half_gm_num + 6  digit of row_id
# this function should be called just before adding into parts_labour

def createGM(parent_id, sub_parent_id):
	gm_num = ''
	half_gm = createHalfGM(parent_id, sub_parent_id)
	q = "select MAX(id) from parts_labour;"
	row_id = findX(q) + 1
	if (row_id < 10):
		gm_num = half_gm + "00000" + str(row_id)
	elif (row_id < 100):
		gm_num = half_gm + "0000" + str(row_id)
	elif (row_id < 1000):
		gm_num = half_gm + "000" + str(row_id)
	elif (row_id < 10000):
		gm_num = half_gm + "00" + str(row_id)
	elif (row_id < 100000):
		gm_num = half_gm + "0" + str(row_id)
	else:
		gm_num = half_gm + str(row_id)
	log_s = "new gm_num generated: '%s'" %(gm_num)
	logIt(text_file, log_s)
	return gm_num

# This function will return base_variant_id
def findBase_id(car_id):
	q = "select base_variant_id, is_base from car_models_new where id='%s';" %(car_id)
	arr = runReturnQuery(q)
	if( arr[0][1] == 1):
		return car_id
	else:
		return arr[0][0]

#Finding Ids and stuff
#>>>	will return 0 if no id found	<<<

# This function is for unique calls only where you want only 1 id/variable
def findX(q):
	arr = runReturnQuery(q)
	if(arr != []):
		x = arr[0][0]
		return x
	else:
		return 0

# This function would find car Id from car_models_new given it's Name, Variant, yoms, yome
def findCarId(carName, variant, yoms, yome):
	q = "select id from car_models_new where Name like '%s' and variant like '%s' and yoms='%s' and yome='%s';" %(carName, variant, yoms, yome)
	car_id = findX(q)
	return car_id

# This function would find category_id given category name
def findCat_id(category):
	q = "select id from categories where Name like '%s';" %(category)
	cat_id = findX(q)
	return cat_id

# This function would find subcategory_id given subcategory name
def findSubCat_id(subCategory, parent_id):
	q = "select id from sub_category where Name like '%s' and parent_id = '%s';" %(category, parent_id)
	subCat_id = findX(q)
	return subCat_id

# this function will   find gm_num for a part given it's name, category_id and subcategory_id
# this function finds gm_num from part name and compares half_gm
def findGM(partName, parent_id, sub_parent_id):
	#find gm_num agains partName
	half_gm = createHalfGM(parent_id, sub_parent_id)
	gm_num = ''
	q = "select gm_num from car_parts_mapping where name like '%s';" %(partName)
	arr = runReturnQuery(q)
	#one part name might have multiple gm_num so match half_gm
	if (arr != []):
		for i in arr:
			if(i[:8] == half_gm):
				gm_num = i
				break
	return gm_num

# this function will find car_parts_mapping_id given gm_num, car_id
def findCPM(gm_num, car_id):
	q = "Select id from car_parts_mapping where gm_num = '%s' and car_id='%s';" % (gm_num, car_id)
	cpm_id = findX(q)
	return cpm_id

# this function will find brand_part_mapping given partNum and gm_num
def findBPM(partNum, gm_num):
	q = "select id from brand_part_mapping where part_no like '%s' and gm_num like '%s';" %(partNum, gm_num)
	bpm_id = findX(q)
	return bpm_id

# this function will find labour_work_done__mapping_id given gm_num and wd_id
def findLWDM(gm_num, wd_id):
	q = "select id from labours_work_done_mapping where gm_num like '%s' and work_done_id='%s';" %(gm_num, wd_id)
	lwdm_id = findX(q)
	return lwdm_id

# this function will find cae_labour_mapping
def findCLM(cpm_id, lwdm_id):
	q = "select id from car_labour_mapping where cpm_id = '%s' and lwdm_id = '%s';" %(cpm_id, lwdm_id)
	clm_id = findX(q)
	return clm_id
	
# this function will find car_brand_part_mapping
def findCBPM(cpm_id, bpm_id):
	q = "select id from car_brand_part_mapping where cpm_id = '%s' and bpnm_id = '%s';" %(cpm_id, bpm_id)
	cbpm_id = findX(q)
	return cbpm_id

def logIt(text_file, s):
	text_file.write(s)
	print(s)

clist = ['addMoreParts.xlsx']
#for each row entry
#Brand	Car	Variant	f_type	YOMS	YOME	Category	Subcategory	Part Num	Part Name	Price	hsn_code	Tax_percent
for i in clist:
	if (i[-4:] == 'xlsx'):
		wb = open_workbook(i)
		numS = wb.nsheets
		sh0 = wb.sheet_by_index(0)
		logFile = i[:-5] + "_log.txt"
		text_file = open(logFile, "w")
		nr = sh0.nrows
		for j in range(1,nr):
			print(j)
			arr = sh0.row_values(j)
			#name variables
			carBrand = str(arr[0]).encode('utf-8').upper()
			carName = str(arr[1]).encode('utf-8').upper()
			carVariant = str(arr[2]).encode('utf-8').upper()
			fType = str(arr[3]).encode('utf-8').upper()
			yoms = int(arr[4])
			yome = arr[5]
			if (yome != 'NOW'):
				yome = int(yome)
			category = str(arr[6]).encode('utf-8').upper().strip(' ').replace(',','').replace('"','').replace("'", "").replace("\\", " ")
			subCategory = str(arr[7]).encode('utf-8').upper().strip(' ').replace(',','').replace('"','').replace("'", "").replace("\\", " ")
			partNum = str(arr[8]).replace(',','').replace('"','').replace("'", "")
			partName = str(arr[9]).encode('utf-8').upper().strip(' ').replace(',','').replace('"','').replace("'", "").replace("\\", " ")
			marketPrice = str(arr[10]).replace(',','').replace('"','').replace("'", "")
			m_price = float(marketPrice)
			hsnCode = str(arr[11])
			taxPercent = str(arr[12])
			#Find Car
			if(findCarId(carName, carVariant, yoms, yome) != 0):
				car_id = findCarId(carName, carVariant, yoms, yome)
				base_id = findBase_id(car_id)
				#Check category
				if(findCat_id(category) != 0):
					cat_id = findCat_id(category)
					#get subCat_id
					subCat_id = findSubCat_id(category, cat_id)
					#check cpm		For cpm also check if base variant has mappings
					#find gm_num
					gm_num = findGM(category, cat_id, subCat_id)
					if (gm_num == ''):
						#if entries done according to my script should find gm_num
						log_s = "no gm_num found for category part: '%s'" %(category)
						logIt(text_file, log_s)
					else:
						#check cpm
						if(findCPM(gm_num, car_id) == 0):
							if(findCPM(gm_num, base_id) == 0):
								#create all mappings cpm, #bpm, #cbpm, lwdm, clm
								insertINTOcar_parts_mapping(car_id, gm_num, category)
								# if(findBPM(partNum, gm_num) == 0):
								# 	insertINTObrand_part_mapping(partNum, partBrand, gm_num, partType)
								cpm_id = findCPM(gm_num, car_id)
								# bpm_id = findBPM(partNum, gm_num)
								# insertINTOcar_brand_part_mapping(cpm_id, bpm_id, m_price)
								for wd_id in assWise:
									if(findLWDM(gm_num, wd_id) == 0):
										insertINTOlabours_work_done_mapping(gm_num, wd_id)
									lwdm_id = findLWDM(gm_num, wd_id)
									if(findCLM(cpm_id, lwdm_id) == 0):
										insertINTOcar_labour_mapping(cpm_id, lwdm_id)
							else:
								cpm_id = findCPM(gm_num, base_id)
								for wd_id in assWise:
									if(findLWDM(gm_num, wd_id) == 0):
										insertINTOlabours_work_done_mapping(gm_num, wd_id)
									lwdm_id = findLWDM(gm_num, wd_id)
									if(findCLM(cpm_id, lwdm_id) == 0):
										insertINTOcar_labour_mapping(cpm_id, lwdm_id)
						else:
							cpm_id = findCPM(gm_num, car_id)
							for wd_id in assWise:
								if(findLWDM(gm_num, wd_id) == 0):
									insertINTOlabours_work_done_mapping(gm_num, wd_id)
								lwdm_id = findLWDM(gm_num, wd_id)
								if(findCLM(cpm_id, lwdm_id) == 0):
										insertINTOcar_labour_mapping(cpm_id, lwdm_id)
				else:
					insertINTOcategories(category)
					cat_id = findCat_id(category)
					insertINTOsub_category(category, cat_id)
					subCat_id = findSubCat_id(category, cat_id)
					gm_num = createGM(cat_id, subCat_id)
					insertINTOparts_labour(gm_num, subCat_id, hsnCode, taxPercent)
					insertINTOcar_parts_mapping(car_id, gm_num, category)
					cpm_id = findCPM(gm_num, car_id)
					for wd_id in assWise:
						if(findLWDM(gm_num, wd_id) == 0):
							insertINTOlabours_work_done_mapping(gm_num, wd_id)
						lwdm_id = findLWDM(gm_num, wd_id)
						if(findCLM(cpm_id, lwdm_id) == 0):
										insertINTOcar_labour_mapping(cpm_id, lwdm_id)
				#check subCategory
				if(findSubCat_id(subCategory, cat_id) != 0):
					subCat_id = findSubCat_id(subCategory, cat_id)
					gm_num = findGM(subCategory, cat_id, subCat_id)
					if (gm_num == ''):
						#if entries done according to my script should find gm_num
						log_s = "no gm_num found for subcategory part: '%s'" %(subCategory)
						logIt(text_file, log_s)
					else:
						#check/get cpm, bpm				For cpm also check if base variant has mappings
						if(findCPM(gm_num, car_id) == 0):
							if(findCPM(gm_num, base_id) == 0):
								#create all mappings cpm, #bpm, #cbpm, lwdm, clm
								insertINTOcar_parts_mapping(car_id, gm_num, subCategory)
								cpm_id = findCPM(gm_num, car_id)
								for wd_id in assWise:
									if(findLWDM(gm_num, wd_id) == 0):
										insertINTOlabours_work_done_mapping(gm_num, wd_id)
									lwdm_id = findLWDM(gm_num, wd_id)
									if(findCLM(cpm_id, lwdm_id) == 0):
										insertINTOcar_labour_mapping(cpm_id, lwdm_id)
							else:
								#mappings exist for base variant
								cpm_id = findCPM(gm_num, base_id)
								for wd_id in assWise:
									if(findLWDM(gm_num, wd_id) == 0):
										insertINTOlabours_work_done_mapping(gm_num, wd_id)
									lwdm_id = findLWDM(gm_num, wd_id)
									if(findCLM(cpm_id, lwdm_id) == 0):
										insertINTOcar_labour_mapping(cpm_id, lwdm_id)
						else:
							cpm_id = findCPM(gm_num, car_id)
							for wd_id in assWise:
								if(findLWDM(gm_num, wd_id) == 0):
									insertINTOlabours_work_done_mapping(gm_num, wd_id)
								lwdm_id = findLWDM(gm_num, wd_id)
								if(findCLM(cpm_id, lwdm_id) == 0):
									insertINTOcar_labour_mapping(cpm_id, lwdm_id)
				else:
					insertINTOsub_category(subCategory, cat_id)
					subCat_id = findSubCat_id(subCategory, cat_id)
					gm_num = createGM(cat_id, subCat_id)
					insertINTOparts_labour(gm_num, subCat_id, hsnCode, taxPercent)
					insertINTOcar_parts_mapping(car_id, gm_num, subCategory)
					cpm_id = findCPM(gm_num, car_id)
					for wd_id in assWise:
						if(findLWDM(gm_num, wd_id) == 0):
							insertINTOlabours_work_done_mapping(gm_num, wd_id)
						lwdm_id = findLWDM(gm_num, wd_id)
						if(findCLM(cpm_id, lwdm_id) == 0):
										insertINTOcar_labour_mapping(cpm_id, lwdm_id)
				#check for partName
				gm_num = findGM(partName, cat_id, subCat_id)
				if(gm_num != ''):
					if(findCPM(gm_num, car_id) == 0):
						if(findCPM(gm_num, base_id) == 0):
							#create all mappings cpm, bpm, cbpm, lwdm, clm
							insertINTOcar_parts_mapping(car_id, gm_num, partName)
							if(findBPM(partNum, gm_num) == 0):
								insertINTObrand_part_mapping(partNum, partBrand, gm_num, partType)
							cpm_id = findCPM(gm_num, car_id)
							bpm_id = findBPM(partNum, gm_num)
							if(findCBPM(cpm_id, bpm_id) == 0):
								insertINTOcar_brand_part_mapping(cpm_id, bpm_id, m_price)
							for wd_id in partWise:
								if(findLWDM(gm_num, wd_id) == 0):
									insertINTOlabours_work_done_mapping(gm_num, wd_id)
								lwdm_id = findLWDM(gm_num, wd_id)
								if(findCLM(cpm_id, lwdm_id) == 0):
									insertINTOcar_labour_mapping(cpm_id, lwdm_id)
						else:
							if(findBPM(partNum, gm_num) == 0):
								insertINTObrand_part_mapping(partNum, partBrand, gm_num, partType)
							cpm_id = findCPM(gm_num, base_id)
							bpm_id = findBPM(partNum, gm_num)
							if(findCBPM(cpm_id, bpm_id) == 0):
								insertINTOcar_brand_part_mapping(cpm_id, bpm_id, m_price)
							for wd_id in partWise:
								if(findLWDM(gm_num, wd_id) == 0):
									insertINTOlabours_work_done_mapping(gm_num, wd_id)
								lwdm_id = findLWDM(gm_num, wd_id)
								if(findCLM(cpm_id, lwdm_id) == 0):
									insertINTOcar_labour_mapping(cpm_id, lwdm_id)
					else:
						if(findBPM(partNum, gm_num) == 0):
							insertINTObrand_part_mapping(partNum, partBrand, gm_num, partType)
						cpm_id = findCPM(gm_num, car_id)
						bpm_id = findBPM(partNum, gm_num)
						if(findCBPM(cpm_id, bpm_id) == 0):
							insertINTOcar_brand_part_mapping(cpm_id, bpm_id, m_price)
						for wd_id in partWise:
							if(findLWDM(gm_num, wd_id) == 0):
								insertINTOlabours_work_done_mapping(gm_num, wd_id)
							lwdm_id = findLWDM(gm_num, wd_id)
							if(findCLM(cpm_id, lwdm_id) == 0):
								insertINTOcar_labour_mapping(cpm_id, lwdm_id)
				else:
					#create gm_num
					gm_num = createGM(cat_id, subCat_id)
					insertINTOparts_labour(gm_num, subCat_id, hsnCode, taxPercent)
					insertINTOcar_parts_mapping(car_id, gm_num, partName)
					#create mappings
					if(findBPM(partNum, gm_num) == 0):
						insertINTObrand_part_mapping(partNum, partBrand, gm_num, partType)
					cpm_id = findCPM(gm_num, car_id)
					bpm_id = findBPM(partNum, gm_num)
					if(findCBPM(cpm_id, bpm_id) == 0):
						insertINTOcar_brand_part_mapping(cpm_id, bpm_id, m_price)
					for wd_id in partWise:
						if(findLWDM(gm_num, wd_id) == 0):
							insertINTOlabours_work_done_mapping(gm_num, wd_id)
						lwdm_id = findLWDM(gm_num, wd_id)
						if(findCLM(cpm_id, lwdm_id) == 0):
							insertINTOcar_labour_mapping(cpm_id, lwdm_id)
			else:
				log_s = "Error_Log: Car Does not Exist."
				logIt(text_file, log_s)
	text_file.close()


# Change HSN code from decimal to non-decimal string

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
