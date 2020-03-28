---
title: tkinter example movie ticket stalker (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'movie ticket stalker'

Functions in program: 
* `def center_window(root, width, height):`
* `def get_window_size(window):`
* `def get_screen_size(window):`

Modules used in program: 
* `import re, time`
* `import json`
* `import Tkinter`
* `import scrapy`

## python movie ticket stalker

Python tkinter example: movie ticket stalker

```python
# -*- coding: utf-8 -*-
import scrapy
import Tkinter
from scrapy.shell import inspect_response
import json
from scrapy.selector import Selector

import re, time


wechat_url = 'http://weixin.sogou.com/weixin?type=1&query=%E7%94%B5%E5%BD%B1%E4%B8%8D%E6%97%A0%E8%81%8A&ie=utf8'
text_to_detect = '活动'


current_milli_time = lambda: int(round(time.time() * 1000))

class MovieTicketStalkerSpider(scrapy.Spider):
    user_agent = 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.112 Safari/537.36'
    name = "movie_ticket_stalker"
    allowed_domains = ["weixin.sogou.com"]
    start_urls = (
        wechat_url,
    )

    def parse(self, response):

        results_div = Selector(response).xpath('//div[@class="results mt7"]/div')
        
        for div in results_div:
            div_id = div.xpath('@id').extract()
            latest_artile_id = self.latest_artile_id_for_div(div_id[0])

            latest_artile_title = div.xpath('//a[@id="{}"]/text()'.format(latest_artile_id)).extract()[0]
            latest_artile_title = latest_artile_title.encode('utf8')
            self.logger.info('latest_artile_title is: ' + latest_artile_title)
            
            
            latest_artile_date_script = div.xpath('//span[@class="{}"]/script/text()'.format('hui')).extract()[0]
            self.logger.info('latest_artile_date_script is: ' + latest_artile_date_script)
            
            matchResult = re.search(r"'(\d+)'", latest_artile_date_script)


            if matchResult:
                
                if self.hasTicketActivity(str(latest_artile_title)) and self.isNewArticle(int(matchResult.group(1)) * 1000):
                    self.showMsg('开始抢票啦: {}'.format(latest_artile_title))
            
            
            
            
        
    def latest_artile_id_for_div(self, div_id):
        
        link_id = re.sub(r'box', r'link_first', div_id)
        
        return link_id
        
    def hasTicketActivity(self, title):
        # 有“活动”，没“获奖名单”，则表示有电影票抢
        # 有“活动”，有“获奖名单”，则表示抢电影票结果出来了
        matchResult = re.search(text_to_detect, title)
        

        if matchResult:
            return True
        
        
        return False
        
    def isNewArticle(self, artcle_milli_time):
        
            
        now_milli_time = current_milli_time()
        
        print(now_milli_time, artcle_milli_time)
        
        day_gap = (now_milli_time - artcle_milli_time) / (1000 * 60 * 60 * 24)
        
        print(day_gap)
        
        if  day_gap < 1:
            return True
        
        return False
            

    def showMsg(self, msg):
    	import Tkinter
    	root = Tkinter.Tk()
    	root.title('福利到！')
    	label = Tkinter.Label(root, text=msg)
        label.place(relx=0.5, rely=0.5, anchor='center')
    	center_window(root, 500, 240)
    	root.maxsize(800, 400)
    	root.minsize(500, 240)

        root.call('wm', 'attributes', '.', '-topmost', '1')
    	Tkinter.mainloop()




def get_screen_size(window):
    return window.winfo_screenwidth(),window.winfo_screenheight()

def get_window_size(window):
    return window.winfo_reqwidth(),window.winfo_reqheight()

def center_window(root, width, height):
    screenwidth = root.winfo_screenwidth()
    screenheight = root.winfo_screenheight()
    size = '%dx%d+%d+%d' % (width, height, (screenwidth - width)/2, (screenheight - height)/2)
    print(size)
    root.geometry(size)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
