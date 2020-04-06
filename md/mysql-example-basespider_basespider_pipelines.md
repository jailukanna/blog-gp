---
title: mysql example basespider basespider pipelines (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'basespider basespider pipelines'


Modules used in program: 
* `import re`
* `import logging`
* `import MySQLdb`

## python basespider basespider pipelines

Python mysql example: basespider basespider pipelines

```python
# -*- coding: utf-8 -*-

# Define your item pipelines here
#
# Don't forget to add your pipeline to the ITEM_PIPELINES setting
# See: http://doc.scrapy.org/en/latest/topics/item-pipeline.html

import MySQLdb
import logging
from basespider.utils.date_parse import parse_date
from basespider.utils.loadconfig import loadscrapyconf
import re


class BasespiderPipeline(object):
    conf = loadscrapyconf()['mysql']

    def open_spider(self, spider):
        self.cout = 1
        self.conn = MySQLdb.connect(host=self.conf.get("host", "localhost"), port=self.conf.get("port", 3306),
                                    user=self.conf.get("user", "root"), passwd=self.conf.get("passwd", "root"),
                                    db=self.conf.get("databases"), charset=u"utf8")
        self.cur = self.conn.cursor()
        logging.info(u"mysql连接成功!")
        if spider.proxy:
            logging.info(u"代理已启动...")
        else:
            logging.info(u"无代理状态抓取!")

    def process_item(self, item, spider):
        for k in item:
            item[k] = u"".join(item[k])
            item[k] = re.sub(r"\xa0", "", item[k])
            item[k] = re.sub(r"\u200b", "", item[k])
            item[k] = re.sub(r"\xa5", "", item[k])
        try:
            # 判断字段是否存在
            if 'name' in item:
                item['name'] = "".join(re.split("\s", item['name'])[-2:])
            elif 'pubtime' in item:
                item['pubtime'] = parse_date(item['pubtime'])
        except:
            logging.error(u"时间格式化错误!")
            return item
        print(u"{:=^30}".format(self.cout))

        # 收集item字段名及值
        fields = []
        values = []

        # 显示采集字段及内容
        for k, v in item.iteritems():
            print(u"{:>13.13}:{}".format(k, v))
            fields.append(k)
            values.append(v)

        if spider.debug:
            pass
        else:
            try:
                # 根据 item 字段插入数据
                self.cur.execute(
                    u"INSERT INTO {}({}) VALUES({});".format(spider.tablename,
                                                             u",".join(fields), u','.join([u'%s'] * len(fields))),
                    values)
                self.conn.commit()
                logging.info(u"数据插入成功!")
            except MySQLdb.Error, e:
                logging.error(u"Mysql Error %d: %s" % (e.args[0], e.args[1]))

        self.cout += 1
        return item

    def close_spider(self, spider):
        self.cur.close()
        self.conn.close()
        logging.info(u"mysql关闭成功")



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
