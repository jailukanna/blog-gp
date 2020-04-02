---
title: PIL example pricecrawler (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'pricecrawler'


Modules used in program: 
* `import pymysql.cursors`
* `import scrapy`

## python pricecrawler

Python PIL example: pricecrawler

```python
# -*- coding: utf-8 -*-
import scrapy
import pymysql.cursors
from scrapy.conf import settings
from scrapy.spiders import CrawlSpider
from scrapy.selector import Selector
from sat8.items import ProductPriceItem, ProductItemLoader
from time import gmtime, strftime

class TgdtSpider(CrawlSpider):
	name = "TgdtSpider"
	allowed_domains = []
	start_urls = []

	def __init__(self):
		domains = settings['PRODUCT_PRICE_RULES']
		for d in domains:
			self.allowed_domains.append(d['Source'])
			self.start_urls.append(d['Url'])

	def parse(self, response):
		domains = settings['PRODUCT_PRICE_RULES']

		# sel = Selector(response)
		# product_links = sel.xpath('//*[@id="lstprods"]/li/a/@href')
		for d in domains:
			url = response.urljoin(d['Url']);
			request = scrapy.Request(url, callback = self.parse_detail_content)
			request.meta['item'] = d
			yield request

	def parse_detail_content(self, response):
		domain = response.meta['item']
		link = response.url
		pil = ProductItemLoader(item = ProductPriceItem(), response = response)
		pil.add_xpath('price', domain['Price'])
		pil.add_xpath('name', domain['Title'])
		pil.add_value('source', domain['Source'])
		pil.add_value('link', link)
		pil.add_value('created_at', strftime("%Y-%m-%d %H:%M:%S", gmtime()))
		pil.add_value('updated_at', strftime("%Y-%m-%d %H:%M:%S", gmtime()))
		item = pil.load_item()
		print(item)
		# try:
		# 	item['price']
		# except Exception, e:
		# 	print("Price is null")
		# else:
		# 	yield(item)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
