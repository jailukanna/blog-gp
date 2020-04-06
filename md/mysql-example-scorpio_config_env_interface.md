---
title: mysql example scorpio config env interface (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'scorpio config env interface'


## python scorpio config env interface

Python mysql example: scorpio config env interface

```python
# =============================业务模块=============================
PAS = "pas"
CONSOLE = "console"
RDM_REST = "rdm-rest"
CAS = "cas"

ENV_55 = {
    CAS: "https://192.168.130.55:8443/cas/login?service=https%3A%2F%2F192.168.130.55%3A8443%2Fcas%2Flogin&"
         "token=eyJjdHkiOiJKV1QiLCJlbmMiOiJBMTkyQ0JDLUhTMzg0IiwiYWxnIjoiZGlyIn0..9KTQ8TOfjtvmVpvCkhFJ0g.1m"
         "t4nHm7y-BTKqYuCKoamjcpsRloPTFHgi-8hehOrgzqVJeg66vjr4hZiNCir4zT6hGab4s_lDZvuv7K-W2ssFBPa7xFCXMzQF"
         "Zxe7LFXI78t5-6opSQTfQKNuwfHzSpzfBh6_MnD-j-Fn6Agw3cuKvxhAurAXxN-RM4PRz6n6BDNJs8-YsnU57Z2gBv9_4XaA"
         "CetJAzMd6mdNJ27W6wQ2yQIH5D5WODj4wrMFE5YqLlLYM__j2vGptNHXn0ylk0BQm0lFcaxUzdmiib12nC2w.YEutvoPTguT"
         "khotTrY4DszmH64kH-vAw",
    PAS: "http://192.168.130.55:28085",
    CONSOLE: "http://192.168.130.55:28090/object-rest",
    RDM_REST: "http://192.168.130.56:28094/rdm-rest",
}

ENV_83 = {
    CAS: "https://192.168.130.83:8443/cas/login?service=https%3A%2F%2F192.168.130.83%3A8443%2Fcas%2Flogin&"
         "token=eyJjdHkiOiJKV1QiLCJlbmMiOiJBMTkyQ0JDLUhTMzg0IiwiYWxnIjoiZGlyIn0..9KTQ8TOfjtvmVpvCkhFJ0g.1m"
         "t4nHm7y-BTKqYuCKoamjcpsRloPTFHgi-8hehOrgzqVJeg66vjr4hZiNCir4zT6hGab4s_lDZvuv7K-W2ssFBPa7xFCXMzQF"
         "Zxe7LFXI78t5-6opSQTfQKNuwfHzSpzfBh6_MnD-j-Fn6Agw3cuKvxhAurAXxN-RM4PRz6n6BDNJs8-YsnU57Z2gBv9_4XaA"
         "CetJAzMd6mdNJ27W6wQ2yQIH5D5WODj4wrMFE5YqLlLYM__j2vGptNHXn0ylk0BQm0lFcaxUzdmiib12nC2w.YEutvoPTguT"
         "khotTrY4DszmH64kH-vAw",
    PAS: "http://192.168.130.83:28085",
    CONSOLE: "",
    RDM_REST: "http://192.168.130.83:28094/rdm-rest",
}
# =============================当前全局环境=============================
CUR_ENV = ENV_55
# ======================================================================

# =============================  其他配置  =============================
IGNORE_WARN = "IGNORE_WARN"

env_variable = {
    CAS: True
}

http_variable = {
    IGNORE_WARN: True
}

pas_variable = {
    "username": "k2data",
    "jobid": 21119,
    "dataSourceId": "ff44b991-4ade-41a7-81c6-1bd7ea1923c2",
    "operationID": "682834df-a582-47a2-b678-91eed0eb50f0",
    "scriptSourceId": "ff44b991-4ade-41a7-81c6-1bd7ea1923c2",
    "projectID": "100350"
}


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
