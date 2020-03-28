---
title: tkinter example simple video transcoder gui (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'simple video transcoder gui'

Functions in program: 
* `def main():`
* `def _finish_process(proc):`
* `def run_process(cmd):`
* `def run_ffmpeg(cmd, outfile, filters=None):`
* `def get_tempfile():`
* `def get_cmd(program):`
* `def dbg(*args):`

Modules used in program: 
* `import tkinter.messagebox`
* `import tkinter.filedialog`
* `import tkinter.ttk`
* `import tkinter as tk`
* `import tempfile`
* `import subprocess`
* `import sys`
* `import os`

## python simple video transcoder gui

Python tkinter example: simple video transcoder gui

```python

debug = False
debug_output = False

def dbg(*args):
    if not debug:
        return
    print(*args)

import os
import sys
import subprocess
import tempfile
import tkinter as tk
import tkinter.ttk
import tkinter.filedialog
import tkinter.messagebox
from pathlib import Path
from threading import Thread


def get_cmd(program):
    # first try the local folder
    path = Path(sys.argv[0]).absolute().parent / program
    if path.is_file():
        return str(path)

    # otherwise just assume the program is in PATH
    return program

ffmpeg_cmd = get_cmd('ffmpeg')
gifsicle_cmd = get_cmd('gifsicle')

def get_tempfile():
    file_ = tempfile.NamedTemporaryFile(delete=False, suffix='.png')
    file_.close()
    return file_

def run_ffmpeg(cmd, outfile, filters=None):
    if filters:
        cmd = cmd+['-lavfi', ','.join(filters)]
    cmd = cmd+['-y', outfile]
        
    if debug_output:
        proc = subprocess.Popen(cmd)
    else:
        cmd = cmd+['-loglevel', 'error'] #reduce output
        proc = subprocess.Popen(cmd, stdout=subprocess.DEVNULL, stderr=subprocess.PIPE)
    return _finish_process(proc)

def run_process(cmd):
    if debug_output:
        proc = subprocess.Popen(cmd)
    else:
        proc = subprocess.Popen(cmd, stdout=subprocess.DEVNULL, stderr=subprocess.DEVNULL)
    return _finish_process(proc)

class SubprocessError(Exception):
    def __init__(self, proc):
        super().__init__(proc)
        self.process = proc

def _finish_process(proc):
    retval = proc.wait()
    if retval!=0:
        raise SubprocessError(proc)

class TranscoderWidget(tk.Frame):
    def __init__(self, master, video=None):
        super().__init__(master)
        self._create_gui(video)
    
    def _create_gui(self, video):
        #file selection
        file_box = tk.LabelFrame(self, text='Video')
        file_box.pack(expand=True, fill=tk.X)
        
        self.file_entry = tk.Entry(file_box)
        self.file_entry.pack(side=tk.LEFT, expand=True, fill=tk.X)
        if video:
            self.file_entry.insert(tk.END, video)
        
        file_chooser_button = tk.Button(file_box, text='Select File...', command=self._select_video)
        file_chooser_button.pack(side=tk.LEFT)
        
        #time
        time_box = tk.LabelFrame(self, text='Time')
        time_box.pack(expand=True, fill=tk.X, pady=10)
        
        tk.Label(time_box, text='start: ').grid(row=0, column=0, sticky=tk.E)
        self.start_entry = tk.Entry(time_box)
        self.start_entry.grid(row=0, column=1, sticky=tk.W+tk.E)
        tk.Label(time_box, text='[hh:mm:ss]').grid(row=0, column=2, sticky=tk.W)
        
        tk.Label(time_box, text='duration: ').grid(row=1, column=0, sticky=tk.E)
        self.duration_entry = tk.Entry(time_box)
        self.duration_entry.grid(row=1, column=1, sticky=tk.W+tk.E)
        tk.Label(time_box, text='[hh:mm:ss]').grid(row=1, column=2, sticky=tk.W)
        
        #resolution
        resolution_box = tk.LabelFrame(self, text='Resolution')
        resolution_box.pack(expand=True, fill=tk.X, pady=10)
        
        tk.Label(resolution_box, text='width: ').grid(row=0, column=0, sticky=tk.E)
        self.width_entry = tk.Entry(resolution_box)
        self.width_entry.grid(row=0, column=1, sticky=tk.W+tk.E)
        tk.Label(resolution_box, text='[px]').grid(row=0, column=2, sticky=tk.W)
        
        tk.Label(resolution_box, text='height: ').grid(row=1, column=0, sticky=tk.E)
        self.height_entry = tk.Entry(resolution_box)
        self.height_entry.grid(row=1, column=1, sticky=tk.W+tk.E)
        tk.Label(resolution_box, text='[px]').grid(row=1, column=2, sticky=tk.W)
        
        #output format
        video_format_box = tk.LabelFrame(self, text='Output')
        video_format_box.pack(expand=True, fill=tk.X, pady=10)

        # tk.Label(video_format_box, text='fps: ').grid(row=1, column=0, sticky=tk.E)
        self.fps_entry = tk.Entry(video_format_box)
        # self.fps_entry.grid(row=1, column=1, sticky=tk.W + tk.E)
        # self.fps_entry.insert(tk.END, '25')

        tk.Label(video_format_box, text='format: ').grid(row=2, column=0, sticky=tk.E)
        self.video_format_chooser = tkinter.ttk.Combobox(video_format_box, values=['gif','avi'])
        self.video_format_chooser.grid(row=2, column=1, sticky=tk.W+tk.E)
        self.video_format_chooser.bind('<<ComboboxSelected>>', self._video_format_selected)
        
        self.format_settings_box = tk.Frame(video_format_box)
        self.format_settings_box.grid(row=3, column=1, columnspan=2, sticky=tk.W+tk.E)
        self.format_settings_widgets = {}
        
        #GIF settings
        gif_settings_box = tk.Frame(self.format_settings_box)
        self.format_settings_widgets['gif']= gif_settings_box
        
        self.high_quality_gif_var = tk.IntVar()
        high_quality_button = tk.Checkbutton(gif_settings_box, text='High Quality (slow)', variable=self.high_quality_gif_var)
        high_quality_button.select()
        high_quality_button.pack(anchor=tk.W)
        
        self.compress_gif_var = tk.IntVar()
        compress_button = tk.Checkbutton(gif_settings_box, text='Compress', variable=self.compress_gif_var)
        compress_button.select()
        compress_button.pack(anchor=tk.W)
        
        self.video_format_chooser.set(self.video_format_chooser.cget('values')[0])
        self._video_format_selected()
        
        #convert button
        self.convert_button = tk.Button(self, text='convert', command=self._start_convert)
        self.convert_button.pack(pady=10)
    
    def _select_video(self):
        file_ = tk.filedialog.askopenfilename()
        if file_ is None:
            return
        
        self.file_entry.delete(0, tk.END)
        self.file_entry.insert(tk.END, file_)
    
    def _video_format_selected(self, event=None):
        vformat = self.video_format_chooser.get()
        
        #change the video_format_settings widget
        for widget in self.format_settings_box.winfo_children():
            widget.pack_forget()
        
        widget = self.format_settings_widgets.get(vformat)
        if widget is not None:
            widget.pack()
    
    def _start_convert(self):
        self._converter = Thread(target=self._convert, daemon=True)
        self._converter.start()
        
        self.convert_button.configure(state='disabled', text='converting')
        self.after(500, self._check_finished)
    
    def _convert(self):
        cmd = [ffmpeg_cmd]
        filters = []
        
        start = self.start_entry.get()
        if start:
            cmd+= ['-ss', start]
        
        duration = self.duration_entry.get()
        if duration:
            cmd+= ['-t', duration]
        
        video = self.file_entry.get()
        if not video:
            self._report_error('No input file specified.')
            return
        cmd+= ['-i', video]
        
        width = self.width_entry.get()
        height = self.height_entry.get()
        if width and height:
            filters.append('scale={}:{}'.format(width, height))
        elif width:
            filters.append('scale={}:-1'.format(width))
        elif height:
            filters.append('scale=-1:{}'.format(height))

        fps = self.fps_entry.get()
        if fps:
            cmd+= ['-r', fps]

        vformat = self.video_format_chooser.get().lower()
        outfile = str(Path(video).with_suffix('.'+vformat))
        try:
            if vformat == 'gif':
                if self.high_quality_gif_var.get():
                    #first, generate a suitable color palette
                    dbg('creating palette...')
                    palette_file = get_tempfile()
                    run_ffmpeg(cmd, palette_file.name, filters=filters+['palettegen'])
                    
                    #then create a gif with this palette
                    dbg('converting...')
                    gif_cmd = cmd+['-i', palette_file.name]
                    run_ffmpeg(gif_cmd, outfile, filters=filters+['paletteuse'])
                    
                    os.remove(palette_file.name)
                else:
                    dbg('converting...')
                    run_ffmpeg(cmd, outfile, filters=filters)
                
                if self.compress_gif_var.get():
                    dbg('compressing...')
                    compress_cmd = [gifsicle_cmd, '-b', '--optimize=3', '--careful', outfile]
                    run_process(compress_cmd)
            elif vformat == 'apng':
                cmd = cmd+['-plays', '0']
                run_ffmpeg(cmd, outfile, filters=filters)
            elif vformat == 'webp':
                cmd = cmd+['-loop', '0']
                run_ffmpeg(cmd, outfile, filters=filters)
            elif vformat == 'mng':
                cmd = cmd+['-loop', '0']
                run_ffmpeg(cmd, outfile, filters=filters)
            elif vformat == 'avi':
                #~ cmd = cmd+['-codec', 'copy']
                cmd = cmd+['-b', '700k', '-qscale', '0', '-ab', '160k', '-ar', '44100']
                run_ffmpeg(cmd, outfile, filters=filters)
            elif vformat == 'wmv':
                cmd = cmd+['-c:v', 'wmv2', 'b:v', '1024k', '-c:a', 'wmav2', '-b:a', '192k']
                run_ffmpeg(cmd, outfile, filters=filters)
            else:
                run_ffmpeg(cmd, outfile, filters=filters)
            
            dbg('finished.')
        except SubprocessError as e:
            self._report_error(e)
    
    def _report_error(self, error):
        def report():
            if isinstance(error, SubprocessError):
                proc = error.process
                if proc.stderr is None:
                    msg = 'Process "{}" failed with error code {}'.format(proc.args[0], proc.returncode)
                else:
                    msg = proc.stderr.read()
            else:
                msg = str(error)
            
            tkinter.messagebox.showerror('Conversion failed', msg)
        
        self.after(0, report)
    
    def _check_finished(self):
        if not self._converter.is_alive():
            self.convert_button.configure(state='normal', text='convert')
            return
        
        text = self.convert_button.cget('text')
        dots = (text.count('.', -3)+1) % 4
        text = text.rstrip('.')+ '.'*dots
        self.convert_button.configure(text=text)
        
        self.after(500, self._check_finished)

def main():
    if len(sys.argv) == 1:
        video = ''
    else:
        video = sys.argv[1]

    win = tk.Tk()
    win.title('video transcoder')
    win.resizable(False, False)
    TranscoderWidget(win, video).pack(expand=True, fill=tk.BOTH)
    win.mainloop()

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
