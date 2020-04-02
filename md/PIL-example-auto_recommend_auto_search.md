---
title: PIL example auto recommend auto search (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'auto recommend auto search'

Functions in program: 
* `def main():`

Modules used in program: 
* `import img_compress_helper`
* `import jieba`
* `import time`
* `import sys`
* `import config`
* `import re`
* `import thread`
* `import url_helper`
* `import selenium_helper`
* `import screen_shot_helper`
* `import recommend_helper`
* `import question_helper`
* `import ocr_helper`
* `import jieba_helper`

## python auto recommend auto search

Python PIL example: auto recommend auto search

```python
#encoding=utf-8
import jieba_helper
import ocr_helper
import question_helper
import recommend_helper
import screen_shot_helper
import selenium_helper
import url_helper
import thread
import re
import config
import sys
import time
import jieba
import img_compress_helper
reload(sys) 
sys.setdefaultencoding('utf8')

def main():
    jieba.initialize()
    driver = selenium_helper.browser_init()
    time.sleep(2)
    while(True): 
        time.sleep(1)
        beg = time.time()
        img_name = str(time.time())+".png"
        screen_shot_helper.window_capture(img_name,config.screen_shot_position,config.screen_shot_size)
        if screen_shot_helper.isQuestion(img_name) == False:
            continue
        #再截一张，确保题目完整
        time.sleep(0.2)
        screen_shot_helper.window_capture(img_name,config.screen_shot_position,config.screen_shot_size)
        screen_shot_tag = time.time()
        print('截图:',)
        print(screen_shot_tag - beg)
        path = config.path
        new_img = 'compress_' + img_name
        img_compress_helper.img_zip(path, img_name, new_img)
        question = ocr_helper.get_question(new_img)
        print('ocr耗时:',)
        ocr_tag = time.time()
        print(ocr_tag - screen_shot_tag)
#         print('原题:',)
#         print(question)
        question_body = question_helper.parse_question_and_answer(question) #题干和选项分开
        print('分词耗时:',)
        parse_tag = time.time()
        print(parse_tag - ocr_tag)
        print('选项:',)
        for i in question_body[2]:
            print(i,)
        print('')
#        不加选项搜
#         question = question_body[1]
        question = question_body[1]+" "+" ".join(question_body[2])
#         选项加引号
#         question = question_body[1]+" \""+'\" \"'.join(question_body[2]) +"\""
        print('搜索:',)
        print(question)
        thread.start_new_thread(selenium_helper.baidu_search, (driver,question))
        print('总耗时:',)
        end = time.time()
        print(end - beg),
        print('秒')
        try:
            print('**推荐**')
            thread.start_new_thread(recommend_helper.recommend_fast,(question_body,0,1))
            thread.start_new_thread(recommend_helper.recommend_fast,(question_body,0,0))
            thread.start_new_thread(recommend_helper.recommend_fast,(question_body,1,1))
            thread.start_new_thread(recommend_helper.baidu_count2,(question_body,))
        except:
            print('except')
#             print(traceback.print_exc())
#             time.sleep(7)
            continue
        time.sleep(10)

if __name__ =='__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
