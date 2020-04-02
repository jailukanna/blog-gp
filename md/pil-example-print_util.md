---
title: pil example print util (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'print util'

Functions in program: 
* `def print_pretty(data) -> None:`
* `def format_json(data) -> str:`
* `def print_ascii_art(text: str, text_format: TextFormat = None) -> None:`
* `def format_error_text(text: str, text_format: TextFormat = None) -> str:`
* `def format_warning_text(text: str, text_format: TextFormat = None) -> str:`
* `def format_success_text(text: str, text_format: TextFormat = None) -> str:`
* `def format_text(text: str, _format: TextFormat) -> str:`
* `def print_progress(message: str) -> None:`

Modules used in program: 
* `import re`
* `import sys`
* `import json`

## python print util

Python pil example: print util

```python
import json
import sys

import re
from enum import Enum


def print_progress(message: str) -> None:
    sys.stdout.write(message + '\r')
    sys.stdout.flush()


class TextFont(Enum):
    FREE_MONO_BOLDOBLIQUE = '/usr/share/fonts/truetype/freefont/FreeMonoBoldOblique.ttf'
    FREE_MONO_BOLD = '/usr/share/fonts/truetype/freefont/FreeMonoBold.ttf'
    FREE_MONO_OBLIQUE = '/usr/share/fonts/truetype/freefont/FreeMonoOblique.ttf'
    FREE_MONO = '/usr/share/fonts/truetype/freefont/FreeMono.ttf'
    FREE_SANS_BOLD_OBLIQUE = '/usr/share/fonts/truetype/freefont/FreeSansBoldOblique.ttf'
    FREE_SANS_BOLD = '/usr/share/fonts/truetype/freefont/FreeSansBold.ttf'
    FREE_SANS_OBLIQUE = '/usr/share/fonts/truetype/freefont/FreeSansOblique.ttf'
    FREE_SANS = '/usr/share/fonts/truetype/freefont/FreeSans.ttf'
    FREE_SERIF_BOLD_ITALIC = '/usr/share/fonts/truetype/freefont/FreeSerifBoldItalic.ttf'
    FREE_SERIF_BOLD = '/usr/share/fonts/truetype/freefont/FreeSerifBold.ttf'
    FREE_SERIF_ITALIC = '/usr/share/fonts/truetype/freefont/FreeSerifItalic.ttf'
    FREE_SERIF = '/usr/share/fonts/truetype/freefont/FreeSerif.ttf'


class TextColor(Enum):
    HEADER = '\033[95m'
    OKBLUE = '\033[94m'
    OKGREEN = '\033[92m'
    WARNING = '\033[93m'
    FAIL = '\033[91m'
    BLACK = '\033[30m'
    RED = '\033[31m'
    GREEN = '\033[32m'
    YELLOW = '\033[33m'
    BLUE = '\033[34m'
    PURPLE = '\033[35m'
    CYAN = '\033[36m'
    WHITE = '\033[37m'
    NO_EFFECT = '\033[0m'


class TextBackground(Enum):
    BLACK = '\033[40m'
    RED = '\033[41m'
    GREEN = '\033[42m'
    YELLOW = '\033[43m'
    BLUE = '\033[44m'
    PURPLE = '\033[45m'
    CYAN = '\033[46m'
    WHITE = '\033[47m'
    NO_COLOR = ''


class TextStyle(Enum):
    BOLD = '\033[1m'
    UNDERLINE = '\033[4m'


class TextFormat(object):
    """
    :type background: TextBackground
    :type text: TextColor
    :type styles: typing.List[TextStyle]
    :type quote: bool
    :type font: TextFont
    """

    def __init__(self, **kwargs):
        self.bg = kwargs.get('background', TextBackground.NO_COLOR)
        self.color = kwargs.get('text', TextColor.NO_EFFECT)
        self.styles = kwargs.get('styles', [])
        self.quote = False
        self.font = kwargs.get('font')


def format_text(text: str, _format: TextFormat) -> str:
    _style = ''
    for style in _format.styles:
        _style += style.value

    if _format.quote:
        text = re.sub('<', _format.color + _style + _format.bg, text)
        text = re.sub('>', TextColor.NO_EFFECT.value, text)
        return text

    return _format.color.value + _style + _format.bg.value + text + TextColor.NO_EFFECT.value


def format_success_text(text: str, text_format: TextFormat = None) -> str:
    if not text_format:
        text_format = TextFormat()

    text_format.color = TextColor.OKGREEN
    return format_text(text, text_format)


def format_warning_text(text: str, text_format: TextFormat = None) -> str:
    if not text_format:
        text_format = TextFormat()

    text_format.color = TextColor.WARNING
    return format_text(text, text_format)


def format_error_text(text: str, text_format: TextFormat = None) -> str:
    if not text_format:
        text_format = TextFormat()

    text_format.color = TextColor.FAIL
    return format_text(text, text_format)


def print_ascii_art(text: str, text_format: TextFormat = None) -> None:
    import PIL.ImageDraw

    if not text_format or text_format.font:
        raise Exception('Err: No Font Selected')

    font = PIL.ImageFont.truetype(text_format.font, 10)  # load the font
    size = font.getsize(text)  # calc the size of text in pixels
    image = PIL.Image.new('1', size, 1)  # create a b/w image
    draw = PIL.ImageDraw.Draw(image)
    draw.text((0, 0), text, font=font)  # render the text to the bitmap
    for row_num in range(size[1]):
        # scan the bitmap:
        # print(' ' for black pixel and)
        # print('#' for white one)
        line = []
        for column in range(size[0]):
            if image.getpixel((column, row_num)):
                line.append(' '),
            else:
                line.append('0'),

        print(format_text(''.join(line), text_format))

    return


def format_json(data) -> str:
    return json.dumps(data, indent=4)


def print_pretty(data) -> None:
    print(format_json(data))
    return


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
