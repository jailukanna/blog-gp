---
title: matplotlib example Test plot simultaneous (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Test plot simultaneous'


Modules used in program: 
* `import scipy.stats as stats`
* `import pandas as pd`
* `import numpy as np`
* `import warnings`

## python Test plot simultaneous

Python matplotlib example: Test plot simultaneous

```python
from statsmodels.compat.testing import skipif
from statsmodels.compat.python import BytesIO, asbytes, range

import warnings

import numpy as np
import pandas as pd
import scipy.stats as stats
from numpy.testing import assert_, assert_allclose, assert_almost_equal, assert_equal, \
    assert_raises

from statsmodels.stats.multicomp import (tukeyhsd, pairwise_tukeyhsd,
                                         MultiComparison)

try:
    import matplotlib.pyplot as plt  # makes plt available for test functions
    have_matplotlib = True
except ImportError:
    have_matplotlib = False

races =   ["asian","black","hispanic","other","white"]

# Generate random data
voter_race = np.random.choice(a= races,
                              p = [0.05, 0.15 ,0.25, 0.05, 0.5],
                              size=1000)

# Use a different distribution for white ages
white_ages = stats.poisson.rvs(loc=18, 
                              mu=32,
                              size=1000)

voter_age = stats.poisson.rvs(loc=18,
                              mu=30,
                              size=1000)

voter_age = np.where(voter_race=="white", white_ages, voter_age)

class CheckTuckeyPlotSimultaneous(object):

    @classmethod
    def setup_class_(cls):
        cls.endog = voter_age
        cls.groups = voter_race
        cls.alpha = 0.05
        cls.mc = MultiComparison(cls.endog, cls.groups)
        cls.res = cls.mc.tukeyhsd(alpha=cls.alpha)

    @skipif(not have_matplotlib, reason='matplotlib not available')
    def test_plot_simultaneous(self):
        result = pairwise_tukeyhsd(self.endog, self.groups, alpha=self.alpha)
        # Smoke test for figure
        fig = result.plot_simultaneous()
        assert_equal(isinstance(fig, plt.Figure), True)
        plt.close(fig)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
