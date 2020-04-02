---
title: PIL example overlayimage (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'overlayimage'

Functions in program: 
* `def overlayimage(overlaypicture,`

## python overlayimage

Python PIL example: overlayimage

```python

def overlayimage(overlaypicture,
                 baseimg,
                 outfile='same',
                 # position in relative coordinates for icon/other image
                 position=[0.1, 0.1],
                 # scaling off image
                 width=0.2,
                 # scaling off image
                 height=0.2,
                 # if the height/width do not fit the aspect
                 # of the image, adjust it automatically
                 keepaspect=True,
                 # the adjustment is by default for width, change to True
                 # to scale width instead
                 adjustheight=False,
                 # use the top corner (usual for images)
                 # set to False, as mainly we think from lower left as 0,0
                 # in science
                 fromtop=False,
                 # if outfile is not changed, input is required
                 # set force to change the original without checking
                 force=False,
                 ):

    import PIL

    if outfile == 'same':
        if not force:
            print('Are you sure you wan\'t to overlay it on the same picture?',
                  'Set force to suppress this warning')
            from time import sleep

            try:
                print("Press Ctrl+C within 10 seconds and then Y/N")
                for i in range(0, 10):  # waits 10 seconds
                    sleep(1)
            except KeyboardInterrupt:
                ans = input()
                if (ans.lower()[0] in ['y', 'j'] and 'n' not in ans.lower()):
                    force = True
                else:
                    print('There was an unrecognized character!',
                          'Are you sure you know how this works?',
                          'Let\'s try again...')
                    return overlayimage(overlaypicture,
                                        baseimg,
                                        position=position,
                                        width=width,
                                        height=height,
                                        keepaspect=keepaspect,
                                        adjustheight=adjustheight,
                                        fromtop=fromtop,
                                        force=force)

        if force:
            outfile = baseimg
        else:
            outfile = outfile.split('.')
            outfile[0] += '_overlay'
            outfile = '.'.join(outfile)

    if width > 1:
        width /= 100

    if height > 1:
        height /= 100

    position = [p / 100 if p > 1 else p for p in position]
    background = PIL.Image.open(baseimg)
    foreground = PIL.Image.open(overlaypicture)
    if keepaspect:
        if adjustheight:
            height *= foreground.height / foreground.width
        else:
            width *= foreground.width / foreground.height
        foreground = foreground.resize((int(width*background.width),
                                        int(height*background.height)),
                                       PIL.Image.LANCZOS)
    else:
        foreground = foreground.resize((int(width*background.width),
                                        int(height*background.height)),
                                       PIL.Image.LANCZOS)

    # 4-tuple defining the left, upper, right, and lower pixel corners
    # since we usually define this from lower left but images are from upper
    # left the proper position is 1-height-ypos
    pos = [int(position[0]*background.width),
           int((1-position[1]-height)*background.height)]

    background.paste(foreground, pos, foreground)
    background.save(outfile)
    return outfile


if __name__ == '__main__':
    print('*'*10, 'HELP for overlayimage', '*'*10)
    print('Put another picture into one that already exists')
    print('Outputfile is by default the same and position is 20%',
          'and starts from lower left corner but can be adjusted as needed')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
