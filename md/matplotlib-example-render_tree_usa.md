---
title: matplotlib example render tree usa (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'render tree usa'

Functions in program: 
* `def get_color_name(state):`

Modules used in program: 
* `import matplotlib.colorbar as cb`
* `import matplotlib.colors as colors`
* `import matplotlib.cm as cm`
* `import matplotlib.pyplot as plt`
* `import matplotlib.gridspec as gridspec`
* `import pandas as pd`
* `import numpy as np`
* `import datetime`

## python render tree usa

Python matplotlib example: render tree usa

```python
from Bio import Phylo
import datetime
from decimal import Decimal

import numpy as np

import pandas as pd

import matplotlib.gridspec as gridspec
import matplotlib.pyplot as plt
import matplotlib.cm as cm
import matplotlib.colors as colors
import matplotlib.colorbar as cb
from matplotlib.patches import Polygon, Circle, PathPatch
from matplotlib import rc

from mpl_toolkits.basemap import Basemap

rc('font',**{'family':'sans-serif','sans-serif':['Helvetica'], 'size': 18})

tp = Phylo.parse("../2018.10.17/RAxML_bipartitions.2018.10.17.wnv.usa", 'newick')
tree = None
for t in tp:
    tree = t

# Root at oldest sequence
_min_date = datetime.datetime(2018, 8, 18, 10, 9, 9, 425642)
root = None
for i in tree.get_terminals():
    _ = datetime.datetime.strptime(i.name.split("_")[1], "%Y-%m-%d")
    if _min_date > _:
        _min_date = _
        root = i
    i.name = i.name.replace("SanFransisco", "SanFrancisco")

tree.root_with_outgroup(root)
# tree.root_at_midpoint()
tree.ladderize()

# Write tree to file
Phylo.write(tree, "../2018.10.17/RAxML_bipartitions.2018.10.17.wnv.usa.rerooted.nwk", "newick")

# Get list of states other than CA
states = [i.name.split("_")[3].lower() for i in tree.get_terminals() if i.name.split("_")[3]!="CA"]
states = list(set(states))
states.sort()

cnorm_state = colors.Normalize(vmin=0, vmax=len(states))
smap_state = cm.ScalarMappable(norm=cnorm_state, cmap=cm.Dark2)

for _i, i in enumerate(tree.get_terminals()):
    i.y = _i
    i.x = tree.distance(i)

for i in reversed(tree.get_nonterminals()):
    _ = i.clades
    i.y = (_[0].y + _[-1].y)/2
    i.x = tree.distance(i)

f = plt.figure(figsize=(20,25))
gs = gridspec.GridSpec(2, 2, width_ratios=[1,0.75], height_ratios = [0.5,0.5])

ax = plt.subplot(gs[:,0])
rtax = plt.subplot(gs[0, 1])
cax = plt.subplot(gs[1, 1])

ax.set_title("A. Maximum likelihood tree")
rtax.set_title("B. Root to tip regression plot")
cax.set_title("C. Legend")

# Draw California
ll_lng = -125.0
ll_lat = 25.0
ur_lat = 49.5
ur_lng = -66.96
m = Basemap(projection='merc', resolution = 'i', llcrnrlon= ll_lng , llcrnrlat=ll_lat, urcrnrlon=ur_lng, urcrnrlat=ur_lat, ax = cax)
m.readshapefile("../../shapefile/gadm36_USA_shp/gadm36_USA_1", "units", drawbounds=False)
centroids = []
patches = []
for info, shape in zip(m.units_info, m.units):
    if info["NAME_1"] == "Alaska" or info["NAME_1"] == "Hawaii":
        continue
    poly = Polygon(np.array(shape), True)
    poly.set_linewidth(0.5)
    poly.set_edgecolor("#000000")    
    poly.set_zorder(1)
    poly=cax.add_patch(poly)
    patches.append(poly)
    x, y = zip(*shape)
    centroids.append({
        "x": np.mean(x),
        "y": np.mean(y),
        "name": info["HASC_1"][3:],
        "RINGNUM": info["RINGNUM"]
    })


_ = [i["x"] for i in centroids]
cNorm  = colors.Normalize(vmin=np.min(_), vmax=np.max(_))
smap_state = cm.ScalarMappable(norm=cNorm, cmap=cm.plasma)

for i in range(0, len(patches)):
    patches[i].set_facecolor(smap_state.to_rgba(centroids[i]["x"]))
    patches[i].set_zorder(1)
    patches[i].set_alpha(1)

    m.scatter([i["x"] for i in centroids],[i["y"] for i in centroids],color="#000000",marker="o",s=0)

centroids = pd.DataFrame(centroids)

def get_color_name(state):
    _ = centroids[centroids["name"].str.lower().str.replace(" ", "") == state.lower()]
    _ =  _["x"].mean()
    return smap_state.to_rgba(_)

# Plot branches
_ = {
    "x": [],
    "y": [],
    "c": []
}
for i in tree.get_nonterminals():
    for j in i.clades:
        _t = ax.plot([i.x, i.x], [i.y, j.y], ls='-', color="#000000", zorder = 1)
        _t = ax.plot([i.x, j.x], [j.y, j.y], ls='-', color="#000000", zorder = 1)
        if j.confidence == None:
            continue
        if j.confidence >= 75:
            _["x"].append(j.x)
            _["y"].append(j.y)
            _["c"].append("#000000")
        elif j.confidence >= 50:
            _["x"].append(j.x)
            _["y"].append(j.y)
            _["c"].append("#FFFFFF")

ax.scatter(_["x"], _["y"], c = "#000000", s = 50, zorder = 2)
ax.scatter(_["x"], _["y"], c = _["c"], s = 25, zorder = 2)

# for i in tree.get_nonterminals():
#     if i.branch_length != None:
#         _ = ax.plot(i.x, i.y, marker = 'o', color='#000000')
#         _ = ax.text(i.x, i.y+1, str(i.y))

_ = {
    "x": [],
    "y": [],
    "c": []
}
for i in tree.get_terminals():
    c = (150,75,0,1)
    c = [i/255 for i in c]
    c[-1] = 1
    c =get_color_name(i.name.split("_")[3].lower())
    _["x"].append(i.x)
    _["y"].append(i.y)
    _["c"].append(c)
    # ax.text(i.x+0.001, i.y, i.name.split("_")[3])
    # if i.name in hp["taxon name"].tolist():
    #     ax.text(i.x + 0.000025, i.y, i.name.split("_")[0] + " " + str(hp[hp["taxon name"] == i.name]["#of homoplasic mutations"].values[0]))

ax.scatter(_["x"], _["y"], c = "#000000", s = 100, zorder = 2)
ax.scatter(_["x"], _["y"], c = _["c"], s = 50, zorder = 2)
ax.set_yticks([])

for i in np.arange(0, 0.014, 0.004):
    ax.axvspan(i, i+0.002, color = "#ECECEC", zorder = -1)

df = pd.read_table("../2018.10.17/clock_rate.tsv", sep ="\t")

fit = np.polyfit(df["date"],df["distance"],1)
fit_fn = np.poly1d(fit)

_ = np.arange(1995, 2021)
rtax.plot(_, fit_fn(_), "--k")
rtax.set_xlim([1995,2020])
rtax.set_ylim([0,0.014])

c = []
for j in df.index.values:
    i = df.ix[j]["tip"]
    _ = get_color_name(i.split("_")[3].lower())
    c.append(_)
    # if i in hp["taxon name"].tolist():
    #     rtax.text(df.ix[j]["date"], df.ix[j]["distance"], i.split("_")[0])


_ = "%.2E" % Decimal((np.diff(fit_fn(_)[:2]) / np.diff(_[:2]))[0])
_ += " subs/site/year"
_ = "slope = " + _
rtax.text(1996, 0.012, _)

rtax.scatter(df["date"], df["distance"], c = "#000000", s = 100)
rtax.scatter(df["date"], df["distance"], c = c, s = 50)

plt.tight_layout()
plt.savefig("../2018.10.17/2018.10.17.wnv.state.png")
plt.clf()
plt.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
