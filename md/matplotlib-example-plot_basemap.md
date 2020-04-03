---
title: matplotlib example plot basemap (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot basemap'

Functions in program: 
* `def np2pd(data,lat,lon,slat,slon,svar):`
* `def pd2np(DF,slat,slon,svar):`
* `def plot_streamlines(u,v,speed,lat,lon,clevs,units,title,ismask_continent,density_lines=2):`
* `def plot_wind(u,v,speed,lat,lon,clevs,units,title,ismask_continent):`
* `def plot_slpwind(slp,u,v,lat,lon,clevs,units,title,ismask_continent):`
* `def plot_data(data,lat,lon,clevs,units,title,ismask_continent):`

## python plot basemap

Python matplotlib example: plot basemap

```python
"""
Plot any meteorological variable on Geographical Map
"""
def plot_data(data,lat,lon,clevs,units,title,ismask_continent):
    from mpl_toolkits.basemap import Basemap
    import matplotlib.cm as cm
    import numpy as np
    import matplotlib.pyplot as plt
    import sys

    """
    PARAMETERS
    resolution: c, l, i, h, f
    area_thresh: 10000,1000,100,10,1 
    """
    resolution = 'h'
    details_resolution = 10000
    figsize = (12,12)
    parallels = np.arange(20.,40,1.)
    meridians = np.arange(110.,140.,1.)

    try:
        # create figure and axes instances
        fig = plt.figure(figsize=figsize)
        ax = fig.add_axes([0.1,0.1,0.8,0.8])

        # prepare geographical data
        latcorners = [np.min(lat),np.min(lat),np.max(lat),np.max(lat)]
        loncorners = [np.min(lon),np.max(lon),np.max(lon),np.min(lon)]
        lon_0 = np.mean(lon)
        lat_0 = np.mean(lat)


        # create Basemap instance.
        m = Basemap(projection='mill',lon_0=lon_0,lat_0=lat_0,\
                    llcrnrlat=latcorners[0],urcrnrlat=latcorners[-1],\
                    llcrnrlon=loncorners[0],urcrnrlon=loncorners[1],\
                    resolution=resolution,area_thresh=details_resolution)

        # draw coastlines, edge of map.
        m.drawcoastlines(linewidth=1.5)
        m.drawcountries()
        if ismask_continent: m.fillcontinents(color='grey',lake_color='aqua')

        # draw parallels.
        m.drawparallels(parallels,labels=[1,0,0,0],fontsize=10)
        # draw meridians
        m.drawmeridians(meridians,labels=[0,0,0,1],fontsize=10)

        # prepare data
        #data = data[::-1,:]
        ny = data.shape[0]; nx = data.shape[1]
        lons, lats = m.makegrid(nx, ny) # get lat/lons of ny by nx evenly space grid.
        x, y = m(lons, lats) # compute map proj coordinates.    

        # draw filled contours.
        cs = m.contourf(x,y,data,clevs,cmap=cm.jet)

        # add colorbar.
        cbar = m.colorbar(cs,location='bottom',pad="5%")
        cbar.set_label("%s"%(units),fontsize=12)
        # add title
        plt.title("%s"%(title),fontsize=12)

        # plot by screen
        plt.show()
        
    except Exception as e:
        print("ERROR: There are any problem plotting %s"%title)
        print(str(e))
        sys.stop("Stop!")
        
    return None



"""
Plot slp + wind (using arrows) on Geographical Map
"""
def plot_slpwind(slp,u,v,lat,lon,clevs,units,title,ismask_continent):
    from mpl_toolkits.basemap import Basemap, shiftgrid
    import matplotlib.cm as cm
    import numpy as np
    import matplotlib.pyplot as plt
    import sys

    """
    PARAMETERS:
    resolution: c, l, i, h, f
    area_thresh: 10000,1000,100,10,1 
    """
    resolution = 'h'
    details_resolution = 10000
    figsize = (12,12)
    parallels = np.arange(20.,40,1.)
    meridians = np.arange(110.,140.,1.)

    try:
        # create figure, add axes
        fig = plt.figure(figsize=figsize)
        ax = fig.add_axes([0.1,0.1,0.8,0.8])

        # prepare geographical data
        latcorners = [np.min(lat),np.min(lat),np.max(lat),np.max(lat)]
        loncorners = [np.min(lon),np.max(lon),np.max(lon),np.min(lon)]
        lon_0 = np.mean(lon)
        lat_0 = np.mean(lat)
        lons, lats = np.meshgrid(lon,lat)

        # create Basemap instance.
        m = Basemap(projection='mill',lon_0=lon_0,lat_0=lat_0,\
                    llcrnrlat=latcorners[0],urcrnrlat=latcorners[-1],\
                    llcrnrlon=loncorners[0],urcrnrlon=loncorners[1],\
                    resolution=resolution,area_thresh=details_resolution)

        # draw coastlines, edge of map.
        m.drawcoastlines(linewidth=1.5)
        m.drawcountries()
        if ismask_continent: m.fillcontinents(color='grey',lake_color='aqua')

        # draw parallels.
        m.drawparallels(parallels,labels=[1,0,0,0],fontsize=10)
        # draw meridians
        m.drawmeridians(meridians,labels=[0,0,0,1],fontsize=10)


        # compute native x,y coordinates of grid.
        x, y = m(lons, lats)

        # plot SLP contours.
        CS1 = m.contour(x,y,slp,clevs,linewidths=0.5,colors='k',animated=True)
        CS2 = m.contourf(x,y,slp,clevs,cmap=plt.cm.RdBu_r,animated=True)

        # filter points to be showed
        yy=np.arange(0,len(lat),filter_points)
        xx=np.arange(0,len(lon),filter_points)
        points=np.meshgrid(yy,xx)

        # plot wind arrows
        Q = m.quiver(lons[points],lats[points],u[points],v[points],latlon=True)

        # add colorbar
        cb = m.colorbar(CS2,"bottom", size="5%", pad="4%")
        cb.set_label(units,fontsize=12)

        # set plot title
        ax.set_title(title,fontsize=12)
        plt.show()
        
    except Exception as e:
        print("ERROR: There are any problem plotting %s"%title)
        print(str(e))
        sys.stop("Stop!")
        
    return None



"""
Plot Wind (using arrows) on Geographical Map
"""
def plot_wind(u,v,speed,lat,lon,clevs,units,title,ismask_continent):
    from mpl_toolkits.basemap import Basemap, shiftgrid
    import matplotlib.cm as cm
    import numpy as np
    import matplotlib.pyplot as plt

    """
    PARAMETERS:
    resolution: c, l, i, h, f
    area_thresh: 10000,1000,100,10,1 
    """
    resolution = 'h'
    details_resolution = 10000
    figsize = (12,12)
    parallels = np.arange(20.,40,1.)
    meridians = np.arange(110.,140.,1.)

    try:
        # create figure, add axes
        fig = plt.figure(figsize=figsize)
        ax = fig.add_axes([0.1,0.1,0.8,0.8])

        # prepare geographical data
        latcorners = [np.min(lat),np.min(lat),np.max(lat),np.max(lat)]
        loncorners = [np.min(lon),np.max(lon),np.max(lon),np.min(lon)]
        lon_0 = np.mean(lon)
        lat_0 = np.mean(lat)
        lons, lats = np.meshgrid(lon,lat)

        # create Basemap instance.
        m = Basemap(projection='mill',lon_0=lon_0,lat_0=lat_0,\
                    llcrnrlat=latcorners[0],urcrnrlat=latcorners[-1],\
                    llcrnrlon=loncorners[0],urcrnrlon=loncorners[1],\
                    resolution=resolution,area_thresh=details_resolution)

        # draw coastlines, edge of map.
        m.drawcoastlines(linewidth=1.5)
        m.drawcountries()
        if ismask_continent: m.fillcontinents(color='grey',lake_color='aqua')

        # draw parallels.
        m.drawparallels(parallels,labels=[1,0,0,0],fontsize=10)
        # draw meridians
        m.drawmeridians(meridians,labels=[0,0,0,1],fontsize=10)

        # filter points to be showed
        yy=np.arange(0,len(lat),filter_points)
        xx=np.arange(0,len(lon),filter_points)
        points=np.meshgrid(yy,xx)

        # plot wind arrows
        Q = m.quiver(lons[points],lats[points],u[points],v[points],speed[points],cmap=cm.jet ,latlon=True)

        # add colorbar
        cb = m.colorbar(Q,"bottom", size="5%", pad="4%")
        cb.set_label(units,fontsize=12)

        # set plot title
        ax.set_title(title,fontsize=12)
        plt.show()
        
    except Exception as e:
        print("ERROR: There are any problem plotting %s"%title)
        print(str(e))
        sys.stop("Stop!")
        
    return None


"""
Plot Wind (using streamlines) on Geographical Map
"""
def plot_streamlines(u,v,speed,lat,lon,clevs,units,title,ismask_continent,density_lines=2):
    from mpl_toolkits.basemap import Basemap, shiftgrid
    import matplotlib.cm as cm
    import numpy as np
    import matplotlib.pyplot as plt

    """
    PARAMETERS:
    resolution: c, l, i, h, f
    area_thresh: 10000,1000,100,10,1 
    """
    resolution = 'f'
    details_resolution = 0
    figsize = (16,16)
    parallels = np.arange(20.,40,1.)
    meridians = np.arange(110.,140.,1.)
    width_lines = 2

    try:
        # create figure, add axes
        fig = plt.figure(figsize=figsize)
        ax = fig.add_axes([0.1,0.1,0.8,0.8])

        # prepare geographical data
        latcorners = [np.min(lat),np.min(lat),np.max(lat),np.max(lat)]
        loncorners = [np.min(lon),np.max(lon),np.max(lon),np.min(lon)]
        lon_0 = np.mean(lon)
        lat_0 = np.mean(lat)
        lons, lats = np.meshgrid(lon,lat)


        # create Basemap instance.
        m = Basemap(projection='cyl',
                    llcrnrlat=latcorners[0],urcrnrlat=latcorners[-1],\
                    llcrnrlon=loncorners[0],urcrnrlon=loncorners[1],\
                    resolution=resolution,area_thresh=details_resolution)


        # draw coastlines, edge of map.
        m.drawcoastlines(linewidth=1.5)
        m.drawcountries()
        if ismask_continent: m.fillcontinents(color='grey',lake_color='aqua')

        # draw parallels.
        m.drawparallels(parallels,labels=[1,0,0,0],fontsize=10)
        # draw meridians
        m.drawmeridians(meridians,labels=[0,0,0,1],fontsize=10)

        # plot
        x, y = np.meshgrid(lon, lat)
        Q = m.streamplot(x,y,u,v,color=speed,linewidth=width_lines,density=density_lines,cmap=plt.cm.spectral)
        # add colorbar
        cb = m.colorbar(location="bottom", size="5%", pad="4%")
        cb.set_label(units,fontsize=12)

        # set plot title
        ax.set_title(title,fontsize=22)
        plt.show()

    except Exception as e:
        print("ERROR: There are any problem plotting %s"%title)
        print(str(e))
        sys.stop("Stop!")
        
    return None



"""
Lat/Lon/Variable Pandas Dataframe to numpy arrays: lat inverse(1D), lon(1D), variable (2D)
"""
def pd2np(DF,slat,slon,svar):
    # get arrays of lat inverse / lon
    lat = np.array(sorted(set(DF[slat].tolist()),reverse=True))
    lon = np.array(sorted(set(DF[slon].tolist()),reverse=False))
    # get array matrix of data (lat inverse)
    data = np.reshape(DF[svar].as_matrix(),(len(lat),len(lon)))
    # return 
    return [data,lat,lon]

"""
Numpy arrays: lat inverse(1D), lon(1D), variable (2D) to Lat/Lon/Variable Pandas Dataframe
"""
def np2pd(data,lat,lon,slat,slon,svar):
    import pandas as pd
    import numpy as np

    # inverse sort latitude
    lat = np.sort(lat)[::-1]
    # create numpy arrays
    fdata = np.reshape(data,(len(lat)*len(lon),))
    flat = np.repeat(lat,len(lon))
    flon = np.reshape(np.repeat(np.reshape(lon, (1,len(lon))),len(lat),axis=0),(len(lon)*len(lat),))
    # return dataframe
    return pd.DataFrame({slat:flat,slon:flon,svar:fdata})


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
