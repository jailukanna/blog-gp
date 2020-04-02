---
title: pil example find invisible (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'find invisible'

Functions in program: 
* `def assert_principles():`
* `def letter_draw_bytes(char):`
* `def unicode_characters():`
* `def invisible_characters():`
* `def main():`

Modules used in program: 
* `import unicodedata`

## python find invisible

Python pil example: find invisible

```python
import unicodedata

from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont


FONT = ImageFont.truetype('./unifont-7.0.06.ttf', 32)


def main():
    assert_principles()

    for char in invisible_characters():
        try:
            name = unicodedata.name(char)
        except ValueError:
            name = ''

        line = 'U+{:X}\t{}'.format(ord(char), name)
        print(line)


def invisible_characters():
    '''空白として描画された文字のみを返却する'''
    empty_draw_bytes = letter_draw_bytes(' ')
    for char in unicode_characters():
        draw_bytes = letter_draw_bytes(char)
        if draw_bytes == empty_draw_bytes:
            yield char


def unicode_characters():
    '''Unicode コードポイントに含まれる文字全てを返すジェネレータ
ただし、 PUA (Private Use Area) は含めない。
    '''

    for codepoint in range(0, 0xE000):
        yield chr(codepoint)

    for codepoint in range(0xF8FF + 1, 0xF0000):
        yield chr(codepoint)

    for codepoint in range(0xFFFFD + 1, 0x100000):
        yield chr(codepoint)

    for codepoint in range(0x10FFFD + 1, 0x10FFFF):
        yield chr(codepoint)


def letter_draw_bytes(char):
    '''空白の画像に文字を描画した際の bytes を取得する'''

    image = Image.new('RGB', (32, 32), (255, 255, 255))
    draw = ImageDraw.Draw(image)
    draw.text((0, 0), char, font=FONT, fill='#000')
    return image.tobytes()


def assert_principles():
    '''以下の原則が成立することをいくつかの例でチェック'''
    f = letter_draw_bytes

    assert f('あ') == f('あ')  # 同じ文字の bytes は同じ

    assert f('') == f(' ') == f('　')  # スペースは空文字列と同じ bytes になる

    assert f('あ') != f('い')  # 見た目が異なる文字の bytes は異なる


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
