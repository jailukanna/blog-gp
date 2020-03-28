---
title: tkinter example recoveryCHK (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'recoveryCHK'

Functions in program: 
* `def main():`

Modules used in program: 
* `import os`
* `import tkinter.filedialog as fd`
* `import tkinter`
* `import filetype`

## python recoveryCHK

Python tkinter example: recoveryCHK

```python
import filetype
import tkinter
import tkinter.filedialog as fd
import os

def main():
    dir_path = fd.askdirectory()

    for file in os.listdir(dir_path):
        file_path = os.path.join(dir_path, file)
        if not os.path.isfile(os.path.join(dir_path, file)):
            continue
        splited_file_path =os.path.splitext(file_path)
        file_name = splited_file_path[0]
        file_ext = splited_file_path[1]
        #if file_ext != 'chk' and file_ext != 'CHK':
        #    continue
        extension = filetype.guess_extension(file_path)
        if extension is None:
            print("恢复"+file+"失败，无法猜测格式！")
            continue
        file_name = os.path.splitext(file_path)[0]
        os.rename(file_path, file_name + "." + extension)
        print("恢复"+file+"成功！")
    print('恢复完成！')

if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
