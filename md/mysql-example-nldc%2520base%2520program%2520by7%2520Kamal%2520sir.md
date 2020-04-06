---
title: mysql example nldc%2520base%2520program%2520by7%2520Kamal%2520sir (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'nldc%2520base%2520program%2520by7%2520Kamal%2520sir'

Functions in program: 
* `def pdf_to_db():`
* `def Download_pdf():`

Modules used in program: 
* `import mysql.connector`
* `import datetime`
* `import pandas as pd`
* `import tabula `
* `import shutil`
* `import datetime`
* `import db_conn_param`
* `import mysql.connector`
* `import os`
* `import mysql.connector as mariadb`
* `import xlrd`
* `import time`

## python nldc%2520base%2520program%2520by7%2520Kamal%2520sir

Python mysql example: nldc%2520base%2520program%2520by7%2520Kamal%2520sir

```python


import time
import xlrd
import mysql.connector as mariadb
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import Select, WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from time import sleep, strftime
import os
from datetime import date, timedelta
import mysql.connector
from pyvirtualdisplay import Display
import db_conn_param
import datetime
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.action_chains import ActionChains
import shutil
import tabula 
import pandas as pd
import datetime
import mysql.connector
from tabula import wrapper

def Download_pdf():
    
    Date=datetime.datetime.now().strftime("%d-%m-%y")
    Date=str(Date)
    try:
        driver.get('https://posoco.in/download/'+Date+'_nldc_psp/')
    except:
        print('Timeout...Retrying.....')
        time.sleep(900)
        Download_pdf()    
    driver.maximize_window()
    WebDriverWait(driver, 20)
    time.sleep(10)
    try:
        driver.find_element_by_xpath('/html/body/div[1]/div[1]/div/main/article/div/div/div/div[2]/table/tbody/tr[5]/td/a').click()
        time.sleep(10)
        shutil.move("/home/ealserver/Downloads/"+str(datetime.datetime.now().strftime("%d.%m.%y"))+"_NLDC_PSP.pdf", "/home/ealserver/code_11/nldc_daily_reports")
    except:
        print('File Not found...Retrying.....')
        time.sleep(1800)
        Download_pdf()    
    #actions = ActionChains(driver)
    #actions.key_down(Keys.CONTROL).send_keys("s").key_up(Keys.CONTROL).perform()
    #elem = driver.switch_to_active_element()
    #elem.send_keys(Keys.RETURN)
    
    #actions.perform()

def pdf_to_db():
    conn = db_conn_param.conn_db()
    cur=conn.cursor()
    #Date=datetime.datetime.now().strftime("%Y-%m-%d")
    yesterday = date.today() - timedelta(1)
    Date=yesterday.strftime('%Y-%m-%d')
    df = tabula.read_pdf("/home/ealserver/code_11/nldc_daily_reports/"+str(datetime.datetime.now().strftime("%d.%m.%y"))+"_NLDC_PSP.pdf" , encoding="cp932",pages="all", multiple_tables=True)
    #Picked up JAVA_TOOL_OPTIONS: -Dfile.encoding=UTF-8
    Regional_data = df[0]
    Freq = pd.DataFrame(df[1])
    PSPS = pd.DataFrame(df[2])
    ICTE = pd.DataFrame(df[3])
    IER =  pd.DataFrame(df[4])
    GEN_OUT = pd.DataFrame(df[5])
    SOURCE_GEN = pd.DataFrame(df[6])
    #new7 = pd.DataFrame(df[7])

    # Regional_data.to_csv("Regional_data.csv", sep=',')
    # Freq.to_csv("Freq.csv", sep=',')
    # PSPS.to_csv("PSPS.csv", sep=',')
    # ICTE.to_csv("ICTE.csv", sep=',')
    # IER.to_csv("IER.csv", sep=',')
    # GEN_OUT.to_csv("GEN_OUT.csv", sep=',')
    # SOURCE_GEN.to_csv("SOURCE_GEN.csv", sep=',')
    ##new7.to_csv("output7.csv", sep=',')

    NR_peak_demand_met_MW = str(Regional_data[1][2])
    NR_Peak_Shortage_MW = Regional_data[1][4]
    NR_energy_met_MU = Regional_data[1][5]
    NR_Hydro_gen_MU = Regional_data[1][6]
    NR_Wind_gen_MU = Regional_data[1][7]
    NR_Solar_gen_MU = Regional_data[1][8]
    NR_Shortage_MU = Regional_data[1][9]
    NR_Maximum_Demand_Met = Regional_data[1][11]

    #print(NR_peak_demand_met_MW,NR_Peak_Shortage_MW,NR_energy_met_MU,NR_Hydro_gen_MU,NR_Wind_gen_MU,NR_Solar_gen_MU,NR_Shortage_MU,NR_Maximum_Demand_Met)


    WR_peak_demand_met_MW = Regional_data[2][2]
    WR_Peak_Shortage_MW = Regional_data[2][4]
    WR_energy_met_MU = Regional_data[2][5]
    WR_Hydro_gen_MU = Regional_data[2][6]
    WR_Wind_gen_MU = Regional_data[2][7]
    WR_Solar_gen_MU = Regional_data[2][8]
    WR_Shortage_MU = Regional_data[2][9]
    WR_Maximum_Demand_Met = Regional_data[2][11]

    SR_peak_demand_met_MW = Regional_data[3][2]
    SR_Peak_Shortage_MW = Regional_data[3][4]
    SR_energy_met_MU = Regional_data[3][5]
    SR_Hydro_gen_MU = Regional_data[3][6]
    SR_Wind_gen_MU = Regional_data[3][7]
    SR_Solar_gen_MU = Regional_data[3][8]
    SR_Shortage_MU = Regional_data[3][9]
    SR_Maximum_Demand_Met = Regional_data[3][11]

    ER_peak_demand_met_MW = Regional_data[4][2]
    ER_Peak_Shortage_MW = Regional_data[4][4]
    ER_energy_met_MU = Regional_data[4][5]
    ER_Hydro_gen_MU = Regional_data[4][6]
    ER_Wind_gen_MU = Regional_data[4][7]
    ER_Solar_gen_MU = Regional_data[4][8]
    ER_Shortage_MU = Regional_data[4][9]
    ER_Maximum_Demand_Met = Regional_data[4][11]


    NER_peak_demand_met_MW = Regional_data[5][2]
    NER_Peak_Shortage_MW = Regional_data[5][4]
    NER_energy_met_MU = Regional_data[5][5]
    NER_Hydro_gen_MU = Regional_data[5][6]
    NER_Wind_gen_MU = Regional_data[5][7]
    NER_Solar_gen_MU = Regional_data[5][8]
    NER_Shortage_MU = Regional_data[5][9]
    NER_Maximum_Demand_Met = Regional_data[5][11]


    Total_peak_demand_met_MW = Regional_data[6][2]
    Total_Peak_Shortage_MW = Regional_data[6][4]
    Total_energy_met_MU = Regional_data[6][5]
    Total_Hydro_gen_MU = Regional_data[6][6]
    Total_Wind_gen_MU = Regional_data[6][7]
    Total_Solar_gen_MU = Regional_data[6][8]
    Total_Shortage_MU = Regional_data[6][9]
    Total_Maximum_Demand_Met = Regional_data[6][11]

    # PSPS_demand_data_pun= PSPS[3][5]
    # demanddata = PSPS_demand_data_pun.split(" ")
    PSPS_max_dem_day_MW_pun = PSPS[3][5]
    PSPS_sho_dur_Max_dem_pun = PSPS[4][5]
    PSPS_Energy_met_MU_pun = PSPS[5][5]
    PSPS_Draw_MU_pun = PSPS[6][5]
    PSPS_OD_UD_MU_pun = PSPS[7][5]
    PSPS_Max_OD_MW_pun = PSPS[8][5]
    PSPS_Energy_short_MU_pun = PSPS[9][5]




    PSPS_max_dem_day_MW_HR = PSPS[3][6]
    PSPS_sho_dur_Max_dem_HR = PSPS[4][6]
    PSPS_Energy_met_MU_HR = PSPS[5][6]
    PSPS_Draw_MU_HR = PSPS[6][6]
    PSPS_OD_UD_MU_HR = PSPS[7][6]
    PSPS_Max_OD_MW_HR = PSPS[8][6]
    PSPS_Energy_short_MU_HR = PSPS[9][6]



    PSPS_max_dem_day_MW_Raj = PSPS[3][7]
    PSPS_sho_dur_Max_dem_Raj = PSPS[4][7]
    PSPS_Energy_met_MU_Raj = PSPS[5][7]
    PSPS_Draw_MU_Raj = PSPS[6][7]
    PSPS_OD_UD_MU_Raj = PSPS[7][7]
    PSPS_Max_OD_MW_Raj = PSPS[8][7]
    PSPS_Energy_short_MU_Raj = PSPS[9][7]



    PSPS_max_dem_day_MW_DL = PSPS[3][8]
    PSPS_sho_dur_Max_dem_DL = PSPS[4][8]
    PSPS_Energy_met_MU_DL = PSPS[5][8]
    PSPS_Draw_MU_DL = PSPS[6][8]
    PSPS_OD_UD_MU_DL = PSPS[7][8]
    PSPS_Max_OD_MW_DL = PSPS[8][8]
    PSPS_Energy_short_MU_DL = PSPS[9][8]



    PSPS_max_dem_day_MW_UP = PSPS[3][9]
    PSPS_sho_dur_Max_dem_UP = PSPS[4][9]
    PSPS_Energy_met_MU_UP = PSPS[5][9]
    PSPS_Draw_MU_UP = PSPS[6][9]
    PSPS_OD_UD_MU_UP = PSPS[7][9]
    PSPS_Max_OD_MW_UP = PSPS[8][9]
    PSPS_Energy_short_MU_UP = PSPS[9][9]



    PSPS_max_dem_day_MW_UK = PSPS[3][10]
    PSPS_sho_dur_Max_dem_UK = PSPS[4][10]
    PSPS_Energy_met_MU_UK = PSPS[5][10]
    PSPS_Draw_MU_UK = PSPS[6][10]
    PSPS_OD_UD_MU_UK = PSPS[7][10]
    PSPS_Max_OD_MW_UK = PSPS[8][10]
    PSPS_Energy_short_MU_UK = PSPS[9][10]




    PSPS_max_dem_day_MW_HP = PSPS[3][11]
    PSPS_sho_dur_Max_dem_HP = PSPS[4][11]
    PSPS_Energy_met_MU_HP = PSPS[5][11]
    PSPS_Draw_MU_HP = PSPS[6][11]
    PSPS_OD_UD_MU_HP = PSPS[7][11]
    PSPS_Max_OD_MW_HP = PSPS[8][11]
    PSPS_Energy_short_MU_HP = PSPS[9][11]





    PSPS_max_dem_day_MW_JK = PSPS[3][12]
    PSPS_sho_dur_Max_dem_JK = PSPS[4][12]
    PSPS_Energy_met_MU_JK = PSPS[5][12]
    PSPS_Draw_MU_JK = PSPS[6][12]
    PSPS_OD_UD_MU_JK = PSPS[7][12]
    PSPS_Max_OD_MW_JK = PSPS[8][12]
    PSPS_Energy_short_MU_JK = PSPS[9][12]




    PSPS_max_dem_day_MW_CHD = PSPS[3][13]
    PSPS_sho_dur_Max_dem_CHD = PSPS[4][13]
    PSPS_Energy_met_MU_CHD = PSPS[5][13]
    PSPS_Draw_MU_CHD = PSPS[6][13]
    PSPS_OD_UD_MU_CHD = PSPS[7][13]
    PSPS_Max_OD_MW_CHD = PSPS[8][13]
    PSPS_Energy_short_MU_CHD = PSPS[9][13]



    PSPS_max_dem_day_MW_CGR = PSPS[3][14]
    PSPS_sho_dur_Max_dem_CGR = PSPS[4][14]
    PSPS_Energy_met_MU_CGR = PSPS[5][14]
    PSPS_Draw_MU_CGR = PSPS[6][14]
    PSPS_OD_UD_MU_CGR = PSPS[7][14]
    PSPS_Max_OD_MW_CGR = PSPS[8][14]
    PSPS_Energy_short_MU_CGR = PSPS[9][14]



    PSPS_max_dem_day_MW_GJ = PSPS[3][15]
    PSPS_sho_dur_Max_dem_GJ = PSPS[4][15]
    PSPS_Energy_met_MU_GJ = PSPS[5][15]
    PSPS_Draw_MU_GJ = PSPS[6][15]
    PSPS_OD_UD_MU_GJ = PSPS[7][15]
    PSPS_Max_OD_MW_GJ = PSPS[8][15]
    PSPS_Energy_short_MU_GJ = PSPS[9][15]



    PSPS_max_dem_day_MW_MP = PSPS[3][16]
    PSPS_sho_dur_Max_dem_MP = PSPS[4][16]
    PSPS_Energy_met_MU_MP = PSPS[5][16]
    PSPS_Draw_MU_MP = PSPS[6][16]
    PSPS_OD_UD_MU_MP = PSPS[7][16]
    PSPS_Max_OD_MW_MP = PSPS[8][16]
    PSPS_Energy_short_MU_MP = PSPS[9][16]



    PSPS_max_dem_day_MW_MH = PSPS[3][17]
    PSPS_sho_dur_Max_dem_MH = PSPS[4][17]
    PSPS_Energy_met_MU_MH = PSPS[5][17]
    PSPS_Draw_MU_MH = PSPS[6][17]
    PSPS_OD_UD_MU_MH = PSPS[7][17]
    PSPS_Max_OD_MW_MH = PSPS[8][17]
    PSPS_Energy_short_MU_MH = PSPS[9][17]



    PSPS_max_dem_day_MW_GOA=PSPS[3][19]
    PSPS_sho_dur_Max_dem_GOA=PSPS[4][19]
    PSPS_Energy_met_MU_GOA=PSPS[5][19]
    PSPS_Draw_MU_GOA=PSPS[6][19]
    PSPS_OD_UD_MU_GOA=PSPS[7][19]
    PSPS_Max_OD_MW_GOA=PSPS[8][19]
    PSPS_Energy_short_MU_GOA=PSPS[9][19]



    PSPS_max_dem_day_MW_DD=PSPS[3][20]
    PSPS_sho_dur_Max_dem_DD=PSPS[4][20]
    PSPS_Energy_met_MU_DD=PSPS[5][20]
    PSPS_Draw_MU_DD=PSPS[6][20]
    PSPS_OD_UD_MU_DD=PSPS[7][20]
    PSPS_Max_OD_MW_DD=PSPS[8][20]
    PSPS_Energy_short_MU_DD=PSPS[9][20]



    PSPS_max_dem_day_MW_DNH=PSPS[3][21]
    PSPS_sho_dur_Max_dem_DNH=PSPS[4][21]
    PSPS_Energy_met_MU_DNH=PSPS[5][21]
    PSPS_Draw_MU_DNH=PSPS[6][21]
    PSPS_OD_UD_MU_DNH=PSPS[7][21]
    PSPS_Max_OD_MW_DNH=PSPS[8][21]
    PSPS_Energy_short_MU_DNH=PSPS[9][21]




    PSPS_max_dem_day_MW_ES=PSPS[3][22]
    PSPS_sho_dur_Max_dem_ES=PSPS[4][22]
    PSPS_Energy_met_MU_ES=PSPS[5][22]
    PSPS_Draw_MU_ES=PSPS[6][22]
    PSPS_OD_UD_MU_ES=PSPS[7][22]
    PSPS_Max_OD_MW_ES=PSPS[8][22]
    PSPS_Energy_short_MU_ES=PSPS[9][22]



    PSPS_max_dem_day_MW_AP=PSPS[3][23]
    PSPS_sho_dur_Max_dem_AP=PSPS[4][23]
    PSPS_Energy_met_MU_AP=PSPS[5][23]
    PSPS_Draw_MU_AP=PSPS[6][23]
    PSPS_OD_UD_MU_AP=PSPS[7][23]
    PSPS_Max_OD_MW_AP=PSPS[8][23]
    PSPS_Energy_short_MU_AP=PSPS[9][23]



    PSPS_max_dem_day_MW_TG=PSPS[3][24]
    PSPS_sho_dur_Max_dem_TG=PSPS[4][24]
    PSPS_Energy_met_MU_TG=PSPS[5][24]
    PSPS_Draw_MU_TG=PSPS[6][24]
    PSPS_OD_UD_MU_TG=PSPS[7][24]
    PSPS_Max_OD_MW_TG=PSPS[8][24]
    PSPS_Energy_short_MU_TG=PSPS[9][24]



    PSPS_max_dem_day_MW_KAR=PSPS[3][25]
    PSPS_sho_dur_Max_dem_KAR=PSPS[4][25]
    PSPS_Energy_met_MU_KAR=PSPS[5][25]
    PSPS_Draw_MU_KAR=PSPS[6][25]
    PSPS_OD_UD_MU_KAR=PSPS[7][25]
    PSPS_Max_OD_MW_KAR=PSPS[8][25]
    PSPS_Energy_short_MU_KAR=PSPS[9][25]



    PSPS_max_dem_day_MW_KER=PSPS[3][26]
    PSPS_sho_dur_Max_dem_KER=PSPS[4][26]
    PSPS_Energy_met_MU_KER=PSPS[5][26]
    PSPS_Draw_MU_KER=PSPS[6][26]
    PSPS_OD_UD_MU_KER=PSPS[7][26]
    PSPS_Max_OD_MW_KER=PSPS[8][26]
    PSPS_Energy_short_MU_KER=PSPS[9][26]



    PSPS_max_dem_day_MW_TN=PSPS[3][27]
    PSPS_sho_dur_Max_dem_TN=PSPS[4][27]
    PSPS_Energy_met_MU_TN=PSPS[5][27]
    PSPS_Draw_MU_TN=PSPS[6][27]
    PSPS_OD_UD_MU_TN=PSPS[7][27]
    PSPS_Max_OD_MW_TN=PSPS[8][27]
    PSPS_Energy_short_MU_TN=PSPS[9][27]



    PSPS_max_dem_day_MW_PON=PSPS[3][28]
    PSPS_sho_dur_Max_dem_PON=PSPS[4][28]
    PSPS_Energy_met_MU_PON=PSPS[5][28]
    PSPS_Draw_MU_PON=PSPS[6][28]
    PSPS_OD_UD_MU_PON=PSPS[7][28]
    PSPS_Max_OD_MW_PON=PSPS[8][28]
    PSPS_Energy_short_MU_PON=PSPS[9][28]



    PSPS_max_dem_day_MW_BH=PSPS[3][29]
    PSPS_sho_dur_Max_dem_BH=PSPS[4][29]
    PSPS_Energy_met_MU_BH=PSPS[5][29]
    PSPS_Draw_MU_BH=PSPS[6][29]
    PSPS_OD_UD_MU_BH=PSPS[7][29]
    PSPS_Max_OD_MW_BH=PSPS[8][29]
    PSPS_Energy_short_MU_BH=PSPS[9][29]



    PSPS_max_dem_day_MW_DVC=PSPS[3][30]
    PSPS_sho_dur_Max_dem_DVC=PSPS[4][30]
    PSPS_Energy_met_MU_DVC=PSPS[5][30]
    PSPS_Draw_MU_DVC=PSPS[6][30]
    PSPS_OD_UD_MU_DVC=PSPS[7][30]
    PSPS_Max_OD_MW_DVC=PSPS[8][30]
    PSPS_Energy_short_MU_DVC=PSPS[9][30]




    PSPS_max_dem_day_MW_JH=PSPS[3][31]
    PSPS_sho_dur_Max_dem_JH=PSPS[4][31]
    PSPS_Energy_met_MU_JH=PSPS[5][31]
    PSPS_Draw_MU_JH=PSPS[6][31]
    PSPS_OD_UD_MU_JH=PSPS[7][31]
    PSPS_Max_OD_MW_JH=PSPS[8][31]
    PSPS_Energy_short_MU_JH=PSPS[9][31]




    PSPS_max_dem_day_MW_OS=PSPS[3][33]
    PSPS_sho_dur_Max_dem_OS=PSPS[4][33]
    PSPS_Energy_met_MU_OS=PSPS[5][33]
    PSPS_Draw_MU_OS=PSPS[6][33]
    PSPS_OD_UD_MU_OS=PSPS[7][33]
    PSPS_Max_OD_MW_OS=PSPS[8][33]
    PSPS_Energy_short_MU_OS=PSPS[9][33]




    PSPS_max_dem_day_MW_WB=PSPS[3][34]
    PSPS_sho_dur_Max_dem_WB=PSPS[4][34]
    PSPS_Energy_met_MU_WB=PSPS[5][34]
    PSPS_Draw_MU_WB=PSPS[6][34]
    PSPS_OD_UD_MU_WB=PSPS[7][34]
    PSPS_Max_OD_MW_WB=PSPS[8][34]
    PSPS_Energy_short_MU_WB=PSPS[9][34]




    PSPS_max_dem_day_MW_SK=PSPS[3][35]
    PSPS_sho_dur_Max_dem_SK=PSPS[4][35]
    PSPS_Energy_met_MU_SK=PSPS[5][35]
    PSPS_Draw_MU_SK=PSPS[6][35]
    PSPS_OD_UD_MU_SK=PSPS[7][35]
    PSPS_Max_OD_MW_SK=PSPS[8][35]
    PSPS_Energy_short_MU_SK=PSPS[9][35]




    PSPS_max_dem_day_MW_ARP=PSPS[3][36]
    PSPS_sho_dur_Max_dem_ARP=PSPS[4][36]
    PSPS_Energy_met_MU_ARP=PSPS[5][36]
    PSPS_Draw_MU_ARP=PSPS[6][36]
    PSPS_OD_UD_MU_ARP=PSPS[7][36]
    PSPS_Max_OD_MW_ARP=PSPS[8][36]
    PSPS_Energy_short_MU_ARP=PSPS[9][36]





    PSPS_max_dem_day_MW_AS=PSPS[3][37]
    PSPS_sho_dur_Max_dem_AS=PSPS[4][37]
    PSPS_Energy_met_MU_AS=PSPS[5][37]
    PSPS_Draw_MU_AS=PSPS[6][37]
    PSPS_OD_UD_MU_AS=PSPS[7][37]
    PSPS_Max_OD_MW_AS=PSPS[8][37]
    PSPS_Energy_short_MU_AS=PSPS[9][37]




    PSPS_max_dem_day_MW_MN=PSPS[3][38]
    PSPS_sho_dur_Max_dem_MN=PSPS[4][38]
    PSPS_Energy_met_MU_MN=PSPS[5][38]
    PSPS_Draw_MU_MN=PSPS[6][38]
    PSPS_OD_UD_MU_MN=PSPS[7][38]
    PSPS_Max_OD_MW_MN=PSPS[8][38]
    PSPS_Energy_short_MU_MN=PSPS[9][38]




    PSPS_max_dem_day_MW_MG=PSPS[3][39]
    PSPS_sho_dur_Max_dem_MG=PSPS[4][39]
    PSPS_Energy_met_MU_MG=PSPS[5][39]
    PSPS_Draw_MU_MG=PSPS[6][39]
    PSPS_OD_UD_MU_MG=PSPS[7][39]
    PSPS_Max_OD_MW_MG=PSPS[8][39]
    PSPS_Energy_short_MU_MG=PSPS[9][39]




    PSPS_max_dem_day_MW_MZ=PSPS[3][40]
    PSPS_sho_dur_Max_dem_MZ=PSPS[4][40]
    PSPS_Energy_met_MU_MZ=PSPS[5][40]
    PSPS_Draw_MU_MZ=PSPS[6][40]
    PSPS_OD_UD_MU_MZ=PSPS[7][40]
    PSPS_Max_OD_MW_MZ=PSPS[8][40]
    PSPS_Energy_short_MU_MZ=PSPS[9][40]





    PSPS_max_dem_day_MW_NG=PSPS[3][41]
    PSPS_sho_dur_Max_dem_NG=PSPS[4][41]
    PSPS_Energy_met_MU_NG=PSPS[5][41]
    PSPS_Draw_MU_NG=PSPS[6][41]
    PSPS_OD_UD_MU_NG=PSPS[7][41]
    PSPS_Max_OD_MW_NG=PSPS[8][41]
    PSPS_Energy_short_MU_NG=PSPS[9][41]





    PSPS_max_dem_day_MW_TR=PSPS[3][42]
    PSPS_sho_dur_Max_dem_TR=PSPS[4][42]
    PSPS_Energy_met_MU_TR=PSPS[5][42]
    PSPS_Draw_MU_TR=PSPS[6][42]
    PSPS_OD_UD_MU_TR=PSPS[7][42]
    PSPS_Max_OD_MW_TR=PSPS[8][42]
    PSPS_Energy_short_MU_TR=PSPS[9][42]

    Bhutan_Actual_MU = ICTE[1][1]
    Bhutan_Day_Peak_MW = ICTE[1][2]
    Nepal_Actual_MU = ICTE[2][1]
    Nepal_Day_Peak_MW = ICTE[2][2]
    Bangladesh_Actual_MU = ICTE[3][1]
    Bangladesh_Day_Peak_MW = ICTE[3][2]


    NR_SCH_MU = IER[1][1]
    NR_ACT_MU = IER[1][2]
    NR_ODUD_MU = IER[1][3]

    #print(NR_SCH_MU,NR_ACT_MU,NR_ODUD_MU)


    WR_SCH_MU = IER[2][1]
    WR_ACT_MU = IER[2][2]
    WR_ODUD_MU = IER[2][3]

    SR_SCH_MU = IER[3][1]
    SR_ACT_MU = IER[3][2]
    SR_ODUD_MU = IER[3][3]

    ER_SCH_MU = IER[4][1]
    ER_ACT_MU = IER[4][2]
    ER_ODUD_MU = IER[4][3]

    NER_SCH_MU = IER[5][1]
    NER_ACT_MU = IER[5][2]
    NER_ODUD_MU = IER[5][3]

    TOTAL_SCH_MU = IER[6][1]
    TOTAL_ACT_MU = IER[6][2]
    TOTAL_ODUD_MU = IER[6][3]

    NR_CS_MW = GEN_OUT[1][1]
    NR_SS_MW = GEN_OUT[1][2]
    NR_TOTAL_MW = GEN_OUT[1][3]
    #print(NR_CS_MW,NR_SS_MW,NR_TOTAL_MW)

    WR_CS_MW = GEN_OUT[2][1]
    WR_SS_MW = GEN_OUT[2][2]
    WR_TOTAL_MW = GEN_OUT[2][3]

    SR_CS_MW = GEN_OUT[3][1]
    SR_SS_MW = GEN_OUT[3][2]
    SR_TOTAL_MW = GEN_OUT[3][3]

    ER_CS_MW = GEN_OUT[4][1]
    ER_SS_MW = GEN_OUT[4][2]
    ER_TOTAL_MW = GEN_OUT[4][3]

    NER_CS_MW = GEN_OUT[5][1]
    NER_SS_MW = GEN_OUT[5][2]
    NER_TOTAL_MW = GEN_OUT[5][3]

    TOTAL_CS_MW = GEN_OUT[6][1]
    TOTAL_SS_MW = GEN_OUT[6][2]
    TOTAL_TOTAL_MW = GEN_OUT[6][3]

    NR_THER_MW = SOURCE_GEN[1][1]
    NR_HYDRO_MW = SOURCE_GEN[1][2]
    NR_NUCLEAR_MW = SOURCE_GEN[1][3]
    NR_GAS_DES_MW = SOURCE_GEN[1][4]
    NR_RES_MW = SOURCE_GEN[1][5]

    #print(NR_THER_MW,NR_HYDRO_MW,NR_NUCLEAR_MW,NR_GAS_DES_MW,NR_RES_MW)

    WR_THER_MW = SOURCE_GEN[2][1]
    WR_HYDRO_MW = SOURCE_GEN[2][2]
    WR_NUCLEAR_MW = SOURCE_GEN[2][3]
    WR_GAS_DES_MW = SOURCE_GEN[2][4]
    WR_RES_MW = SOURCE_GEN[2][5]


    SR_THER_MW = SOURCE_GEN[3][1]
    SR_HYDRO_MW = SOURCE_GEN[3][2]
    SR_NUCLEAR_MW = SOURCE_GEN[3][3]
    SR_GAS_DES_MW = SOURCE_GEN[3][4]
    SR_RES_MW = SOURCE_GEN[3][5]

    ER_THER_MW = SOURCE_GEN[4][1]
    ER_HYDRO_MW = SOURCE_GEN[4][2]
    ER_NUCLEAR_MW = SOURCE_GEN[4][3]
    ER_GAS_DES_MW = SOURCE_GEN[4][4]
    ER_RES_MW = SOURCE_GEN[4][5]

    NER_THER_MW = SOURCE_GEN[5][1]
    NER_HYDRO_MW = SOURCE_GEN[5][2]
    NER_NUCLEAR_MW = SOURCE_GEN[5][3]
    NER_GAS_DES_MW = SOURCE_GEN[5][4]
    NER_RES_MW = SOURCE_GEN[5][5]

    TOTAL_THER_MW = SOURCE_GEN[6][1]
    TOTAL_HYDRO_MW = SOURCE_GEN[6][2]
    TOTAL_NUCLEAR_MW = SOURCE_GEN[6][3]
    TOTAL_GAS_DES_MW = SOURCE_GEN[6][4]
    TOTAL_RES_MW = SOURCE_GEN[6][5]



    cur.execute('''INSERT into posoco_dailyreport_regional (Date,demand_evening_peak_MW,peak_shortage_MW,energy_met_MU,hydro_gen_MU,wind_gen_MU,solar_gen_MU,energy_shortage_MU,max_demand_day_MW,schedule_MU,actual_MU,odud_MU,central_sector_MW,state_sector_MW,total_gen_outage_MW,thermal_sourcewise_gen_MU,hydro_sourcewise_gen_MU,nuclear_sourcewise_gen_MU,gas_naptha_diesel_sourcewise_gen_MU,res_sourcewise_gen_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,NR_peak_demand_met_MW,NR_Peak_Shortage_MW,NR_energy_met_MU,NR_Hydro_gen_MU,NR_Wind_gen_MU,NR_Solar_gen_MU,NR_Shortage_MU,NR_Maximum_Demand_Met,NR_SCH_MU,NR_ACT_MU,NR_ODUD_MU,NR_CS_MW,NR_SS_MW,NR_TOTAL_MW,NR_THER_MW,NR_HYDRO_MW,NR_NUCLEAR_MW,NR_GAS_DES_MW,NR_RES_MW))

    cur.execute('''INSERT into posoco_dailyreport_regional (Date,demand_evening_peak_MW,peak_shortage_MW,energy_met_MU,hydro_gen_MU,wind_gen_MU,solar_gen_MU,energy_shortage_MU,max_demand_day_MW,schedule_MU,actual_MU,odud_MU,central_sector_MW,state_sector_MW,total_gen_outage_MW,thermal_sourcewise_gen_MU,hydro_sourcewise_gen_MU,nuclear_sourcewise_gen_MU,gas_naptha_diesel_sourcewise_gen_MU,res_sourcewise_gen_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,WR_peak_demand_met_MW,WR_Peak_Shortage_MW,WR_energy_met_MU,WR_Hydro_gen_MU,WR_Wind_gen_MU,WR_Solar_gen_MU,WR_Shortage_MU,WR_Maximum_Demand_Met,WR_SCH_MU,WR_ACT_MU,WR_ODUD_MU,WR_CS_MW,WR_SS_MW,WR_TOTAL_MW,WR_THER_MW,WR_HYDRO_MW,WR_NUCLEAR_MW,WR_GAS_DES_MW,WR_RES_MW))

    cur.execute('''INSERT into posoco_dailyreport_regional (Date,demand_evening_peak_MW,peak_shortage_MW,energy_met_MU,hydro_gen_MU,wind_gen_MU,solar_gen_MU,energy_shortage_MU,max_demand_day_MW,schedule_MU,actual_MU,odud_MU,central_sector_MW,state_sector_MW,total_gen_outage_MW,thermal_sourcewise_gen_MU,hydro_sourcewise_gen_MU,nuclear_sourcewise_gen_MU,gas_naptha_diesel_sourcewise_gen_MU,res_sourcewise_gen_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,SR_peak_demand_met_MW,SR_Peak_Shortage_MW,SR_energy_met_MU,SR_Hydro_gen_MU,SR_Wind_gen_MU,SR_Solar_gen_MU,SR_Shortage_MU,SR_Maximum_Demand_Met,SR_SCH_MU,SR_ACT_MU,SR_ODUD_MU,SR_CS_MW,SR_SS_MW,SR_TOTAL_MW,SR_THER_MW,SR_HYDRO_MW,SR_NUCLEAR_MW,SR_GAS_DES_MW,SR_RES_MW))

    cur.execute('''INSERT into posoco_dailyreport_regional (Date,demand_evening_peak_MW,peak_shortage_MW,energy_met_MU,hydro_gen_MU,wind_gen_MU,solar_gen_MU,energy_shortage_MU,max_demand_day_MW,schedule_MU,actual_MU,odud_MU,central_sector_MW,state_sector_MW,total_gen_outage_MW,thermal_sourcewise_gen_MU,hydro_sourcewise_gen_MU,nuclear_sourcewise_gen_MU,gas_naptha_diesel_sourcewise_gen_MU,res_sourcewise_gen_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,ER_peak_demand_met_MW,ER_Peak_Shortage_MW,ER_energy_met_MU,ER_Hydro_gen_MU,ER_Wind_gen_MU,ER_Solar_gen_MU,ER_Shortage_MU,ER_Maximum_Demand_Met,ER_SCH_MU,ER_ACT_MU,ER_ODUD_MU,ER_CS_MW,ER_SS_MW,ER_TOTAL_MW,ER_THER_MW,ER_HYDRO_MW,ER_NUCLEAR_MW,ER_GAS_DES_MW,ER_RES_MW))

    cur.execute('''INSERT into posoco_dailyreport_regional (Date,demand_evening_peak_MW,peak_shortage_MW,energy_met_MU,hydro_gen_MU,wind_gen_MU,solar_gen_MU,energy_shortage_MU,max_demand_day_MW,schedule_MU,actual_MU,odud_MU,central_sector_MW,state_sector_MW,total_gen_outage_MW,thermal_sourcewise_gen_MU,hydro_sourcewise_gen_MU,nuclear_sourcewise_gen_MU,gas_naptha_diesel_sourcewise_gen_MU,res_sourcewise_gen_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,NER_peak_demand_met_MW,NER_Peak_Shortage_MW,NER_energy_met_MU,NER_Hydro_gen_MU,NER_Wind_gen_MU,NER_Solar_gen_MU,NER_Shortage_MU,NER_Maximum_Demand_Met,NER_SCH_MU,NER_ACT_MU,NER_ODUD_MU,NER_CS_MW,NER_SS_MW,NER_TOTAL_MW,NER_THER_MW,NER_HYDRO_MW,NER_NUCLEAR_MW,NER_GAS_DES_MW,NER_RES_MW))

    cur.execute('''INSERT into posoco_dailyreport_country (Date,country,actual_MU,day_peak_MW) values (%s,%s,%s,%s)''',(Date,'bhutan',Bhutan_Actual_MU,Bhutan_Day_Peak_MW))
    cur.execute('''INSERT into posoco_dailyreport_country (Date,country,actual_MU,day_peak_MW) values (%s,%s,%s,%s)''',(Date,'nepal',Nepal_Actual_MU,Nepal_Day_Peak_MW))
    cur.execute('''INSERT into posoco_dailyreport_country (Date,country,actual_MU,day_peak_MW) values (%s,%s,%s,%s)''',(Date,'bangladesh',Bangladesh_Actual_MU,Bangladesh_Day_Peak_MW))


    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NR','pun',PSPS_max_dem_day_MW_pun,PSPS_sho_dur_Max_dem_pun,PSPS_Energy_met_MU_pun,PSPS_Draw_MU_pun,PSPS_OD_UD_MU_pun,PSPS_Max_OD_MW_pun,PSPS_Energy_short_MU_pun))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NR','HR',PSPS_max_dem_day_MW_HR,PSPS_sho_dur_Max_dem_HR,PSPS_Energy_met_MU_HR,PSPS_Draw_MU_HR,PSPS_OD_UD_MU_HR,PSPS_Max_OD_MW_HR,PSPS_Energy_short_MU_HR))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NR','Raj',PSPS_max_dem_day_MW_Raj,PSPS_sho_dur_Max_dem_Raj,PSPS_Energy_met_MU_Raj,PSPS_Draw_MU_Raj,PSPS_OD_UD_MU_Raj,PSPS_Max_OD_MW_Raj,PSPS_Energy_short_MU_Raj))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NR','DL',PSPS_max_dem_day_MW_DL,PSPS_sho_dur_Max_dem_DL,PSPS_Energy_met_MU_DL,PSPS_Draw_MU_DL,PSPS_OD_UD_MU_DL,PSPS_Max_OD_MW_DL,PSPS_Energy_short_MU_DL))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NR','UP',PSPS_max_dem_day_MW_UP,PSPS_sho_dur_Max_dem_UP,PSPS_Energy_met_MU_UP,PSPS_Draw_MU_UP,PSPS_OD_UD_MU_UP,PSPS_Max_OD_MW_UP,PSPS_Energy_short_MU_UP))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NR','UK',PSPS_max_dem_day_MW_UK,PSPS_sho_dur_Max_dem_UK,PSPS_Energy_met_MU_UK,PSPS_Draw_MU_UK,PSPS_OD_UD_MU_UK,PSPS_Max_OD_MW_UK,PSPS_Energy_short_MU_UK))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NR','HP',PSPS_max_dem_day_MW_HP,PSPS_sho_dur_Max_dem_HP,PSPS_Energy_met_MU_HP,PSPS_Draw_MU_HP,PSPS_OD_UD_MU_HP,PSPS_Max_OD_MW_HP,PSPS_Energy_short_MU_HP))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NR','JK',PSPS_max_dem_day_MW_JK,PSPS_sho_dur_Max_dem_JK,PSPS_Energy_met_MU_JK,PSPS_Draw_MU_JK,PSPS_OD_UD_MU_JK,PSPS_Max_OD_MW_JK,PSPS_Energy_short_MU_JK))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NR','CHD',PSPS_max_dem_day_MW_CHD,PSPS_sho_dur_Max_dem_CHD,PSPS_Energy_met_MU_CHD,PSPS_Draw_MU_CHD,PSPS_OD_UD_MU_CHD,PSPS_Max_OD_MW_CHD,PSPS_Energy_short_MU_CHD))

    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'WR','CGR',PSPS_max_dem_day_MW_CGR,PSPS_sho_dur_Max_dem_CGR,PSPS_Energy_met_MU_CGR,PSPS_Draw_MU_CGR,PSPS_OD_UD_MU_CGR,PSPS_Max_OD_MW_CGR,PSPS_Energy_short_MU_CGR))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'WR','GJ',PSPS_max_dem_day_MW_GJ,PSPS_sho_dur_Max_dem_GJ,PSPS_Energy_met_MU_GJ,PSPS_Draw_MU_GJ,PSPS_OD_UD_MU_GJ,PSPS_Max_OD_MW_GJ,PSPS_Energy_short_MU_GJ))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'WR','MP',PSPS_max_dem_day_MW_MP,PSPS_sho_dur_Max_dem_MP,PSPS_Energy_met_MU_MP,PSPS_Draw_MU_MP,PSPS_OD_UD_MU_MP,PSPS_Max_OD_MW_MP,PSPS_Energy_short_MU_MP))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'WR','MH',PSPS_max_dem_day_MW_MH,PSPS_sho_dur_Max_dem_MH,PSPS_Energy_met_MU_MH,PSPS_Draw_MU_MH,PSPS_OD_UD_MU_MH,PSPS_Max_OD_MW_MH,PSPS_Energy_short_MU_MH))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'WR','GOA',PSPS_max_dem_day_MW_GOA,PSPS_sho_dur_Max_dem_GOA,PSPS_Energy_met_MU_GOA,PSPS_Draw_MU_GOA,PSPS_OD_UD_MU_GOA,PSPS_Max_OD_MW_GOA,PSPS_Energy_short_MU_GOA))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'WR','DD',PSPS_max_dem_day_MW_DD,PSPS_sho_dur_Max_dem_DD,PSPS_Energy_met_MU_DD,PSPS_Draw_MU_DD,PSPS_OD_UD_MU_DD,PSPS_Max_OD_MW_DD,PSPS_Energy_short_MU_DD))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'WR','DNH',PSPS_max_dem_day_MW_DNH,PSPS_sho_dur_Max_dem_DNH,PSPS_Energy_met_MU_DNH,PSPS_Draw_MU_DNH,PSPS_OD_UD_MU_DNH,PSPS_Max_OD_MW_DNH,PSPS_Energy_short_MU_DNH))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'WR','ES',PSPS_max_dem_day_MW_ES,PSPS_sho_dur_Max_dem_ES,PSPS_Energy_met_MU_ES,PSPS_Draw_MU_ES,PSPS_OD_UD_MU_ES,PSPS_Max_OD_MW_ES,PSPS_Energy_short_MU_ES))

    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'SR','AP',PSPS_max_dem_day_MW_AP,PSPS_sho_dur_Max_dem_AP,PSPS_Energy_met_MU_AP,PSPS_Draw_MU_AP,PSPS_OD_UD_MU_AP,PSPS_Max_OD_MW_AP,PSPS_Energy_short_MU_AP))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'SR','TG',PSPS_max_dem_day_MW_TG,PSPS_sho_dur_Max_dem_TG,PSPS_Energy_met_MU_TG,PSPS_Draw_MU_TG,PSPS_OD_UD_MU_TG,PSPS_Max_OD_MW_TG,PSPS_Energy_short_MU_TG))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'SR','KAR',PSPS_max_dem_day_MW_KAR,PSPS_sho_dur_Max_dem_KAR,PSPS_Energy_met_MU_KAR,PSPS_Draw_MU_KAR,PSPS_OD_UD_MU_KAR,PSPS_Max_OD_MW_KAR,PSPS_Energy_short_MU_KAR))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'SR','KER',PSPS_max_dem_day_MW_KER,PSPS_sho_dur_Max_dem_KER,PSPS_Energy_met_MU_KER,PSPS_Draw_MU_KER,PSPS_OD_UD_MU_KER,PSPS_Max_OD_MW_KER,PSPS_Energy_short_MU_KER))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'SR','TN',PSPS_max_dem_day_MW_TN,PSPS_sho_dur_Max_dem_TN,PSPS_Energy_met_MU_TN,PSPS_Draw_MU_TN,PSPS_OD_UD_MU_TN,PSPS_Max_OD_MW_TN,PSPS_Energy_short_MU_TN))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'SR','PON',PSPS_max_dem_day_MW_PON,PSPS_sho_dur_Max_dem_PON,PSPS_Energy_met_MU_PON,PSPS_Draw_MU_PON,PSPS_OD_UD_MU_PON,PSPS_Max_OD_MW_PON,PSPS_Energy_short_MU_PON))

    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'ER','BH',PSPS_max_dem_day_MW_BH,PSPS_sho_dur_Max_dem_BH,PSPS_Energy_met_MU_BH,PSPS_Draw_MU_BH,PSPS_OD_UD_MU_BH,PSPS_Max_OD_MW_BH,PSPS_Energy_short_MU_BH))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'ER','DVC',PSPS_max_dem_day_MW_DVC,PSPS_sho_dur_Max_dem_DVC,PSPS_Energy_met_MU_DVC,PSPS_Draw_MU_DVC,PSPS_OD_UD_MU_DVC,PSPS_Max_OD_MW_DVC,PSPS_Energy_short_MU_DVC))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'ER','JH',PSPS_max_dem_day_MW_JH,PSPS_sho_dur_Max_dem_JH,PSPS_Energy_met_MU_JH,PSPS_Draw_MU_JH,PSPS_OD_UD_MU_JH,PSPS_Max_OD_MW_JH,PSPS_Energy_short_MU_JH))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'ER','OS',PSPS_max_dem_day_MW_OS,PSPS_sho_dur_Max_dem_OS,PSPS_Energy_met_MU_OS,PSPS_Draw_MU_OS,PSPS_OD_UD_MU_OS,PSPS_Max_OD_MW_OS,PSPS_Energy_short_MU_OS))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'ER','WB',PSPS_max_dem_day_MW_WB,PSPS_sho_dur_Max_dem_WB,PSPS_Energy_met_MU_WB,PSPS_Draw_MU_WB,PSPS_OD_UD_MU_WB,PSPS_Max_OD_MW_WB,PSPS_Energy_short_MU_WB))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'ER','SK',PSPS_max_dem_day_MW_SK,PSPS_sho_dur_Max_dem_SK,PSPS_Energy_met_MU_SK,PSPS_Draw_MU_SK,PSPS_OD_UD_MU_SK,PSPS_Max_OD_MW_SK,PSPS_Energy_short_MU_SK))

    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NER','ARP',PSPS_max_dem_day_MW_ARP,PSPS_sho_dur_Max_dem_ARP,PSPS_Energy_met_MU_ARP,PSPS_Draw_MU_ARP,PSPS_OD_UD_MU_ARP,PSPS_Max_OD_MW_ARP,PSPS_Energy_short_MU_ARP))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NER','AS',PSPS_max_dem_day_MW_AS,PSPS_sho_dur_Max_dem_AS,PSPS_Energy_met_MU_AS,PSPS_Draw_MU_AS,PSPS_OD_UD_MU_AS,PSPS_Max_OD_MW_AS,PSPS_Energy_short_MU_AS))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NER','MN',PSPS_max_dem_day_MW_MN,PSPS_sho_dur_Max_dem_MN,PSPS_Energy_met_MU_MN,PSPS_Draw_MU_MN,PSPS_OD_UD_MU_MN,PSPS_Max_OD_MW_MN,PSPS_Energy_short_MU_MN))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NER','MG',PSPS_max_dem_day_MW_MG,PSPS_sho_dur_Max_dem_MG,PSPS_Energy_met_MU_MG,PSPS_Draw_MU_MG,PSPS_OD_UD_MU_MG,PSPS_Max_OD_MW_MG,PSPS_Energy_short_MU_MG))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NER','MZ',PSPS_max_dem_day_MW_MZ,PSPS_sho_dur_Max_dem_MZ,PSPS_Energy_met_MU_MZ,PSPS_Draw_MU_MZ,PSPS_OD_UD_MU_MZ,PSPS_Max_OD_MW_MZ,PSPS_Energy_short_MU_MZ))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NER','NG',PSPS_max_dem_day_MW_NG,PSPS_sho_dur_Max_dem_NG,PSPS_Energy_met_MU_NG,PSPS_Draw_MU_NG,PSPS_OD_UD_MU_NG,PSPS_Max_OD_MW_NG,PSPS_Energy_short_MU_NG))
    cur.execute('''INSERT into posoco_dailyreport_states (Date,region,state,max_demand_MW,shortage_during_max_demand_MW,energy_met_MU,drawal_schedule_MU,od_ud_MU,max_od_MW,energy_shortage_MU) values (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)''',(Date,'NER','TR',PSPS_max_dem_day_MW_TR,PSPS_sho_dur_Max_dem_TR,PSPS_Energy_met_MU_TR,PSPS_Draw_MU_TR,PSPS_OD_UD_MU_TR,PSPS_Max_OD_MW_TR,PSPS_Energy_short_MU_TR))
    conn.commit()


 
if __name__ == '__main__':
        
    #display = Display(visible=0, size=(1366, 768))
    #display.start()
    driver = webdriver.Chrome("/usr/local/bin/chromedriver")
    driver.wait = WebDriverWait(driver, 5)
    driver.get("chrome://settings/content/pdfDocuments")
    actions = ActionChains(driver)
    actions.send_keys(Keys.TAB)
    actions.send_keys(Keys.TAB)
    actions.send_keys(Keys.TAB)
    actions.send_keys(Keys.TAB)
    actions.send_keys(Keys.TAB)
    actions.send_keys(Keys.RETURN)
    actions.perform()
    Download_pdf()
    time.sleep(10)
    pdf_to_db()
    time.sleep(30)
    display.stop()
    driver.quit()
    
    
nldc_dailyreport.py
Open with
Displaying nldc_dailyreport.py.

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
