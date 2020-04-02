---
title: PIL example text2image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'text2image'

Functions in program: 
* `def __startup():`

Modules used in program: 
* `import textwrap`
* `import os`
* `import argparse`

## python text2image

Python PIL example: text2image

```python
#!/usr/bin/python3
# -*- coding:utf-8 -*-
r"""テキストファイルを画像に変換

このファイル自体はサブコマンンド向けに作られた内部のクラスを実行する
ためのダミー


使い方
# 縦長(720x1280) 20桁 白色文字 黒色背景 ./font.ttf png形式
$ python text2image.py txt2img gingatetsudono_yoru.txt gingatetsudo

# 横長(1280x720) 25桁 二段組 白色文字 緑色背景 システムフォント jpg形式
$ python text2image.py txt2img text.txt text-nidan -s 1280x720 -c 25 -dan -fg white -bg green -font /system/fonts/MTLmr3m.ttf -fmt jpg


1)画像サイズや桁数も指定も可能。
2)フォントファイルはカレントパスにfont.ttfとして置くか、パラメーターで
  指定する。
3)禁則処理は実装していない。

詳細は
$ python text2image.py txt2img --help


色
  #ffffff形式や色名で指定可能
  white black red green blue brown gray dark***

フォントファイル
  TrueTypeFont(ttf), OpenTypeFont(otf)形式の固定幅 monospace
  カレントパスにfont.ttfとして置くか、フォントファイルへのパスを指定する。

  Android 5.x  /system/fonts/MTLmr3m.ttf


動作に必要なもの
  Python 3.6以降
  Pillow 5.3.0

開発環境
  Python 3.7.1 / Android 5.1


変更履歴
2018-11-17  新規作成
2018-11-18  出力形式を追加(png, jpg)
            二段組に対応
2018-11-19  出力先ディレクトリの生成
            縦書き対応(暫定)
            コマンドラインパラメータを整理
2018-11-30  縦書き対応(暫定その二)
            縦書き用フォントを生成するサブコマンドを追加
"""
#------------------------------------------------------------------------------
# インポート
#------------------------------------------------------------------------------

# 標準ライブラリ
import argparse
import os
import textwrap

# サードパーティー製ライブラリ
#from PIL import Image, ImageDraw, ImageFont

# 自作ライブラリ


#------------------------------------------------------------------------------
# 定数の定義　(スコープ：ファイル内)
#------------------------------------------------------------------------------


#------------------------------------------------------------------------------
# 定数の定義　(スコープ：ファイル外)
#------------------------------------------------------------------------------


#------------------------------------------------------------------------------
# 変数の定義　(スコープ：ファイル内)
#------------------------------------------------------------------------------


#------------------------------------------------------------------------------
# 変数の定義　(スコープ：ファイル外)
#------------------------------------------------------------------------------


#------------------------------------------------------------------------------
# クラスの定義　(スコープ：ファイル内)
#------------------------------------------------------------------------------


#------------------------------------------------------------------------------
# クラスの定義　(スコープ：ファイル外)
#------------------------------------------------------------------------------


class GenerateTateFont():
    """ aaa """
    #--------------------------------------------------------------------------
    # クラス定数の定義 (スコープ：クラス内)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # クラス定数の定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------

    argsparser = None

    #--------------------------------------------------------------------------
    # クラス変数の定義 (スコープ：クラス内)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # クラス変数の定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # スタティックメソッドの定義 (スコープ：クラス内)
    #--------------------------------------------------------------------------

    @staticmethod
    def _load_module(name, absname):
        import importlib

        if name not in globals():
            globals()[name] = importlib.import_module(absname)

    #--------------------------------------------------------------------------
    # スタティックメソッドの定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # クラスメソッドの定義 (スコープ：クラス内)
    #--------------------------------------------------------------------------

    @classmethod
    def _generate(cls, in_file):
        """縦書きフォントを生成する

        フォントファイル内の縦書き用の情報で横書き用の情報を書きかえる
        縦書き用の情報が無いフォントファイルもある


        GSUB — Glyph Substitution Table
        https://docs.microsoft.com/ja-jp/typography/opentype/spec/gsub
        """
        # 縦書きが指定されているlookupのインデックスを抽出する
        ttf = globals()['ttlib'].TTFont(in_file)
        lookup_idx_list = []
        for feature in ttf['GSUB'].table.FeatureList.FeatureRecord:
            if feature.FeatureTag in ['vert', 'vrt2', 'vrtr']:
                for idx in feature.Feature.LookupListIndex:
                    if idx not in lookup_idx_list:
                        lookup_idx_list.append(idx)

        # lookupから代替グリフを抽出する
        new_map = {}
        for idx in lookup_idx_list:
            lookup = ttf['GSUB'].table.LookupList.Lookup[idx]
            for substitution in lookup.SubTable:
                new_map.update(substitution.mapping)

        # 文字コードとグリフIDのマップに代替グリフIDを上書きする
        for cmap in ttf['cmap'].tables:
            for code, name in cmap.cmap.items():
                if name in new_map.keys():
                    cmap.cmap[code] = new_map[name]

#        ttf.save('tate.ttf')
        return ttf

    @classmethod
    def _main(cls, args):
        """メイン処理

        クラス属性
            font         ImageFontオブジェクト
            font_width   基準となる文字の幅
            font_height  基準となる文字の高さ
            page_rows    1ページの行数
            islibraqm    libraqmが使える環境かどうか
        """
        # サードパーティー製のモジュールを動的ロード
        #   本サブコマンドを使わない環境でも
        #   他サブコマンドを動作させるため
        try:
            cls._load_module('ttlib', 'fontTools.ttLib')

        except ModuleNotFoundError:
            print('実行に必要なモジュールが不足しています。')
            print('fontToolsをインストールして下さい。\n')
            return False

        ttf = cls._generate(args.in_file)

        fname, ext = os.path.splitext(os.path.basename(args.in_file))
        ttf.save('{}-tate{}'.format(fname, ext))

        return True

    #--------------------------------------------------------------------------
    # クラスメソッドの定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------

    @classmethod
    def set_argument(cls, parser_):
        """ sample """
        parser = parser_.add_parser(
            'tatefont',
            formatter_class=argparse.RawDescriptionHelpFormatter,
            description=textwrap.dedent('''\
縦書き用フォントを生成する
                '''),
            epilog=textwrap.dedent('''--------------------
pip fontTools
            '''),
            help='縦書き用フォントを生成する')

        parser.add_argument(
            'in_file',
            type=str,
            metavar='INPUT_FILE',
            help='フォントファイル ttf')

        cls.argsparser = parser
        parser.set_defaults(func=cls._main)

    #--------------------------------------------------------------------------
    # 特殊メソッドの定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # プロパティーの定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # インスタンスメソッドの定義 (スコープ：クラス内)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # インスタンスメソッドの定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------


class GenerateTextImage():
    """ aaa """
    #--------------------------------------------------------------------------
    # クラス定数の定義 (スコープ：クラス内)
    #--------------------------------------------------------------------------

    # 文字のサイズを算出する基準となる文字
    _BASE_CHAR = 'あ'

    # 描画時の行間の重みづけ
    _FONT_ROW_WEIGHT = 1.3

    # 段組の間隔(文字数)
    _DAN_SPACE = 1

    #--------------------------------------------------------------------------
    # クラス定数の定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------

    argsparser = None

    #--------------------------------------------------------------------------
    # クラス変数の定義 (スコープ：クラス内)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # クラス変数の定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # スタティックメソッドの定義 (スコープ：クラス内)
    #--------------------------------------------------------------------------

    @staticmethod
    def _load_module(name, absname):
        import importlib

        if name not in globals():
            globals()[name] = importlib.import_module(absname)

    @staticmethod
    def _save_image(canvas, out_file, fmt, page_no):
        if fmt == 'png':
            img = canvas.quantize(8).convert('P')

        else:
            img = canvas

        img.save('{}-{:0>3}.{}'.format(out_file, page_no, fmt))

    #--------------------------------------------------------------------------
    # スタティックメソッドの定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # クラスメソッドの定義 (スコープ：クラス内)
    #--------------------------------------------------------------------------

    @classmethod
    def _generate_font(cls, fontfile, columns, basesize, dan, tate):
        if tate:
            engine = globals()['ImageFont'].LAYOUT_RAQM

        else:
            engine = globals()['ImageFont'].LAYOUT_BASIC

        target_value = basesize[1] if tate else basesize[0]
        for i in range(4, 120, 1):
            tmp = globals()['ImageFont'].truetype(
                fontfile, i, layout_engine=engine)

            if tate:
                v = tmp.getsize(cls._BASE_CHAR)[1]

            else:
                v = tmp.getsize(cls._BASE_CHAR)[0]

            if dan:
                v = v * (columns * 2 + cls._DAN_SPACE)

            else:
                v = v * columns

            if v > target_value:
                break

            font = tmp

        else:
            return None

        return font

    @classmethod
    def _draw_char(cls, col, row, char, draw, fg_color, tate):
        """文字を描画する

        libraqmがインストールされていると自動で縦書きで描画してくれるらしい

        PIL.ImageDraw.Draw.text
        https://pillow.readthedocs.io/en/4.2.x/reference/ImageDraw.html#PIL.ImageDraw.PIL.ImageDraw.Draw.text
            direction, features, Requires libraqm. New in version 4.2.0.
        """
        # 描画位置を算出
        if tate:
            w_v = cls._FONT_ROW_WEIGHT
            h_v = 1
            tmp = col
            col = cls.page_rows - 1 - row
            row = tmp

        else:
            w_v = 1
            h_v = cls._FONT_ROW_WEIGHT

        pos = [
            cls.base_pos_x + (col * (cls.font_width * w_v)),
            cls.base_pos_y + (row * (cls.font_height * h_v))
        ]
        char_width = cls.font.getsize(char)[0]
        if char_width < cls.font_width:
            pos[0] = pos[0] + (cls.font_width - char_width) / 2

        # 文字を描画
        if tate and cls.islibraqm:
            try:
                draw.text(
                    pos, char, fg_color, cls.font,
                    direction='ttb', features=['vert', 'vrt2', 'vtrt'])

            except KeyError:
                print('WARN libraqmで'
                      'direction=\'ttb\', '
                      'features=[\'vert\', \'vrt2\', \'vtrt\']'
                      'が使えませんでした。\n')

                cls.islibraqm = False
                draw.text(pos, char, fg_color, cls.font)

        else:
            draw.text(pos, char, fg_color, cls.font)

    @classmethod
    def _generate(cls, args, text, canvas):
        draw = globals()['ImageDraw'].Draw(canvas)

        page_no = 0
        row = 0
        col = 0
        dan_col = 0
        for char in text:
            # 改行
            if char == '\n':
                row = row + 1
                col = 0

            if col >= args.c:
                row = row + 1
                col = 0

            # 改ページ
            if row >= cls.page_rows:
                row = 0
                col = 0

                if args.dan and dan_col == 0:
                    dan_col = args.c + cls._DAN_SPACE

                else:
                    page_no = page_no + 1
                    dan_col = 0

                    cls._save_image(canvas, args.out_file, args.fmt, page_no)

                    draw.rectangle(
                        (0, 0, canvas.size[0] - 1, canvas.size[1] - 1),
                        args.bg)

            if char == '\n':
                continue    # 改行文字は描画しない

            # 文字を描画
            cls._draw_char(col + dan_col, row, char, draw, args.fg, args.tate)

            col = col + 1

        # 未改ページ分を出力
        if col > 0 or row > 0:
            page_no = page_no + 1
            cls._save_image(canvas, args.out_file, args.fmt, page_no)

    @classmethod
    def _main(cls, args):
        """メイン処理

        クラス属性
            font         ImageFontオブジェクト
            font_width   基準となる文字の幅
            font_height  基準となる文字の高さ
            page_rows    1ページの行数
            islibraqm    libraqmが使える環境かどうか
        """
        # サードパーティー製のモジュールを動的ロード
        #   本サブコマンドを使わない環境でも
        #   他サブコマンドを動作させるため
        try:
            cls._load_module('Features', 'PIL.features')
            cls._load_module('Image', 'PIL.Image')
            cls._load_module('ImageDraw', 'PIL.ImageDraw')
            cls._load_module('ImageFont', 'PIL.ImageFont')

        except ModuleNotFoundError:
            print('実行に必要なモジュールが不足しています。')
            print('Pillowをインストールして下さい。\n')
            return False

        # パラメータをチェック
        if not args.s or args.c < 1:
            cls.argsparser.print_help()
            return False

        tmp = os.path.dirname(args.out_file)
        if tmp and not os.path.exists(tmp):
            os.makedirs(tmp)

        # テキストデータを取得
        with open(args.in_file, 'r') as f:
            text = f.read()

        basesize = args.s.split('x')
        basesize[0] = int(basesize[0])
        basesize[1] = int(basesize[1])

        # フォントを生成
        cls.islibraqm = globals()['Features'].check('raqm')
        if args.tate and not cls.islibraqm:
            print('WARN libraqmがインストールされていないため')
            print('     指定されたフォントが縦書き用だと想定して扱います。\n')

        cls.font = cls._generate_font(
            args.font, args.c, basesize, args.dan, args.tate)
        cls.font_width, cls.font_height = cls.font.getsize(cls._BASE_CHAR)

        # 1ページの行数を算出
        if args.tate:
            font_w = (cls.font_width * cls._FONT_ROW_WEIGHT)
            cls.page_rows = int(basesize[0] / font_w)

            cls.base_pos_y = cls.font_height * args.c
            cls.base_pos_x = font_w * cls.page_rows

            if args.dan:
                cls.base_pos_y = cls.base_pos_y * 2 + cls.font_height

            cls.base_pos_y = int((basesize[1] - cls.base_pos_y) / 2)
            cls.base_pos_x = int((basesize[0] - cls.base_pos_x) / 2)

        else:
            font_w = cls.font_height * cls._FONT_ROW_WEIGHT
            cls.page_rows = int(basesize[1] / font_w)

            cls.base_pos_y = font_w * cls.page_rows
            cls.base_pos_x = cls.font_width * args.c

            if args.dan:
                cls.base_pos_x = cls.base_pos_x * 2 + cls.font_width

            cls.base_pos_y = int((basesize[1] - cls.base_pos_y) / 2)
            cls.base_pos_x = int((basesize[0] - cls.base_pos_x) / 2)

        print('  base_pos {}x{}'.format(cls.base_pos_y, cls.base_pos_x))

        # 描画領域を生成
        canvas = globals()['Image'].new('RGB', basesize, args.bg)

        # 描画
        print('画像{}x{}  {}桁{}行  {} {}({}x{})'.format(
            canvas.size[0], canvas.size[1],
            args.c, cls.page_rows,
            cls.font.getname()[0], cls.font.getname()[1],
            cls.font_width, cls.font_height))
        cls._generate(args, text, canvas)

        return True

    #--------------------------------------------------------------------------
    # クラスメソッドの定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------

    @classmethod
    def set_argument(cls, parser_):
        """ sample """
        parser = parser_.add_parser(
            'txt2img',
            formatter_class=argparse.RawDescriptionHelpFormatter,
            description=textwrap.dedent('''\
テキストファイルを画像に変換する
                '''),
            epilog=textwrap.dedent('''--------------------
色
  #ffffff形式や色名で指定可能
  white black red green blue brown gray dark***

フォントファイル
  TrueTypeFont(ttf), OpenTypeFont(otf)形式の固定幅 monospace
  カレントパスにfont.ttfとして置くか、フォントファイルへのパスを指定する。

  Android 5.x  /system/fonts/MTLmr3m.ttf


使い方
  # 縦長(720x1280) 20桁 白色文字 黒色背景 ./font.ttf png形式
  $ python fuzzyuyils.py txt2img gingatetsudono_yoru.txt gingatetsudo

  # 横長(1280x720) 25桁 二段組 白色文字 緑色背景 システムフォント jpg形式
  $ python fuzzyutils.py txt2img text.txt text-nidan -s 1280x720 -c 25 -dan -fg white -bg green -font /system/fonts/MTLmr3m.ttf -fmt jpg
            '''),
            help='テキストファイルを画像に変換する')

        parser.add_argument(
            'in_file',
            type=str,
            metavar='INPUT_FILE',
            help='変換元のテキストファイル utf-8.linux')

        parser.add_argument(
            'out_file',
            type=str,
            metavar='OUTPUT_FILENAME',
            help='出力先画像ファイル 拡張子なし')

        parser.add_argument(
            '-s',
            type=str,
            default='720x1280',
            help='画像サイズ 幅x高さ (720x1280)')

        parser.add_argument(
            '-c',
            type=int,
            default=20,
            help='桁数 (20)')

        parser.add_argument(
            '-dan',
            action='store_true',
            help='二段組')

        parser.add_argument(
            '-tate',
            action='store_true',
            help='縦書き (実験的な実装)')

        parser.add_argument(
            '-fg',
            type=str,
            default='#eeeeee',
            help='文字色 ("#eeeeee")')

        parser.add_argument(
            '-bg',
            type=str,
            default='#000000',
            help='背景色 ("#000000")')

        parser.add_argument(
            '-font',
            type=str,
            default='font.ttf',
            help='フォントファイル (font.ttf)')

        parser.add_argument(
            '-fmt',
            type=str,
            default='png',
            choices=('png', 'jpg'),
            help='出力ファイル形式 (png)')

        cls.argsparser = parser
        parser.set_defaults(func=cls._main)

    #--------------------------------------------------------------------------
    # 特殊メソッドの定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # プロパティーの定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # インスタンスメソッドの定義 (スコープ：クラス内)
    #--------------------------------------------------------------------------

    #--------------------------------------------------------------------------
    # インスタンスメソッドの定義 (スコープ：クラス外)
    #--------------------------------------------------------------------------


#------------------------------------------------------------------------------
# 関数の定義　(スコープ：ファイル内)
#------------------------------------------------------------------------------

def __startup():
    r""" メイン処理(コマンドラインから呼ばれる).

    各種パラメータを整理しメイン処理へ引き渡す。

    Args:
        None
    Returns:
        None
    Raises:
        None
    """
    # 引数パーサーの起動
    parser = argparse.ArgumentParser(
        formatter_class=argparse.RawDescriptionHelpFormatter,
        description=textwrap.dedent('''起動用'''),
        epilog=textwrap.dedent('''\
# コンパイルしておく場合
$ python3 -m compileall text2image.py
            '''))

    # サブコマンドを登録
    subparsers = parser.add_subparsers(dest='SubCommands')
    GenerateTextImage.set_argument(subparsers)
    GenerateTateFont.set_argument(subparsers)

    # サブコマンドを実行
    args = parser.parse_args()
    if hasattr(args, 'func'):
        args.func(args)

    else:
        parser.print_help()


#------------------------------------------------------------------------------
# 関数の定義　(スコープ：ファイル外)
#------------------------------------------------------------------------------


#------------------------------------------------------------------------------
# メイン処理
#------------------------------------------------------------------------------

if __name__ == '__main__':
    __startup()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
