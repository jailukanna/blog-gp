---
title: tkinter example doublons (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'doublons'

Functions in program: 
* `def main():`
* `def find_collisions(files):`
* `def compute_dist (w1, w2):`
* `def levenshtein(mot1,mot2, couts=(1,1,1)):`
* `def list_files():`
* `def normalize_str(input_str):`

Modules used in program: 
* `import unicodedata`
* `import re, os, sys, mimetypes, unicodedata, threading`

## python doublons

Python tkinter example: doublons

```python
#!/usr/bin/env python3
#coding: utf8
########################
###    doublons.py   ###
### By Ophir LOJKINE ###
### LICENSE : LGPLv3 ###
########################

import re, os, sys, mimetypes, unicodedata, threading

#If a word is used in more than 10% of all filenames, consider it as meaningless
uselessrate = 0.1
#But if it appears only 3 times or less, consider it is still usefull
uselessmin = 3
#Don't compare files that are inside the same directory
notSameDir = True
#The distance below which to consider there is a collision
threshold = 0.5

appname = "Doublons (ophir)"
versionstring = "0.01"

#Consider only files of these types
filetypes = ("video", "audio")

try:
	import tkinter as tk
	import tkinter.ttk
	class TableApp(tk.Frame):
		def __init__(self, root):
			self.root=root
			tk.Frame.__init__(self, root)
			self.root.title('%s - v.%s' % (appname, versionstring))
			for k, colname in enumerate(("File 1", "File 2", "dist")):
				b = tk.Button(self.root, text=colname)
				b.grid(row=0,column=k,sticky=tk.NSEW)
			self.root.grid_columnconfigure(0, weight=1)
			self.root.grid_columnconfigure(1, weight=1)
			self.root.grid_columnconfigure(2, weight=0)

			self.progressBar = tk.ttk.Progressbar(self.root)
			self.progressBar.grid(row=1, column=0, columnspan=3, sticky=tk.NSEW)
			self.progressBarShown = True

			self.num_row = 1

		def update_progress(self, progress):
			if (self.progressBarShown):
				self.progressBar["value"] = progress*100

		def create_label(self, i,j, text):
			if (self.progressBarShown):
				self.progressBarShown = False
				self.progressBar.destroy()

			l = tk.Label(self.root, text=text)
			l.grid(row=i, column=j, sticky=tk.NSEW)

		def display_collision(self, dist, filenames, roots):
			for j in range(2):
				self.create_label(self.num_row, j, "%s\n(in %s)"%(filenames[j], roots[j]))
			self.create_label(self.num_row, 2, "%.3f" % dist)
			self.root.grid_rowconfigure(self.num_row, weight=1)
			self.num_row += 1
	master = tk.Tk()
	app = TableApp(master)

except ImportError:
	app = False

try:
	searchdir = sys.argv[1]
except:
	try:
		import tkinter.filedialog
		searchdir = str(tkinter.filedialog.askdirectory())
	except:
		searchdir = input("Which folder do you want to check for duplicates?\nFolder: ")
		searchdir = searchdir.strip("\"' ")

regxWords = re.compile("[\W_]+")

if not os.path.isdir(searchdir):
	print("'%s' is not a valid directory" % searchdir)
	sys.exit(1)

import unicodedata

def normalize_str(input_str):
	input_str = input_str.lower()
	nkfd_form = unicodedata.normalize('NFKD', input_str)
	only_ascii = nkfd_form.encode('ASCII', 'ignore')
	return only_ascii.decode("utf8")

def list_files():
	files = []
	allwords = {}
	nfilms = 0

	for root, dirs, dirfiles in os.walk(searchdir):
		for f in dirfiles:
			filetype = mimetypes.guess_type(f)[0]
			if filetype and filetype.split("/")[0] in filetypes:
				fname = os.path.splitext(f)[0]
				words = [ normalize_str(w) for w in regxWords.split(fname) ]
				for word in words: allwords[word] = allwords.get(word,0) + 1
				files.append([words, root, f])
				nfilms += 1

	useless = lambda w: allwords[w] >= uselessmin and allwords[w]/nfilms > uselessrate
	uselesswords = tuple(w for w in sorted(allwords, key=allwords.get) if useless(w))
	for i, (words,root,f) in enumerate(files):
		#Create clean file names
		files[i][0] = tuple(filter(lambda w:w not in uselesswords, words))
	return files

def levenshtein(mot1,mot2, couts=(1,1,1)):
	"""Calcule la distance de levenshtein entre mot1 et mot1.
	Le vecteur coûts indique, dans l'ordre, le coût d’une suppression (dans mot1 pour passer à mot2),
	d'un ajout, et d'un remplacement."""
	ligne_i = [ k for k in range(len(mot1)+1) ]
	for i in range(1, len(mot2) + 1):
		ligne_prec = ligne_i
		ligne_i = [i]*(len(mot1)+1)
		for k in range(1,len(ligne_i)):
			cout = 0 if mot1[k-1] == mot2[i-1] else couts[2]
			ligne_i[k] = min(ligne_i[k-1] + couts[0], ligne_prec[k] + couts[1], ligne_prec[k-1] + cout)
	return ligne_i[len(mot1)]

def compute_dist (w1, w2):
	"""Estimate the distance between w1 and w2 bby a number between 0 and 1"""
	return levenshtein(w1,w2,(1,1,2)) / max(len(w1),len(w2))

def find_collisions(files):
	collisions = []

	numCompTot = len(files)*(len(files)-1) / 2
	numComp = 0

	for (w1, root1, f1) in files:
		for (w2, root2, f2) in files:
			if (root1 == root2):
				if f1 == f2: break
				elif notSameDir:
					numCompTot -= 1
					continue #Don't compare files that are in the same directory
			sw = [" ".join(w) for w in (w1,w2)]
			numComp += 1
			if app:
				app.update_progress(numComp/numCompTot)
			else:
				print("Calculating the distances (%d %%)" % (100*numComp/numCompTot), end='\r')
			dist = min(compute_dist(w1,w2), compute_dist(sw[0], sw[1]))
			if dist < threshold:
				collisions.append((dist, (f1,root1), (f2,root2)))
	print("\n")

	if not collisions:
		print("No duplicate found.")

	collisions.sort()
	for dist, (f1,root1), (f2,root2) in collisions:
		if app:
			app.display_collision(dist, (f1,f2), (root1,root2))
		else:
			print("""Potential collision (dist = %f)
	%s\t(in %s)
	%s\t(in %s)
			""" % (dist, f1,root1, f2,root2))

def main():
	print("Listing files...")
	files = list_files()
	print("Finding duplicates...")
	find_collisions(files)

if app:
	t = threading.Thread(target = main)
	t.start()
	app.mainloop()
else:
	main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
