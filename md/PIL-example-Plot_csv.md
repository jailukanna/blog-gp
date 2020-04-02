---
title: PIL example Plot csv (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Plot csv'

Functions in program: 
* `def runScript():`
* `def plotCsvs(conn, scriptParams):`

Modules used in program: 
* `import numpy`
* `import omero.scripts as scripts`

## python Plot csv

Python PIL example: Plot csv

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import omero.scripts as scripts
from omero.gateway import BlitzGateway
from omero.rtypes import rstring, rlong
import numpy

try:
    from PIL import Image  # see ticket:2597
except ImportError:
    import Image


def plotCsvs(conn, scriptParams):

    images = []
    if scriptParams['Data_Type'] == "Image":
        images = conn.getObjects("Image", scriptParams['IDs'])
    elif scriptParams['Data_Type'] == "Dataset":
        for d in conn.getObjects("Dataset", scriptParams['IDs']):
            images.extend(list(d.listChildren()))

    filterBy = scriptParams['CSV_Name']
    for image in images:
        print("Processing image: ", image.getId(), image.getName())
        dataset = image.getParent()

        for ann in image.listAnnotations():
            if ann.__class__.__name__ == "FileAnnotationWrapper":
                fname = ann.getFileName()
                if filterBy in fname and fname.endswith(".csv"):
                    print(" Using csv", ann.id, fname)

                    myCsv = open(str(fname), 'w')
                    print("\nDownloading file to", myCsv, "...")
                    try:
                        for chunk in ann.getFileInChunks():
                            myCsv.write(chunk)
                    finally:
                        myCsv.close()
                        print("File downloaded!")

                    # read file, plot etc.

                    f = open(fname)
                    x = []
                    y = []
                    # lines = []
                    lines = f.read()
                    rows = lines.split('\r')
                    for r in rows:
                        try:
                            xx, yy = r.split(",")
                            x.append(int(xx))
                            y.append(float(yy))
                        except:
                            pass

                    print(x)
                    print(y)

                    # plt.scatter(x, y, s=area, c=colors, alpha=0.5)
                    # create plot.png
                    # then open and upload as new image in OMERO

                    pilImage = Image.open("plot.png")
                    plotW = pilImage.size[0]
                    plotH = pilImage.size[1]
                    pix = numpy.array(pilImage.getdata(), dtype=numpy.int32)
                    red = pix[:, 0]
                    red = red.reshape(plotH, plotW)
                    green = pix[:, 1]
                    green = green.reshape(plotH, plotW)
                    blue = pix[:, 2]
                    blue = blue.reshape(plotH, plotW)

                    zctPlanes = iter([red, green, blue])
                    i = conn.createImageFromNumpySeq(zctPlanes, "plot", sizeC=3, dataset=dataset)
                    print(i)


def runScript():
    """
    The main entry point of the script, as called by the client via the
    scripting service, passing the required parameters.
    """

    dataTypes = [rstring('Dataset'), rstring('Image')]

    client = scripts.client(
        'Plot_csv.py',
        """Downloads a csv file and plots it""",

        scripts.String(
            "Data_Type", optional=False, grouping="1",
            description="The data you want to work with.", values=dataTypes,
            default="Image"),

        scripts.List(
            "IDs", optional=False, grouping="2",
            description="List of Dataset IDs or Image IDs").ofType(rlong(0)),

        scripts.String(
            "CSV_Name", optional=False, grouping="3",
            description="Filter csv files that contain this",
            default=".csv"),
    )

    try:
        scriptParams = {}

        conn = BlitzGateway(client_obj=client)

        scriptParams = client.getInputs(unwrap=True)
        print(scriptParams)

        plotCsvs(conn, scriptParams)

        # call the main script - returns a file annotation wrapper
        client.setOutput("Message", rstring("Done!"))

    finally:
        client.closeSession()

if __name__ == "__main__":
    runScript()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
