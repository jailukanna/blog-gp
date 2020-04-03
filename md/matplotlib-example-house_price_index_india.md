---
title: matplotlib example house price index india (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'house price index india'


## python house price index india

Python matplotlib example: house price index india

```python
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "collapsed": false,
    "scrolled": true
   },
   "outputs": [],
   "source": [
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Housing price index of india for 06-2011 to 09-2013 "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "collapsed": false,
    "scrolled": true
   },
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "\n",
    "df = pd.DataFrame.from_csv('./housing_price_index_2010-11_100.csv')\n",
    "#print(df)\n",
    "df_transposed = df.T\n",
    "df_transposed.plot(kind='bar')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# % Change chart for Housing price index of india for 06-2011 to 09-2013 (%change from the last reported value)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "collapsed": false,
    "scrolled": true
   },
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "import matplotlib\n",
    "plt.style.use('fivethirtyeight')\n",
    "df = pd.DataFrame.from_csv('./housing_price_index_2010-11_100.csv')\n",
    "df_transposed = df.T\n",
    "df_transposed=df_transposed.pct_change()\n",
    "# print(df_transposed)\n",
    "df_transposed.plot(kind='bar')\n",
    "plt.legend(loc='upper center', bbox_to_anchor=(0.5,1.15),ncol=3,shadow=True)\n",
    "mng = plt.get_current_fig_manager()\n",
    "#mng.frame.Maximize(True)\n",
    "mng.window.state('zoomed')\n",
    "plt.show()\n",
    "#matplotlib.get_backend()\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Traditional % Change chart for Housing price index of india for 06-2011 to 09-2013 "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "collapsed": false,
    "scrolled": true
   },
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "import matplotlib\n",
    "import numpy as np\n",
    "plt.style.use('fivethirtyeight')\n",
    "df = pd.DataFrame.from_csv('./housing_price_index_2010-11_100.csv')\n",
    "df_transposed = df.T\n",
    "df_transposed = df_transposed.drop('All India', 1)\n",
    "# df_transposed.rename(columns={'All India':'All_india'},inplace=True)\n",
    "print(df_transposed)\n",
    "# print(df_transposed.ix[:,0])\n",
    "\n",
    "for column in df_transposed:\n",
    "    df_transposed[column] =(df_transposed[column] - df_transposed[column][0]) / df_transposed[column][0] * 100 \n",
    "   \n",
    "df_transposed.plot()\n",
    "plt.legend(loc='upper center',fancybox=True, bbox_to_anchor=(0.5,1.15),ncol=3,shadow=True)\n",
    "mng = plt.get_current_fig_manager()\n",
    "mng.window.state('zoomed')\n",
    "plt.show()\n",
    "#matplotlib.get_backend()\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Traditional percentage chart with Benchmarking( with All india )"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "collapsed": false
   },
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "import matplotlib\n",
    "import numpy as np\n",
    "plt.style.use('fivethirtyeight')\n",
    "df = pd.DataFrame.from_csv('./housing_price_index_2010-11_100.csv')\n",
    "df_transposed = df.T\n",
    "ax1 = plt.subplot2grid((1,1), (0,0))\n",
    "df_transposed.rename(columns={'All India':'All_india'},inplace=True)\n",
    "for column in df_transposed:\n",
    "    df_transposed[column] =(df_transposed[column] - df_transposed[column][0]) / df_transposed[column][0] * 100 \n",
    "    \n",
    "df_transposed.plot(ax=ax1)\n",
    "df_transposed['All_india'].plot(ax=ax1,color='k',linewidth=10)\n",
    "# barlist[0].set_color('k')\n",
    "\n",
    "plt.legend(loc='upper center',fancybox=True, bbox_to_anchor=(0.5,1.15),ncol=3,shadow=True)\n",
    "mng = plt.get_current_fig_manager()\n",
    "mng.window.state('zoomed')\n",
    "plt.show()\n",
    "\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# correlation table and other statitics"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "collapsed": false
   },
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "ax1 = plt.subplot2grid((1,1), (0,0))\n",
    "df = pd.DataFrame.from_csv('./housing_price_index_2010-11_100.csv')\n",
    "#print(df)\n",
    "df_transposed = df.T\n",
    "print(df_transposed.corr())\n",
    "print(df_transposed.corr().describe())\n",
    "df_transposed.plot(kind='bar',ax=ax1)\n",
    "mng = plt.get_current_fig_manager()\n",
    "mng.window.state('zoomed')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {
    "collapsed": true
   },
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "anaconda-cloud": {},
  "kernelspec": {
   "display_name": "Python [Root]",
   "language": "python",
   "name": "Python [Root]"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.5.2"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 0
}


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
