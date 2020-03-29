---
title: pygtk example hackthissite programming mission 2 (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'hackthissite programming mission 2'


## python hackthissite programming mission 2

Python pygtk example: hackthissite programming mission 2

```python
from PIL import Image
#morse alphabet for conversation
letters = [('A',".-"),   ('B',"-..."), ('C',"-.-."), ('D',"-.."), ('E',"."),
           ('F',"..-."), ('G',"--."),  ('H',"...."), ('I',".."),  ('J',".---"),
           ('K',"-.-"),  ('L',".-.."), ('M',"--"),   ('N',"-."),  ('O',"---"),
           ('P',".--."), ('Q',"--.-"), ('R',".-."),  ('S',"..."), ('T',"-"),
           ('U',"..-"),  ('V',"...-"), ('W',".--"),  ('X',"-..-"),('Y',"-.--"),
           ('Z',"--.."), ('1',".----"), ('2',"..---"), ('3',"...--"), ('4',"....-"),
           ('5',"....."), ('6',"-...."), ('7',"--..."), ('8',"---.."), ('9',"----."),
           ('0',"-----")]

if __name__=='__main__':
    #image has to be in a.png file
    #read image
    f = open('a.png')
    im = Image.open(f)
    pixels = im.load()
    width,height = im.size
    #put pixels in an arrray
    all_pixel = []
    for y in range(height):
        for x in range(width):
            all_pixel.append(pixels[x,y])
    #convert pixels to characters
    last_index = 0
    result_morse = []
    for i,p in enumerate(all_pixel):
        if p == 1:
            result_morse.append(i-last_index)
            last_index = i
    #create result string from result list
    st = ''
    for i in result_morse:
        st = st+chr(i)
    #sepearte morse characters and create a mores character list
    morse_list = st.split(' ')
    #decode morse code
    result = ""
    for morse_letter in morse_list:
        for letter in letters:
            if morse_letter == letter[1]:
                result = result + letter[0]
                break
        else:
            #if we cant convert morse code, print(the code)
            if morse_letter:
                print("problem letter = %s"%morse_letter)
    print(result)
    #copy result to clipboard
    import pygtk
    pygtk.require('2.0')
    import gtk
    # get the clipboard
    clipboard = gtk.clipboard_get()
    # set the clipboard text data
    clipboard.set_text(result)
    # make our data available to other applications
    clipboard.store()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
