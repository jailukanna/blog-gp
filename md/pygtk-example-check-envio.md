---
title: pygtk example check-envio (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'check-envio'

Functions in program: 
* `def check_envio(num_envio):`

Modules used in program: 
* `import json`
* `import urllib2`

## python check-envio

Python pygtk example: check-envio

```python
#!/usr/bin/env python

import urllib2
import json

def check_envio(num_envio):
    url = 'http://www.enviosoca.com/Tracking/GetLastTrackingData/default.aspx?trackingNumber=%s' % (num_envio,)
    resp = urllib2.urlopen(url)
    json_data = resp.read()
    return json.loads(json_data)

if __name__ == '__main__':
    import pygtk
    pygtk.require('2.0')
    import pynotify
    import sys

    if not pynotify.init("Urgency"):
        sys.exit(1)

    if len(sys.argv) < 2:
        print('Por favor use: %s <nro_envio>' % sys.argv[0])
        sys.exit(0)
        
    num_envio = sys.argv[1]

    data = check_envio(num_envio)
    '''
    {u'DescripcionEstado': u'En Tr\xe1nsito - Suc: CORDOBA II',
     u'DescripcionMotivo': u'Sin Motivo',
     u'Fecha': u'26/06/2013',
     u'NumeroEnvio': u'3531900000000000436',
     u'Sucursal': u'Suc: CORDOBA II'}
     '''
    estado = data['DescripcionEstado'].strip()
    fecha = data['Fecha']

    # Normal Urgency
    n = pynotify.Notification("Toshiba Estado",
                              "%s - %s" % (fecha, estado))
    n.set_urgency(pynotify.URGENCY_NORMAL)

    if not n.show():
        print("Failed to send notification")
        sys.exit(1)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
