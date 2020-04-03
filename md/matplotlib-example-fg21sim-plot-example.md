---
title: matplotlib example fg21sim-plot-example (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'fg21sim-plot-example'

Functions in program: 
* `def plot_fluxfunc(data, nbin=50, q=None, figsize=(6, 6), figfile=None):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`

## python fg21sim-plot-example

Python matplotlib example: fg21sim-plot-example

```python
import matplotlib as mpl
import matplotlib.pyplot as plt

# %matplotlib inline

# Matplotlib Customization
# References: https://matplotlib.org/users/customizing.html

# refresh the font cache:
#mpl.font_manager._rebuild()

# Reset to the defaults
#matplotlib.rcdefaults()

mpl.style.use("ggplot")
for k, v in [("font.family",       "Inconsolata"),
             ("font.size",         14.0),
             ("pdf.fonttype",      42),  # Type 42 (a.k.a. TrueType)
             ("figure.figsize",    [8, 6]),
             ("image.cmap",        "jet"),
             ("xtick.labelsize",   "large"),
             ("xtick.major.size",  7.0),
             ("xtick.major.width", 2.0),
             ("xtick.minor.size",  4.0),
             ("xtick.minor.width", 1.5),
             ("ytick.labelsize",   "large"),
             ("ytick.major.size",  7.0),
             ("ytick.major.width", 2.0),
             ("ytick.minor.size",  4.0),
             ("ytick.minor.width", 1.5)]:
    mpl.rcParams[k] = v


def plot_fluxfunc(data, nbin=50, q=None, figsize=(6, 6), figfile=None):
    fidx158 = data['fidx158']
    fidx1400 = data['fidx1400']
    halos_df = data['halos_df']
    halos = data['halos']
    area_asec2 = data['area_asec2']
    allskyfactor = data['allskyfactor']
    testdirs = data['testdirs']
    ntests = data['ntests']

    flux158        = [None]*ntests
    flux1400       = [None]*ntests
    power158       = [None]*ntests
    power1400      = [None]*ntests
    for i, df in enumerate(halos):
        power158[i] = np.array(df["power[%d]" % fidx158]) / 1e24  # [1e24 W/Hz]
        flux158[i] = 1e3 * np.array(df["flux[%d]" % fidx158])  # [mJy]
        power1400[i] = np.array(df["power[%d]" % fidx1400]) / 1e24  # [1e24 W/Hz]
        flux1400[i] = 1e3 * np.array(df["flux[%d]" % fidx1400])  # [mJy]

    Tb158 = [Fnu_to_Tb(flux.sum(), area_asec2, 158) for flux in flux158]  # [mK]
    Tb1400 = [Fnu_to_Tb(flux.sum(), area_asec2, 1400) for flux in flux1400]

    fig, ax = plt.subplots(figsize=figsize)

    # observed halos
    cobs1400, xobs1400, __ = countdist_integrated(obs2017["S1.4[mJy]"], nbin=20)
    ax.loglog(xobs1400, cobs1400, color="C2", ls="--", lw=2.5, label="Observations at 1400 MHz")

    try:
        qlow, qhigh = q
    except TypeError:
        qlow, qhigh = 0, 100

    vlow, vhigh = np.percentile(Tb158, q=(qlow, qhigh))
    idx_keep = [idx for idx, Tb in enumerate(Tb158) if (Tb>=vlow and Tb<=vhigh)]
    Tb158_ = [Tb for idx, Tb in enumerate(Tb158) if idx in idx_keep]
    Tb1400_ = [Tb for idx, Tb in enumerate(Tb1400) if idx in idx_keep]
    power158_ = [power for idx, power in enumerate(power158) if idx in idx_keep]
    power1400_ = [power for idx, power in enumerate(power1400) if idx in idx_keep]
    flux158_ = [flux for idx, flux in enumerate(flux158) if idx in idx_keep]
    flux1400_ = [flux for idx, flux in enumerate(flux1400) if idx in idx_keep]
    print("----------------------------------")
    print("q: %d-%d; #tests: %d" % (qlow, qhigh, len(flux158_)))
    print("Tb@158: %.3f +/- %.3f [mK]" % (np.mean(Tb158_), np.std(Tb158_)))
    print("Tb@158: %.3f (%.3f - %.3f) [mK]" % tuple(np.percentile(Tb158_, q=(50, 25, 75))))
    print("Tb@1400: %.5f +/- %.5f [mK]" % (np.mean(Tb1400_), np.std(Tb1400_)))
    print("Tb@1400: %.5f (%.5f - %.5f) [mK]" % tuple(np.percentile(Tb1400_, q=(50, 25, 75))))

    flux = np.concatenate(flux158_)
    f158min, f158max = flux.min(), flux.max()
    flux = np.concatenate(flux1400_)
    f1400min, f1400max = flux.min(), flux.max()
    ntests = len(flux158_)
    ff158  = np.zeros(shape=(ntests, nbin))
    ff1400 = np.zeros(shape=(ntests, nbin))
    pp158  = np.zeros(shape=(ntests, nbin))
    pp1400 = np.zeros(shape=(ntests, nbin))
    for i in range(ntests):
        # 1400 [MHz]
        flux = flux1400_[i]
        c1400, x1400, __ = countdist_integrated(flux, nbin=nbin, xmin=f1400min, xmax=f1400max)
        ff1400[i, :] = c1400
        #ax.loglog(x1400, c1400*allskyfactor, alpha=0.1, color="C0")
        # 158 [MHz]
        flux = flux158_[i]
        c158, x158, __ = countdist_integrated(flux, nbin=nbin, xmin=f158min, xmax=f158max)
        ff158[i, :] = c158
        #ax.loglog(x158, c158*allskyfactor, alpha=0.1, color="C1")

    # mean flux function
    ff1400mean = ff1400.mean(axis=0)
    ff158mean = ff158.mean(axis=0)
    ax.loglog(x1400, ff1400mean*allskyfactor, color="C2", ls="-", lw=2, label="Simulations at 1400 MHz")
    ax.loglog(x158,  ff158mean*allskyfactor, color="C1", ls="-", lw=2, label="Simulations at 158 MHz")

    # uncertainty ranges
    ff1400std = ff1400.std(axis=0)
    ff158std = ff158.std(axis=0)
    ax.fill_between(x1400, (ff1400mean-ff1400std)*allskyfactor,
                    (ff1400mean+ff1400std)*allskyfactor, alpha=0.3, color="C2")
    ax.fill_between(x158, (ff158mean-ff158std)*allskyfactor,
                    (ff158mean+ff158std)*allskyfactor, alpha=0.3, color="C1")

    ax.set(xscale="log", yscale="log",
           xlim=(1e-3, 3e3), ylim=(2, 1e5),
           xlabel="Flux Density [mJy]",
           ylabel="Integrated Counts (>flux)")
    ax.legend()

    if figfile:
        fig.savefig(figfile, dpi=150, bbox_inches='tight')
        print("saved figure to: %s" % figfile)
        figfile = path.splitext(figfile)[0] + '.pdf'
        fig.savefig(figfile, bbox_inches='tight')
        print("saved figure to: %s" % figfile)
    else:
        plt.show()


plot_fluxfunc(data, q=(2.5, 92.5), figsize=(8, 8), figfile='fluxfunc-simucomp-1400.png')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
