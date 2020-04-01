---
title: socket example python-soap (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'python-soap'


Modules used in program: 
* `import socket`
* `import os`

## python python-soap

Python socket example: python-soap

```python
# *-* coding:utf-8 *-*
import os
import socket

target_host = "www.webservicex.com"
target_port = 80
target_path = "/globalweather.asmx"
target_namespace = "http://www.webserviceX.NET"
service = "GetCitiesByCountry"

xmldata = "";
xmldata += '<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:web="http://www.webserviceX.NET">'
xmldata += '<soapenv:Header/>'
xmldata += '<soapenv:Body>'
xmldata += '<web:GetCitiesByCountry>'
xmldata += '<web:CountryName>Argentina</web:CountryName>'
xmldata += '</web:GetCitiesByCountry>'
xmldata += '</soapenv:Body>'
xmldata += '</soapenv:Envelope>'

tamanho = len(xmldata )

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect( (target_host, target_port) )
chamada = '''
POST http://{host}/{path} HTTP/1.1
Content-Type: text/xml;charset=UTF-8
SOAPAction: "{namespace}/{service}"
Content-Length: {tamanho}
Host: {host}
Connection: Keep-Alive

{xml}
          '''.format(host = target_host, path = target_path, namespace = target_namespace, service = service, xml = xmldata, tamanho = tamanho)
if bytes != str: # Testing if is python2 or python3
    chamada = bytes(chamada, 'utf-8')
client.send(chamada)
response = client.recv(4096)
response = response.decode(encoding="utf-8")
response = response.replace("&lt;","<")
response = response.replace("&gt;",">")
print( response )


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
