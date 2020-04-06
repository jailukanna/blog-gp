---
title: mysql example cosmo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'cosmo'


Modules used in program: 
* `import mysql.connector`
* `import numpy as np`
* `import math`
* `import netCDF4`

## python cosmo

Python mysql example: cosmo

```python
import netCDF4
import math
from netCDF4 import Dataset
import numpy as np
from numpy import array
from datetime import datetime
import mysql.connector


"""
Database schema
+------------------+------------------------+-----------------------+----------------------+--------------------------+------------------------+
| usec             | T06.BARO.P.SUR.001.AVG | T06.HMP.T.AIR.002.AVG | T06.HMP.M.RH.002.AVG | T06.SONIC.V.VHOR.001.AVG | T06.SONIC.V.WD.001.AVG |
+------------------+------------------------+-----------------------+----------------------+--------------------------+------------------------+
| 1364394000000000 |            1000.911011 |              5.143711 |            35.794636 |                  3.05004 |              98.574371 |
+------------------+------------------------+-----------------------+----------------------+--------------------------+------------------------+

"""

SITE_TYPE = "T" # "EB"
SHORT_NAME = "T06"
LONG_NAME = "KITCUBE HDCP2 T06"

EDITION_NUMBER = 4
SECTION1_CENTRE = 78
SECTION1_SUBCENTRE = 0
SECTION1_UPDATE_SEQUENCE_NR = 0
SECTION1_DATA_CATEGORY = 0
SECTION1_INT_DATA_SUB_CATEGORY = 4
#SECTION1_DATE = 20130501
#SECTION1_TIME = 0
YSSOSN = SHORT_NAME
YSOSN = LONG_NAME

MII = 1
NIII = 400
NIX = 0

if SHORT_NAME == "EB1" or SHORT_NAME == "T07":
    LATITUDE = 50.8972
    LONGITUDE = 6.4638
    UNN = 113
elif SHORT_NAME == "EB2" or SHORT_NAME == "T06":
    LATITUDE = 50.8913
    LONGITUDE = 6.4298
    UNN = 97

my_example_nc_file = './cdfin_synop.01.nc'
fh = Dataset(my_example_nc_file, mode='w')

bufr_records = fh.createDimension('BUFR_records', None) # 0 or None means unlimited
loop_000_maxlen = fh.createDimension('Loop_000_maxlen', 4)
loop_001_maxlen = fh.createDimension('Loop_001_maxlen', 3)
loop_002_maxlen = fh.createDimension('Loop_002_maxlen', 3)
loop_003_maxlen = fh.createDimension('Loop_003_maxlen', 2)
loop_004_maxlen = fh.createDimension('Loop_004_maxlen', 2)
loop_005_maxlen = fh.createDimension('Loop_005_maxlen', 2)
loop_006_maxlen = fh.createDimension('Loop_006_maxlen', 2)
loop_007_maxlen = fh.createDimension('Loop_007_maxlen', 18)
section1_length = fh.createDimension('section1_length', 22)
section2_length = fh.createDimension('section2_length', 18)
yssosn_strlen = fh.createDimension('YSSOSN_strlen', 3)
ysosn_strlen = fh.createDimension('YSOSN_strlen', 17)


edition_number = fh.createVariable('edition_number','i4',('BUFR_records',), fill_value=-2147483647)
section1_centre = fh.createVariable('section1_centre','i4',('BUFR_records',), fill_value=-2147483647)
section1_subcentre = fh.createVariable('section1_subcentre','i4',('BUFR_records',), fill_value=-2147483647)
section1_update_sequence_nr = fh.createVariable('section1_update_sequence_nr','i4',('BUFR_records',), fill_value=-2147483647)
section1_data_category = fh.createVariable('section1_data_category','i4',('BUFR_records',), fill_value=-2147483647)
section1_data_subcategory = fh.createVariable('section1_data_subcategory','i4',('BUFR_records',), fill_value=-2147483647)
section1_date = fh.createVariable('section1_date','i4',('BUFR_records',), fill_value=-2147483647)
section1_time = fh.createVariable('section1_time','i4',('BUFR_records',), fill_value=-2147483647)

yssosn = fh.createVariable('YSSOSN','c',('BUFR_records', 'YSSOSN_strlen'))
yssosn.long_name = "SHORT STATION OR SITE NAME"
yssosn.units = "CCITT_IA5"
yssosn.mnemonic = "YSSOSN"
yssosn.type = "Data"

ysosn = fh.createVariable('YSOSN','c',('BUFR_records', 'YSOSN_strlen'))
ysosn.long_name = "STATION OR SITE NAME"
ysosn.units = "CCITT_IA5"
ysosn.mnemonic = "YSSOSN"
ysosn.type = "Data"

mii = fh.createVariable('MII','i4',('BUFR_records',), fill_value=-2147483647)
mii.long_name = "WMO BLOCK NUMBER" 
mii.units = "NUMERIC"
mii.mnemonic = "MII"
mii.type = "Data"

niii = fh.createVariable('NIII','i4',('BUFR_records',), fill_value=-2147483647)
niii.long_name = "WMO STATION NUMBER" 
niii.units = "NUMERIC"
niii.mnemonic = "NIII"
niii.type = "Data"

nix = fh.createVariable('NIX','i4',('BUFR_records',), fill_value=-2147483647)
nix.long_name = "TYPE OF STATION" 
nix.units = "CODE_TABLE"
nix.mnemonic = "NIX"
nix.type = "Data"

mjjj = fh.createVariable('MJJJ','i4',('BUFR_records',), fill_value=-2147483647)
mjjj.long_name = "YEAR" 
mjjj.units = "YEAR"
mjjj.mnemonic = "MJJJ"
mjjj.type = "Data"

mmm = fh.createVariable('MMM','i4',('BUFR_records',), fill_value=-2147483647)
mmm.long_name = "MONTH" 
mmm.units = "MONTH"
mmm.mnemonic = "MMM"
mmm.type = "Data"

myy = fh.createVariable('MYY','i4',('BUFR_records',), fill_value=-2147483647)
myy.long_name = "DAY" 
myy.units = "DAY"
myy.mnemonic = "MYY"
myy.type = "Data"

mgg = fh.createVariable('MGG','i4',('BUFR_records',), fill_value=-2147483647)
mgg.long_name = "HOUR" 
mgg.units = "HOUR"
mgg.mnemonic = "MGG"
mgg.type = "Data"

ngg = fh.createVariable('NGG','i4',('BUFR_records',), fill_value=-2147483647)
ngg.long_name = "MINUTE" 
ngg.units = "MINUTE"
ngg.mnemonic = "NGG"
ngg.type = "Data"

mlah = fh.createVariable('MLAH','d',('BUFR_records',), fill_value=9.96920996838687e+36)
mlah.long_name = "LATITUDE (HIGH ACCURACY)"
mlah.units = "DEGREE"
mlah.mnemonic = "MLAH"
mlah.type = "Data"

mloh = fh.createVariable('MLOH','d',('BUFR_records',), fill_value=9.96920996838687e+36)
mloh.long_name = "LONGITUDE (HIGH ACCURACY)"
mloh.units = "DEGREE"
mloh.mnemonic = "MLOH"
mloh.type = "Data"

mhosnn = fh.createVariable('MHOSNN','f',('BUFR_records',), fill_value=9.96921e+36)
mhosnn.long_name = "HEIGHT OF STATION GROUND ABOVE MEAN SEA"
mhosnn.units = "M"
mhosnn.mnemonic = "MHOSNN"
mhosnn.type = "Data"

mhobnn = fh.createVariable('MHOBNN','f',('BUFR_records',), fill_value=9.96921e+36)
mhobnn.long_name = "HEIGHT OF BAROMETER ABOVE MEAN SEA LEVEL"
mhobnn.units = "M"
mhobnn.mnemonic = "MHOBNN"
mhobnn.type = "Data"

mppp = fh.createVariable('MPPP','f',('BUFR_records',), fill_value=9.96921e+36)
mppp.long_name = "PRESSURE"
mppp.units = "PA"
mppp.mnemonic = "MPPP"
mppp.type = "Data"

mtdbt = fh.createVariable('MTDBT','f',('BUFR_records',), fill_value=9.96921e+36)
mtdbt.long_name = "TEMPERATURE/DRY BULB TEMPERATURE"
mtdbt.units = "K"
mtdbt.mnemonic = "MTDBT"
mtdbt.type = "Data"

mtdnh = fh.createVariable('MTDNH','f',('BUFR_records',), fill_value=9.96921e+36)
mtdnh.long_name = "DEW-POINT TEMPERATURE"
mtdnh.units = "K"
mtdnh.mnemonic = "MTDNH"
mtdnh.type = "Data"

nfnfn = fh.createVariable('NFNFN','f',('BUFR_records',), fill_value=9.96921e+36)
nfnfn.long_name = "WIND SPEED"
nfnfn.units = "M/S"
nfnfn.mnemonic = "NFNFN"
nfnfn.type = "Data"

ndndn = fh.createVariable('NDNDN','i4',('BUFR_records',), fill_value=-2147483647)
ndndn.long_name = "WIND DIRECTION"
ndndn.units = "DEGREE_TRUE"
ndndn.mnemonic = "NDNDN"
ndndn.type = "Data"

"""Data  """
edition_number_list = []
section1_centre_list = []
section1_subcentre_list = []
section1_update_sequence_nr_list = []
section1_data_category_list = []
section1_int_data_sub_category_list = []
section1_date_list = []
section1_time_list = []
yssosn_list = []
ysosn_list = []
mii_list = []
niii_list = []
nix_list = []

mjjj_list = [] # year
mmm_list = [] # month
myy_list = [] # day

mgg_list = [] # hour
ngg_list = [] # minute

mlah_list = []
mloh_list = []
mhosnn_list = []

mppp_list = []
mtdbt_list = []
mtdnh_list = []
nfnfn_list = []
ndndn_list = []

cnx = mysql.connector.connect(user="XXXX", password="XXXX", database="HDCP2", host="imk-db1")
cursor = cnx.cursor()

query = ("SELECT * from CDFIN_T06 LIMIT 2")
cursor.execute(query)

for item in cursor:
    cur_datetime = item[0] / 1000000.0
    utc = datetime.utcfromtimestamp(cur_datetime)
    utc_date_int = int(str(utc.year).zfill(2) + str(utc.month).zfill(2) + str(utc.day).zfill(2))
    utc_time_int = int(str(utc.hour).zfill(2) + str(utc.minute).zfill(2) + str(utc.second).zfill(2))

    pressure = item[1] * 100.0
    T_tmp = item[2]
    T = T_tmp + 273.15
    RH = item[3]
    taupunkt = ( 243.04 * ( np.log(RH/100)+((17.625*T_tmp)/(243.04+T_tmp))) / (17.625 - np.log(RH/100)-((17.625*T_tmp)/(243.04+T_tmp))) ) + 273.15

    if SITE_TYPE == "T":
        wind_velocity = item[4] * 1.51
    elif SITE_TYPE == "EB":
        wind_velocity = item[4] * 1.86

    wind_direction = item[5]

    edition_number_list.append(EDITION_NUMBER)
    section1_centre_list.append(SECTION1_CENTRE)
    section1_subcentre_list.append(SECTION1_SUBCENTRE)
    section1_update_sequence_nr_list.append(SECTION1_UPDATE_SEQUENCE_NR)
    section1_data_category_list.append(SECTION1_DATA_CATEGORY)
    section1_int_data_sub_category_list.append(SECTION1_INT_DATA_SUB_CATEGORY)
    section1_date_list.append(utc_date_int)
    section1_time_list.append(utc_time_int)    
    yssosn_list.append(YSSOSN)
    ysosn_list.append(YSOSN)
    mii_list.append(MII)
    niii_list.append(NIII)
    nix_list.append(NIX)
    mjjj_list.append(utc.year)
    mmm_list.append(utc.month)
    myy_list.append(utc.day)
    mgg_list.append(utc.hour)
    ngg_list.append(utc.minute)
    mlah_list.append(LATITUDE)
    mloh_list.append(LONGITUDE)
    mhosnn_list.append(UNN)
    mppp_list.append(pressure)
    mtdbt_list.append(T)
    mtdnh_list.append(taupunkt)
    nfnfn_list.append(wind_velocity)
    ndndn_list.append(wind_direction)
    print(item)

cursor.close()
cnx.close()

edition_number[:] = edition_number_list
section1_centre[:] = section1_centre_list
section1_subcentre[:] = section1_subcentre_list
section1_update_sequence_nr[:] = section1_update_sequence_nr_list
section1_data_category[:] = section1_data_category_list
section1_data_subcategory[:] = section1_int_data_sub_category_list
section1_date[:] = section1_date_list
section1_time[:] = section1_time_list

yssosn[:] = yssosn_list
ysosn[:] = ysosn_list
mii[:] = mii_list
niii[:] = niii_list
nix[:] = nix_list

mjjj[:] = mjjj_list
mmm[:] = mmm_list
myy[:] = myy_list
mgg[:] = mgg_list
ngg[:] = ngg_list
mloh[:] = mloh_list
mlah[:] = mlah_list
mhosnn[:] = mhosnn_list
mhobnn[:] = mhosnn_list
mppp[:] = mppp_list
mtdbt[:] = mtdbt_list
mtdnh[:] = mtdnh_list
nfnfn[:] = nfnfn_list
ndndn[:] = ndndn_list

fh.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
