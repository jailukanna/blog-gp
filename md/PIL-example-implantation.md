---
title: PIL example implantation (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'implantation'

Functions in program: 
* `def parse_locations(impl, fname):`
* `def parse_ei_txt(filename):`
* `def parse_label(label):`

Modules used in program: 
* `import numpy as np`
* `import pprint`
* `import re`

## python implantation

Python PIL example: implantation

```python
import re
import pprint

import numpy as np

def parse_label(label):
    reg, num = re.match("([A-Za-z']+)(\d+)", label).groups()
    num = int(num)
    return reg, num

class Contact(object):
    def __init__(self, label, **info):
        self.label = label
        self.bipolar = '-' in label
        if self.bipolar:
            l1, l2 = label.split('-')
            self.region, self.index = parse_label(l1)
            _, self.index_ref = parse_label(l2)
        else:
            self.region, self.index = parse_label(label)
        for k, v in info.iteritems():
            setattr(self, k, v)
    def __repr__(self):
        fmt = "<Contact %s%s>"
        idx = "{0.index}-{0.index_ref}" if self.bipolar else "{0.index}"
        fmt %= self.region, idx.format(self)
        return fmt

class Electrode(object):
    def __init__(self, contacts, oblique=False, **info):
        self.region = contacts[0].region
        self.contacts = contacts
        for k, v in info.iteritems():
            setattr(self, k, v)
        self.oblique = oblique
        self.ncont = max(
                c.index_ref if c.bipolar else c.index 
                for c in contacts)
    def __repr__(self):
        fmt = "<Electrode %05s %02s contacts>"
        fmt %= self.region, self.ncont
        return fmt
    def __getitem__(self, key):
        if hasattr(self, key):
            return getattr(self, key)
        elif all(hasattr(c, key) for c in self.contacts):
            return np.array([getattr(c, key) for c in self.contacts])
        
class Implantation(dict):
    def __init__(self, electrodes):
        self.electrodes = electrodes
        for elec in self.electrodes:
            self[elec.region] = elec
    def __repr__(self):
        return pprint.pformat(self.electrodes)

def parse_ei_txt(filename):
    with open(filename) as fd:
        ei_lines = fd.readlines()
    electrodes = []
    contacts = []
    for line in ei_lines:
        if line.startswith('-'):
            electrodes.append(Electrode(contacts))
            contacts = []
            continue
        label, ei = line.split('\t')
        ei = float(ei.strip())
        contacts.append(Contact(label, ei=ei))
    if contacts:
        electrodes.append(Electrode(contacts))
    return Implantation(electrodes)

def parse_locations(impl, fname):
    print(impl)
    with open(fname) as fd:
        for l in fd.readlines():
            l = l.strip()
            if l and not l.startswith('#'):
                parts = [s for s in l.split() if s.strip()]
                r = parts[0].strip()
                n = int(parts[1])
                tx, ty, tz, ix, iy, iz = map(float, parts[2:])
                if r not in impl:
                    if r[-1] == 'p':
                        r = r[:-1] + "'"
                elec = impl[r]
                elec.target = np.array([tx, ty, tz])
                elec.entry = np.array([ix, iy, iz])
                elec.oblique = abs(tz - iz) > abs(tx - ix)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
