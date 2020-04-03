---
title: matplotlib example plot all (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot all'

Functions in program: 
* `def main(af):`
* `def make_energy_plots():`
* `def make_sd_plots():`
* `def convert_to_matplotlib(curves, hists, yscale, xr, ymin, units, axs, `
* `def get_plot_sd_en(an):`
* `def get_plot_type(an):`

Modules used in program: 
* `import os`
* `import math`
* `import matplotlib.gridspec as gridspec`
* `import copy`
* `import cPickle as pick`
* `import numpy`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import sys`
* `import ROOT`

## python plot all

Python matplotlib example: plot all

```python
import ROOT
import sys
import matplotlib
if matplotlib.get_backend() != "MacOSX": matplotlib.use('QT4Agg')
import matplotlib.pyplot as plt
import numpy
import cPickle as pick
import copy
import matplotlib.gridspec as gridspec
from utilities import add_subplot_axes
import math
from textwrap import wrap
import os
ROOT.RooCurve()


sheet_name = { 'cc_energy_ss' : "Energy SS (a)",
               'cc_energy_ms' : "Energy MS (b)"}

ns = 18
#matplotlib.rcParams.update({'font.size': ns})
#matplotlib.rc('xtick', labelsize=ns)
#matplotlib.rc('ytick', labelsize=ns)
matplotlib.rcParams['ytick.major.pad']='8'
matplotlib.rcParams['ps.fonttype']=42
matplotlib.rcParams['ps.useafm']=True
#matplotlib.rc('text', usetex=True)

integrate_above = 4000
integrate_below = 9800
overflow_bin = [4000, 4200]

legend_wrap_txt = 33

plot_types = {
  "bb2n_curve" : {
     "label" : r"$2\nu\beta\beta$",
     "lw" : 2,
     "ls" : "-",
     "color" : '0.85',
  },
  "rn_curve" : {
     "label" : r"Rn",
     "lw" : 2,
     "ls" : ":",
     "color" : 'r',
  },
  "vessel_curve" : {
     "label" : "\n".join(wrap(r"Vessel ($^{40}$K, $^{60}$Co, $^{65}$Zn, $^{232}$Th, $^{238}$U)", legend_wrap_txt)),
     "lw" : 2,
     "ls" : "-.",
     "color" : 'g',
     "dashes" : [10, 5],
  },
  "neutron_curve" : {
     "label" : r"$n$-capture",
     "lw" : 2,
     "ls" : "-",
     "color" : 'y',
  },
  "lxe_curve" : {
     "label" : r"$^{135}$Xe, $^{137}$Xe",
     "lw" : 2,
     "ls" : ":",
     "dashes" : [8, 4, 2, 4],
     "color" : 'k',
  },
  "bb0n_Pdf" : {
     "label" : r"$0\nu\beta\beta$",
     "lw" : 2,
     "ls" : "-",
     "color" : 'm',
  },
  "InnerCryo_Th232_Pdf" : {
     "label" : r"$^{232}$Th (far)",
     "lw" : 2,
     "ls" : "--",
     "color" : 'c',
  },
  "best_fit_curve" : {
     "label" : r"Best Fit",
     "color" : "b",
     "lw" : 2,
     "ls" : "-",
  },
}

overflow_label = "Counts (%i - %i keV)" % (integrate_above, integrate_below)
overflow_x_value = (integrate_below + integrate_above)/2.

plot_ordering = ["Data", "Best Fit"]

def get_plot_type(an):
    for k in plot_types:
        if k in an: return plot_types[k]
    return {}


x_range = {
   "energy" : {
     "xr" : [980, overflow_bin[-1]],
     "ymin" : 0.08, 
     "yscale" : "log", 
     "x_title" : "Energy (keV)", 
     "units" : "keV", 
   }
   ,
   "standoff" : { 
     "xr" : [0, 200],
     "ymin" : 0, 
     "yscale" : "linear", 
     "x_title" : "Standoff distance (mm)", 
     "units" : "mm", 
   }
}

def get_plot_sd_en(an):
    for k in x_range:
        if k in an: return x_range[k]
    return {}


def convert_to_matplotlib(curves, hists, yscale, xr, ymin, units, axs, 
      x_title=None, ymax=None, fs=ns, plot_int_pt=True, integrate_all=False):
    c_to_plot = {}
    f = float

    for c in curves:
        x, y = ROOT.Double(), ROOT.Double() 
        c_to_plot[c.GetName()] = numpy.array([(f(x),f(y)) for i in range(c.GetN()) if c.GetPoint(i, x,y) != -1])


    estimated_value = 0.0
    plot_after = [] 
    for n, c in c_to_plot.items():
        adict = get_plot_type(n) 
        c = c[numpy.where(c[:,1] != 0.0)]
        d = c[numpy.where(c[:,0] >= integrate_above)] 
        tsum =  sum(d[:,1])/2.
        ndict = copy.deepcopy(adict)
        lab = ndict["label"]
        del ndict["label"] 
        if n == "best_fit_curve":
            plot_after.append((axs.plot, overflow_bin, [tsum,tsum], ndict))
        else:
            l, = axs.plot(overflow_bin, [tsum, tsum], **ndict)
            if "dashes" in adict: l.set_dashes(adict["dashes"])
        if n == "best_fit_curve" and integrate_all:
            estimated_value = tsum 
        if n == "bb2n_curve": adict = ndict
            
        c = c[numpy.where(c[:,0] < integrate_above)] 


        if n == "best_fit_curve":
            plot_after.append((axs.plot, c[:,0], c[:,1], adict))
            continue
        l, = axs.plot(c[:,0], c[:,1], **adict)
        if "dashes" in adict: l.set_dashes(adict["dashes"])
        if n == "bb2n_curve":
            axs.fill_between(c[:,0], ymin, c[:,1], facecolor=plot_types['bb2n_curve']['color'])
            p = plt.Rectangle((0,0), 1,1, color=plot_types['bb2n_curve']['color'], label=plot_types['bb2n_curve']['label'])
            axs.add_patch(p)

    for f, x, y, d in plot_after:
        f(x, y, **d)

    interval = ROOT.RooHistError.instance().getPoissonInterval
    ret_val = 0.0
    for o in hists:
        x, x_eh, x_el = o.GetX(), o.GetEXhigh(), o.GetEXlow()
        y, y_eh, y_el = o.GetY(), o.GetEYhigh(), o.GetEYlow()
        t = numpy.array([(x[i], x_el[i], x_eh[i], y[i], y_el[i], y_eh[i]) for i in range(o.GetN())]) 
        
        indices = numpy.where(t[:,0] >= integrate_above) 
        indic2 = numpy.where(t[:,0] < integrate_above) 
        num_bins = len(indices[0])
        xs = t[:,0]
        bin_width = x[1] - x[0]

        if num_bins > 0 and integrate_all:
            at = t[indices] 
            at1 = t[indic2] 
            xavg = at[:,0].mean()
            ytot = at[:,3].sum()
        
            print("Seen total: ", ytot)
            #print(at[numpy.where(at[:,3] > 0),0])
            np, nm = ROOT.Double(), ROOT.Double()
            interval(int(ytot), nm, np)
        else:
            at1 = t
        fmt_dict = { "fmt" : "o", "color" : "k", "capsize" : 0 }
        x, y, xerr, yerr = at1[:,0], at1[:,3], [at1[:,1], at1[:,2]], [at1[:,4], at1[:,5]]
        axs.errorbar(x, y,
          xerr=xerr,
          yerr=yerr,
          label="Data", **fmt_dict)
        if num_bins > 0 and plot_int_pt and integrate_all:
            axs.errorbar([sum(overflow_bin)/2.], [ytot], 
              yerr=[[(ytot-nm)], [(np-ytot)]], **fmt_dict)
            div = np

            if ytot >= estimated_value:
                div = nm 
            ret_val = (ytot - estimated_value)/abs(ytot-div)


    axs.set_yscale(yscale)
    axs.set_xlim(xr)

    if x_title: axs.set_xlabel(x_title, fontsize=fs)
    axs.set_ylabel("Counts", fontsize=fs)
    amin, amax = axs.get_ylim()
    if ymax: amax = ymax
    axs.set_ylim([ymin, amax])
    plt.setp(axs.get_xticklabels(), fontsize=fs)
    plt.setp(axs.get_yticklabels(), fontsize=fs)
    return ret_val

    

obj = None 

# Profile plot stuff


def make_sd_plots():
    sf = 1.5
    fig = plt.figure(figsize=(sf*4.25,sf*5.5))
    ax_m = fig.add_subplot(111)
    ax_m.patch.set_alpha(0.0)
    ax1  = fig.add_subplot(211)
    ax2  = fig.add_subplot(212, sharex=ax1)
    
    ax_m.spines['top'].set_color('none')
    ax_m.spines['bottom'].set_color('none')
    ax_m.spines['left'].set_color('none')
    ax_m.spines['right'].set_color('none')
    ax_m.tick_params(labelcolor='w', top='off', bottom='off', left='off', right='off')
    for key,ax, l in [("cc_standoff_ss", ax1, "(a) SS"), ("cc_standoff_ms",ax2, "(b) MS")]:
        it = obj[key]
        if "fit" not in it: continue 
        
        xr = copy.deepcopy(get_plot_sd_en(key))
        if ax == ax1: del xr["x_title"]
        convert_to_matplotlib(*it["fit"], axs=ax, **xr)
        ax.set_ylabel("")
        yloc = plt.MaxNLocator(3)
        ax.yaxis.set_major_locator(yloc)
        plt.text(0.7, 0.8, l, fontsize=ns,transform=ax.transAxes)
    #fig.text(0.04, 0.5, 'Counts', ha='center', va='center', rotation='vertical', fontsize=ns)
    plt.setp(ax1.get_xticklabels(), visible=False) 
    plt.setp(ax_m.get_xticklabels(), visible=False) 
    plt.setp(ax_m.get_yticklabels(), fontsize=ns, alpha=0.0) 
    ax_m.set_ylim(ax1.get_ylim())
    ax_m.set_ylabel('Counts', fontsize=ns)

#make_sd_plots()
#plt.tight_layout()
#plt.subplots_adjust(hspace=0.05)
#plt.draw()
#plt.savefig("../SD_BestFit.eps", bbox_inches='tight')

plot_desc = {
  "cc_energy_ss" : ("Energy Projection (SS)", "Figure 4(a).  Best-fit results and data projected in energy. "),
  "cc_energy_ms" : ("Energy Projection (MS)", "Figure 4(b).  Best-fit results and data projected in energy. "),
}

def make_energy_plots():
    sf = 1.5
    fig = plt.figure(figsize=(sf*8.5,sf*7.5))

    gs = gridspec.GridSpec(8, 1)
    ax1 =   plt.subplot(gs[0:3,:])
    ax1_s = plt.subplot(gs[3,:])#, sharex=ax1)
    ax2 =   plt.subplot(gs[4:7,:])#, sharex=ax1)
    ax2_s = plt.subplot(gs[7,:])#, sharex=ax1)

    for a in [ax1_s, ax2_s]:
        a.yaxis.set_label_position("right")
        a.yaxis.tick_right()
    
    plt.setp(ax1_s.get_xticklabels(), visible=False) 
    plt.setp(ax2_s.get_xticklabels(), fontsize=ns) 

        
    label_text = [] 

    for key,ax,axs, ins,r,ym, ymax, l, first_ymax in [
                   ("cc_energy_ss", ax1, ax1_s, [0.55, 0.43, 0.42, 0.54], [2250, 2600], 0.0, 7, "(a) SS", 80000), 
                   ("cc_energy_ms", ax2, ax2_s, [0.55, 0.5, 0.42, 0.52], [2100, 2700], 0.0, 50, "(b) MS", 80000)
                        ]:
        it = obj[key]
        if "fit" not in it: continue 
        
        xr = copy.deepcopy(get_plot_sd_en(key))
        xr["ymax"] = first_ymax 
        x_title = xr["x_title"]
        if ax == ax1: 
            del xr["x_title"]
        scale_diff = convert_to_matplotlib(*it["fit"], axs=ax, integrate_all=True, **xr)
        pd = plot_desc[key]

        ax.set_ylabel("")
        if ax == ax1: 
            plt.tight_layout()
            for i in ax.get_yticklabels():
                if i.get_text() == '': label_text.append('')
                else: label_text.append(str(int(math.log10(i._y))))
            ax.set_yticklabels(label_text)
            plt.tight_layout()
        ax.set_yticklabels(label_text)


        ax_new = add_subplot_axes(ax,ins)
        if ax == ax2: 
            h_n, l_n = [], []
            hh, ll  = ax.get_legend_handles_labels()
            for order in plot_ordering:
                ind = ll.index(order)
                h_n.append(hh[ind])
                l_n.append(ll[ind])
            for h,j in zip(hh,ll):
                if j not in l_n: 
                  l_n.append(j)
                  h_n.append(h)

            lg = ax.legend(h_n, l_n, numpoints=1, ncol=2, loc="upper left",
               bbox_to_anchor=(0.15, 1.0), prop={'size' : 12.5})
            lg.draw_frame(False)
            ax.set_xlabel("")
        therange = xr["xr"]
        xr["xr"] = r 
        xr["ymax"] = ymax 
        xr["ymin"] = ym 
        xr["fs"] = 12 
        xr["x_title"] = x_title
        xr["yscale"] = "linear" 
        plt.setp(ax.get_xticklabels(), visible=False) 


        dat = it["fit"][1][0]
        convert_to_matplotlib(*it["fit"], axs=ax_new, **xr)
        if ax == ax1:
            for a in [ax, ax_new]:
                for e in [2379.00, 2529.19]:
                    a.plot([e,e], [1e-6, 1e15], '-', color='r') 

        for tick in ax_new.get_xaxis().get_major_ticks():
            tick.set_pad(8.)

        xr["ymax"] = 6
        xr["ymin"] = -6 
        xr["yscale"] = "linear" 
        xr["xr"] = therange 
        xr["plot_int_pt"] = False 
        c_color = (0.4 ,  0.85,  0.85)
        g_color = (0.3 ,  0.65,  0.3)
        axs.fill_between(therange, -2,-1, color=c_color)
        axs.fill_between(therange, 1,2, color=c_color)
        axs.fill_between(therange, -1,1, color=g_color)

        res = it["residuals"][1][0]
        yres, ydat = res.GetY(), dat.GetY() 
        xres, xdat = res.GetX(), dat.GetX() 
        
        indices_to_skip = [i for i in range(dat.GetN()) if (ydat[i] == 0.0 or xdat[i] >= integrate_above)]
        for i in reversed(indices_to_skip): res.RemovePoint(i) 
        n = res.GetN()
        res.SetPoint(n, sum(overflow_bin)/2., scale_diff)
        res.SetPointError(n, 0.0, 0.0, 0.0, 0.0) 
        convert_to_matplotlib(*it["residuals"], axs=axs, **xr)

        axs.set_ylabel(r"Resid. ($\sigma$)")
        yloc = plt.MaxNLocator(5)
        axs.yaxis.set_major_locator(yloc)
        if axs == ax2_s: axs.set_xlabel(xr["x_title"], fontsize=ns)
        else: axs.set_xlabel("")

        # Label big plots
        plt.text(0.05, 0.85, l, fontsize=ns,transform=ax.transAxes)
    
    plt.setp(ax2_s.get_xticklabels(), fontsize=ns) 
    for a in [ax1, ax2]:
        a.set_ylabel(r'log$_{10}$(Counts/14 keV)', fontsize=ns)
    perc_change = 0.4
    for a in [ax1_s, ax2_s]:
        for i in range(2,5):
            t = a.get_yaxis().majorTicks[i]
            t.set_pad((1 + perc_change)*t.get_pad())
        for i in range(2):
            t = a.get_yaxis().majorTicks[i]
            t.set_pad((1 - perc_change)*t.get_pad())
    

    

def main(af):
    global obj
    bn = os.path.basename(af)
    obj = pick.load(af)
    print("Make energy plots")
    make_energy_plots()
    plt.subplots_adjust(hspace=0.05)
    plt.draw()
    plt.savefig(bn + ".png", bbox_inches='tight')
    #plt.show()

for f in sys.argv[1:]:
    main(f)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
