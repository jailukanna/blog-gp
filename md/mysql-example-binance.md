---
title: mysql example binance (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'binance'

Functions in program: 
* `def main():`
* `def doLogin(driver, username, password, otc_pass, count=0):`
* `def solveCaptcha(driver):`
* `def get_firefox_drive(driver_path=None):`
* `def get_chrome_drive(driver_path=None):`

Modules used in program: 
* `import time`
* `import sys`
* `import mysql.connector`
* `import base64`
* `import pyotp`
* `import os`
* `import json`
* `import calendar`

## python binance

Python mysql example: binance

```python
#!/usr/bin/env python3

# Usage:
# ./binance.py <email> <senha> <otp> <wallet to test json>

from solver import PuzleSolver
import calendar
import json
import os
import pyotp
import base64
import mysql.connector
from pyvirtualdisplay import Display
from selenium import webdriver
from selenium.common.exceptions import TimeoutException
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.common.action_chains import ActionChains
import sys
import time

def get_chrome_drive(driver_path=None):
    base_dir = os.path.dirname(os.path.abspath(__file__))
    log_path = os.path.join(base_dir, 'chromedriver.log')
 
    if driver_path is None:
        driver_path = '/usr/bin/chromedriver'
        pass

    options = webdriver.ChromeOptions()
    # options.headless = True
    # options.add_argument('--hide-scrollbars')
    options.add_argument('--no-sandbox')
    options.add_extension(base_dir + '/../plugin_editado.zip')
 
    driver = webdriver.Chrome(
                              executable_path=driver_path,
                              chrome_options=options,
                              service_args=[
                              # '--log-path={}'.format(log_path),
                              # '--verbose',
                              ]
                              )
 
    return driver
 
def get_firefox_drive(driver_path=None):
    base_dir = os.path.dirname(os.path.abspath(__file__))
    log_path = os.path.join(base_dir, 'geckodriver.log')
 
    if driver_path is None:
        driver_path = '/usr/bin/geckodriver'
        pass
 
    options = webdriver.FirefoxOptions()
    # options.add_argument('-headless')
 
    driver = webdriver.Firefox(
                               executable_path=driver_path,
                               firefox_options=options
                               )
 
    return driver

def solveCaptcha(driver):
    base_dir = os.path.dirname(os.path.abspath(__file__))
    
    imgSlice =  driver.execute_script('return document.querySelector(".geetest_canvas_slice.geetest_absolute").toDataURL("image/png").substring(22);')
    
    f = open(base_dir + "/imgSlice.png", "wb")
    f.write(base64.b64decode(imgSlice))
    f.close()

    imgBg =  driver.execute_script('return document.querySelector(".geetest_canvas_bg.geetest_absolute").toDataURL("image/png").substring(22);')

    f = open(base_dir + "/imgBg.png", "wb")
    f.write(base64.b64decode(imgBg))
    f.close()

    solver = PuzleSolver(base_dir + "/imgSlice.png", base_dir + "/imgBg.png")
    solution = solver.get_position()
    
    time.sleep(3)

    sliderBtn = driver.find_element_by_css_selector(".geetest_slider_button")
    ActionChains(driver).drag_and_drop_by_offset(sliderBtn, solution, 0).perform()

    try:
        WebDriverWait(driver, 5).until(EC.presence_of_element_located((By.XPATH, '//*[@id="__next"]/div/main/div/div/div/div/div/span/div/div[3]/div/div[2]/div[2]/div[2]/div/input[1]')))
        return True
    except TimeoutException:
        print("Captcha error")
        return solveCaptcha(driver)

def doLogin(driver, username, password, otc_pass, count=0):
    count += 1

    if (count > 3):
        return False

    driver.get("https://www.binance.com/en/login")

    time.sleep(5)

    # login
    elem = driver.find_element_by_css_selector("#login_input_email")
    elem.clear()
    elem.send_keys(username)
    
    # password
    elem = driver.find_element_by_css_selector("#login_input_password")
    elem.clear()
    elem.send_keys(password)

    # send
    driver.execute_script('document.querySelector("#login_input_login").click();')
    
    time.sleep(5)

    base_dir = os.path.dirname(os.path.abspath(__file__))
    
    solveCaptcha(driver)

    totp = pyotp.TOTP(otc_pass)

    otp = list(totp.now())
    
    elem = driver.find_element_by_xpath('//*[@id="__next"]/div/main/div/div/div/div/div/span/div/div[3]/div/div[2]/div[2]/div[2]/div/input[1]')
    elem.send_keys(otp[0])

    elem = driver.find_element_by_xpath('//*[@id="__next"]/div/main/div/div/div/div/div/span/div/div[3]/div/div[2]/div[2]/div[2]/div/input[2]')
    elem.send_keys(otp[1])

    elem = driver.find_element_by_xpath('//*[@id="__next"]/div/main/div/div/div/div/div/span/div/div[3]/div/div[2]/div[2]/div[2]/div/input[3]')
    elem.send_keys(otp[2])

    elem = driver.find_element_by_xpath('//*[@id="__next"]/div/main/div/div/div/div/div/span/div/div[3]/div/div[2]/div[2]/div[2]/div/input[4]')
    elem.send_keys(otp[3])

    elem = driver.find_element_by_xpath('//*[@id="__next"]/div/main/div/div/div/div/div/span/div/div[3]/div/div[2]/div[2]/div[2]/div/input[5]')
    elem.send_keys(otp[4])

    elem = driver.find_element_by_xpath('//*[@id="__next"]/div/main/div/div/div/div/div/span/div/div[3]/div/div[2]/div[2]/div[2]/div/input[6]')
    elem.send_keys(otp[5])

    try:
        WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.XPATH, '/html/body/div[4]/div/div[2]/div/div[2]/div/div/div[1]/div[2]/h3')))
        print("Device need release aproval, calling php")

        time.sleep(10)

        link = ""
        
        while (link == ""):
            link = os.popen("php " + base_dir + "/imapValidateNewDevice.php").read();

            if (link == ""):
                print("Retrying")
                time.sleep(5)
        
        print("Link received: " + link)

        driver.get(link)
        time.sleep(3)

        return doLogin(driver, username, password, otc_pass, count)
    except TimeoutException:
        print("Device seems to be released!")


    return True

def main():
    username = sys.argv[1]
    password = sys.argv[2]
    otc_pass = sys.argv[3]
    tmpFile = sys.argv[4]

    file = open("/tmp/test_" + tmpFile + ".json", 'r+')
    dataJson = json.loads(file.read(-1))

    mydb = mysql.connector.connect(
        host="server.com.br",
        user="user",
        passwd="pass",
        database="db"
    )

    mycursor = mydb.cursor()

    # display = Display(visible=0, size=(1024, 768))
    # display.start()

    driver = get_chrome_drive()
    driver.set_window_size(1024, 768)

    if (doLogin(driver, username, password, otc_pass)):
        try:
            WebDriverWait(driver, 30).until(EC.presence_of_element_located((By.ID, 'dashboard-balanceDetail-withdraw')))
            
            for coin, markets in dataJson['binance'].items():
                print("Testing coin ", coin)
                driver.get("https://www.binance.com/en/usercenter/wallet/withdrawals/" + coin)
                time.sleep(3)

                for market in markets:
                    elem = driver.find_element_by_css_selector("input[name=address]")
                    elem.clear()
                    elem.send_keys(market['wallet'])

                    time.sleep(1)

                    validation = driver.execute_script('return document.querySelector("input[name=address]").parentElement.parentElement.parentElement.parentElement.parentElement.children[2].innerText;')

                    check = True
                    if (validation == "Please enter a valid address"):
                        check = False
                        sql = "UPDATE markets SET trade_status = 0 WHERE id = " + market['market']
                    else:
                        sql = "UPDATE markets SET trade_status = 1 WHERE id = " + market['market']

                    mycursor.execute(sql)
                    mydb.commit()

                    print("Market #", market['market'], " [", market['coin'], "] (", market['wallet'], ') = ', check)

                    time.sleep(1)


        except TimeoutException:
            print("Something went wrong")
    
    driver.close()
    # display.popen.kill()
    print("Display closed!")

    return

if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
