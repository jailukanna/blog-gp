---
title: mysql example scorpio common ui uiDriverManager (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio common ui uiDriverManager'


## python scorpio common ui uiDriverManager

Python mysql example: scorpio common ui uiDriverManager

```python
# coding = utf-8
from common.ui import *


class UIDriverManager(object):
    def __init__(self):
        self.log = BaseLog(UIDriverManager.__name__).log

    def get_driver(self):
        """get WebDriver"""
        self.log.info(">>> init driver ")

        driver = self.__init_driver(DRIVER["type"])

        if driver is None:
            raise Exception("Driver init failed, Driver is None!")

        self.log.info("<<< init done ")
        return driver

    def close(self, driver):
        """close WebDriver"""
        self.log.info("close driver: %s", driver)
        if driver is not None:
            driver.quit()

    def __set_basic_web_property(self, driver):
        self.log.info("__WINDOW: max")
        driver.maximize_window()
        self.log.info("__DEFAULT_FIND_ELEMENT_TIMEOUT: %ds", DEFAULT_FIND_ELEMENT_TIMEOUT)
        driver.implicitly_wait(DEFAULT_FIND_ELEMENT_TIMEOUT)
        self.log.info("__DEFAULT_PAGE_LOAD_TIMEOUT: %ds", DEFAULT_PAGE_LOAD_TIMEOUT)
        driver.set_page_load_timeout(DEFAULT_PAGE_LOAD_TIMEOUT)
        return driver

    def __init_driver(self, driver_type):
        if driver_type == webdriver.Chrome:
            self.log.info("Create Chrome Driver...")
            options = webdriver.ChromeOptions()

            self.log.info("Allow \"Trying to download multiple files\"")
            prefs = {
                'profile.content_settings.exceptions.automatic_downloads.*.setting': 1,  # 允许自动下载多个文件
                "profile.default_content_settings.popups": 0,  # 下载不弹框
                "download.directory_upgrade": True,
                "download.default_directory": DEFAULT_DOWNLOAD_PATH,  # 指定下载路径
            }
            options.add_experimental_option("prefs", prefs)

            self.log.info("Set disable-infobars")
            options.add_argument('disable-infobars')
            driver = webdriver.Chrome(chrome_options=options)  # type: webdriver.Chrome
            return self.__set_basic_web_property(driver)
        elif driver_type == webdriver.Firefox:
            self.log.info("Create FireFox Driver...")
            driver = webdriver.Firefox(log_path=GECKO_DRIVER_LOG_PATH)  # type: webdriver.Firefox
            return self.__set_basic_web_property(driver)
        else:
            raise Exception("unimplemented driver type")


if __name__ == "__main__":
    manager = UIDriverManager()
    test_driver = manager.get_driver()

    test_driver.get("https://www.baidu.com")

    time.sleep(1)

    manager.close(test_driver)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
