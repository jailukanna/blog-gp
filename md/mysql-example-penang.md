---
title: mysql example penang (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'penang'

Functions in program: 
* `def extract(tag, entry):`

Modules used in program: 
* `import sys`
* `import os`
* `import _mysql`

## python penang

Python mysql example: penang

```python
#!/usr/bin/env python

"""Penang Tunes parser by killkeeper
   killkeeper@gmail.com

   Usage: $ python penang.py [directory]
"""

import _mysql
from mutagen.id3 import ID3
import os
import sys

DB_USER = 'user'
DB_PASS = '123456'

def extract(tag, entry):
    if entry in tag:
        try:
            return tag[entry].text[0].encode('utf8').replace('"', '\\"')
        except:
            print('Bad ID3 tag encountered.')
            sys.exit(1)

if len(sys.argv) < 2:
    print('Usage: %s directory' % sys.argv[0])

for root, dirs, files in os.walk(sys.argv[1]):
    mp3_files = filter(lambda x: x.endswith(".mp3"), files)

    tracks = []
    consensus_alb = []
    consensus_ply = []
    for tune in mp3_files:
        tag = ID3(tune)
        track_no = extract(tag, 'TRCK')
        if track_no.find('/') != -1:
            track_no = int(track_no[:track_no.find("/")])
        else:
            track_no = int(track_no)

        title = extract(tag, 'TIT2')
        album = extract(tag, 'TALB')
        artist = extract(tag, 'TPE1')

        tracks.append((track_no, album, artist, title))
        consensus_alb.append(album)
        consensus_ply.append(artist)

        if track_no:
            print('Renaming %s to %d.mp3' % (tune, track_no))
            os.rename(tune, '%d.mp3' % track_no)

    if len(set(consensus_alb)) > 1:
        print('Album is not unique.')
        sys.exit(1)

    if len(tracks) == 0:
        print('No valid tunes.')
        sys.exit(0)

    album_name = tracks[0][1]
    artist_name = tracks[0][2] if len(set(consensus_ply)) == 1 else 'Various Artists'

    try:
        db = _mysql.connect(host="localhost", user=DB_USER, passwd=DB_PASS, db="dfmusic", unix_socket="/tmp/mysql.sock")
        db.query("SET NAMES 'utf8';")

        # insert an new album 
        album_id = int(os.path.basename(sys.argv[1]))
        db.query('INSERT INTO albums (album_id, album_name, album_artist) VALUES (%d, "%s","%s")' % (album_id, album_name, artist_name))
        
        tracks.sort(cmp=lambda x,y:cmp(x[0], y[0]))
        if album_id:
            fields = ",".join(['(%d, "%s", %d)' % (album_id, track[3], track[0]) for track in tracks])
            db.query("INSERT INTO songs (song_album_id, song_name, song_pos) VALUES %s" % fields)

        db.close()
    except Exception, e:
        print('Failed to operate database.')
        print(e)
        sys.exit(1)

    print('Done. %d tunes processed.' % len(tracks))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
