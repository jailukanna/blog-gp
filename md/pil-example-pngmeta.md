---
title: pil example pngmeta (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'pngmeta'

Functions in program: 
* `def decode_if_json(val):`
* `def parse_pairs(pairs):`

Modules used in program: 
* `import json, optparse, sys`

## python pngmeta

Python pil example: pngmeta

```python
#!/usr/bin/env python

import json, optparse, sys

try:
    import PIL.Image, PIL.PngImagePlugin
except:
    print(>> sys.stderr, "Unable to import Python Imaging Library. Please ensure that it is installed.")
    sys.exit(1)    

# PIL.PngImagePlugin.PngStream mixes these keys in with meta-data, even though they're not from tEXt or zTXt chunks.
reserved_keys = ('interlace', 'gamma', 'dpi', 'transparency', 'aspect', 'icc_profile')

class MyParser(optparse.OptionParser):
    def format_epilog(self, formatter):
        return "".join(formatter.format_epilog(paragraph) for paragraph in self.epilog)

version = "%prog 1.4"
usage = "usage: %prog [options] pngfile [pngfile ...]"
description = "Edits and displays arbitrary meta-data in one or more png files. Requires the Python Imaging Library (PIL)."
epilog = [
    "This tool ignores the following reserved keys: (%s)." % ', '.join(reserved_keys),
    "This tool can't differentiate between values stored in tEXt and zTXt chunks when reading.",
    "Most limitations of this tool are inherited from the PngStream class of the PngImagePlugin module in the Python Imaging Library."]
parser = MyParser(version=version, usage=usage, description=description, epilog=epilog)
readinggroup = optparse.OptionGroup(parser, "Reading meta-data", "All meta-data key=value pairs in all png files are output by default.")
readinggroup.add_option("-q", "--quiet", dest="quiet", default=False, action="store_true", help="Supress per-file headers and errors.")
readinggroup.add_option("-s", "--silent", dest="silent", default=False, action="store_true", help="Supress all output.")
readinggroup.add_option("-g", "--get", dest="get", metavar="KEY", default=None, action="append", help="Retrieve the value of a single meta-data key. Can be used multiple times.")
removegroup = optparse.OptionGroup(parser, "Removing meta-data", "Options that delete meta-data will be applied before options that add or change meta-data.")
removegroup.add_option("-c", "--clear", dest="clear", default=False, action="store_true", help="Clear all png meta-data in the file.")
removegroup.add_option("-d", "--delete", dest="delete", metavar="KEY", default=None, action="append", help="Delete a single meta-data key. Can be used multiple times.")
updategroup = optparse.OptionGroup(parser, "Updating meta-data")
updategroup.add_option("-f", "--file", dest="file", default=None, help="Read meta-data key=value pairs from a file, one per line. Use - to read from stdin.")
updategroup.add_option("-k", "--key", dest="keys", metavar="META", default=None, action="append", help="Add or update a single meta-data key, using a \"key=value\" pair. Can be used multipe times.")
updategroup.add_option("", "--force-save", dest="force", default=False, action="store_true", help="Re-save the png file, even if no key changes are made. Useful for converting meta-data from one chunk-type to another.")
formatgroup = optparse.OptionGroup(parser, "Format options")
formatgroup.add_option("-z", "--ztxt", dest="ztxt", default=False, action="store_true", help="Store meta-data using a zTXt (compressed) chunk instead of the defaut tEXt chunk. (Note: all existing meta-data will be moved to the chosen format)")
formatgroup.add_option("-j", "--json", dest="json", default=False, action="store_true", help="Handle file input and command output as JSON instead of \"key=value\" pairs.")
parser.add_option_group(readinggroup)
parser.add_option_group(removegroup)
parser.add_option_group(updategroup)
parser.add_option_group(formatgroup)
parser.formatter.max_help_position = 26
(options, args) = parser.parse_args()

if len(args) == 0: parser.error("You must specify at least one png file. Use -h for help.")
if options.silent:
    if options.get is not None: parser.error("Using --get with --silent has no effect.")
    if not options.clear and options.delete is None and options.file is None and options.keys is None and not options.ztxt:
        parser.error("No operations specified. Use -h for help.")

# A couple convenience functions
def parse_pairs(pairs):
    for pair in pairs:
        key, sep, val = pair.partition('=')
        if sep == '=':
            yield (key.strip(), val.strip())
        else:
            print(>> sys.stderr, "Parsing error: %p" % pair)

def decode_if_json(val):
    try:
        return json.loads(val)
    except:
        return val

# Handle file input (or stdin)
filekeys = {}
if options.file is not None:
    if options.file == '-':
        infile = sys.stdin
    else:
        try:
            infile = open(options.file)
        except IOError as e:
            print(>> sys.stderr, "Error: Unable to read input file %s: %s" % (options.file, e[1]))
            sys.exit(1)
    if options.json:
        for key, val in json.load(infile).iteritems():
            key=str(key)
            if key in reserved_keys: continue
            if val.__class__ in (unicode, int, float, bool):
                filekeys[key] = val
            else:
                filekeys[key] = str(json.dumps(val))
    else:
        filekeys = dict(parse_pairs(infile.readlines()))
    if infile is not sys.stdin: infile.close()

# Loop through png files and process each one according to options
exitcode = 0
for filename in args:
    try:
        image = PIL.Image.open(filename)
        info = dict([(k,v) for k, v in image.info.iteritems() if k not in reserved_keys])
    except IOError as e:
        if len(e.args) > 1:
            msg = e[1]
        else:
            msg = e[0]
        if not options.quiet: print(>> sys.stderr, "\nError: Unable to read png file %s: %s" % (filename, msg))
        exitcode = 1
        continue

    dirty = False
    
    if options.clear:
        dirty = True
        info.clear()
    
    if options.delete is not None and not options.clear:
        for key in options.delete:
            if key in info:
                dirty = True
                del(info[key])
    
    if options.file is not None:
        dirty = True
        info.update(filekeys)

    if options.keys is not None:
        dirty = True
        info.update(parse_pairs(options.keys))

    if options.get is not None:
        for key in options.get:
            if options.json:
                print(decode_if_json(info[key]))
            else:
                print(info[key])

    if dirty or options.force:
        meta = PIL.PngImagePlugin.PngInfo()
        for k,v in info.iteritems():
            if k not in reserved_keys: meta.add_text(k, v, options.ztxt)
        try:
            image.save(filename, "PNG", pnginfo=meta)
        except IOError as e:
            print(>> sys.stderr, "Unable to save %s: %s" % (filename, e[1]))
            exitcode = 1
            continue

    if not options.silent and options.get is None:
        if not options.quiet and len(args) > 1: print("\n --- %s --- " % filename)
        if options.json:
            print(json.dumps(dict([(key, decode_if_json(val)) for key, val in info.iteritems()])))
        else:
            for meta in info.iteritems():
                print("%s=%s" % meta)

sys.exit(exitcode)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
