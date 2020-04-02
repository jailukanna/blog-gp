---
title: pil example Enhance! (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Enhance!'

Functions in program: 
* `def upscaler():`
* `def formpage():`

Modules used in program: 
* `import random`
* `import PIL.Image`
* `import byond.DMI`
* `import os`
* `import os.path`

## python Enhance!

Python pil example: Enhance!

```python
#!/usr/bin/env python2
from __future__ import print_function, division
from flask import Blueprint, request, render_template, abort, send_file
import os.path
import os
import byond.DMI
import PIL.Image
import random


dmi_uploader = Blueprint("dmi_uploader", __name__)

@dmi_uploader.route("/dmi")
def formpage():
    return render_template("dmi.html")

@dmi_uploader.route("/dmi/upload", methods=["POST"])
def upscaler():
    dmifile = request.files.get("fileinput", None)
    if dmifile == None:
        abort(400)
    
    dmi = None
    try:
        dmi = byond.DMI.DMI(dmifile)
        dmi.loadAll()

    except Exception as e:
        print("%s\n%s" % (e, e.args))
        abort(400)

    random.seed()
    filename = os.path.join("tempdmifiles", "%s.dmi" % (str(random.randint(1000000, 9999999))))
    upscaled = byond.DMI.DMI(filename)
    # Chop up the state list.
    states = request.form.get("stateinput", "")
    if not states.strip():
        # Upscale EVERYTHING.
        upscaled.icon_height = dmi.icon_height * 2
        upscaled.icon_width = dmi.icon_width * 2

        for state in dmi.states.values():
            new_state = byond.DMI.State(state.name)
            new_state.frames = state.frames
            new_state.dirs = state.dirs
            new_state.movement = state.movement
            new_state.loop = state.loop
            new_state.rewind = state.rewind
            new_state.delay = state.delay

            for icon in state.icons:
                new_icon = icon.resize((upscaled.icon_width, upscaled.icon_height), PIL.Image.NEAREST)
                new_state.icons.append(new_icon)
            
            upscaled.states[new_state.name] = new_state

    else:
        states = [x.strip() for x in states.split("\n")]
        print(states)
        for state in states:
            # Upscale the bottom left corner of the state to fill the full icon.
            dmistate = dmi.states.get(state)
            if not dmistate:
                print("nope")
                continue

            for index, icon in enumerate(dmistate.icons):
                width, height = icon.size
                cropped = icon.crop((0, height // 2, width // 2, height))
                dmistate.icons[index] = cropped.resize((width, height), PIL.Image.NEAREST)

        upscaled = dmi # Honk.

    # Hooray I can't tell BYONDTools to write to a temp file so we're doing temp files ourselves!
    upscaled.save(filename)
    honk = send_file(filename, as_attachment=True, attachment_filename=dmifile.filename)
    os.remove(filename)
    return honk

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
