---
title: matplotlib example hull black scholes (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'hull black scholes'

Functions in program: 
* `def MonteCarloEuropeanCallPrice(S0, K, r, sigma, T, N=100, pathCount=5000):`
* `def BinomialTreeEuropeanCallPrice(S0, K, r, sigma, T, N=30):`
* `def BlackScholesEuropeanPutPrice(S0, K, r, sigma, T):`
* `def BlackScholesEuropeanCallPrice(S0, K, r, sigma, T):`
* `def d2(S0, K, r, sigma, T):`
* `def d1(S0, K, r, sigma, T):`

Modules used in program: 
* `import scipy.stats`
* `import matplotlib.pyplot`
* `import numpy`
* `import math`

## python hull black scholes

Python matplotlib example: hull black scholes

```python
import math
import numpy
import matplotlib.pyplot
import scipy.stats

# See [Hull], Chapter 13. The pricing formulae are given in Section 13.8

def d1(S0, K, r, sigma, T):
   return (math.log(S0 / K) + (r + sigma**2 / 2) * T) / (sigma * math.sqrt(T))

def d2(S0, K, r, sigma, T):
   return (math.log(S0 / K) + (r - sigma**2 / 2) * T) / (sigma * math.sqrt(T))

def BlackScholesEuropeanCallPrice(S0, K, r, sigma, T):
   return S0 * scipy.stats.norm.cdf(d1(S0, K, r, sigma, T)) - K * math.exp(-r * T) * scipy.stats.norm.cdf(d2(S0, K, r, sigma, T))

def BlackScholesEuropeanPutPrice(S0, K, r, sigma, T):
   return K * math.exp(-r * T) * scipy.stats.norm.cdf(-d2(S0, K, r, sigma, T)) - S0 * scipy.stats.norm.cdf(-d1(S0, K, r, sigma, T))

S0 = 100.0
K = 110.0
r = 0.03
sigma = 0.1
T = 0.5

print("S0\tstock price at time 0:", S0)
print("K\tstrike price:", K)
print("r\tcontinuously compounded risk-free rate:", r)
print("sigma\tvolatility of the stock price per year:", sigma)
print("T\ttime to maturity in trading years:", T)
# This is usually Number of trading days until option maturity / 252, see
# [Hull], Section 13.4

c_BS = BlackScholesEuropeanCallPrice(S0, K, r, sigma, T)
p_BS = BlackScholesEuropeanPutPrice(S0, K, r, sigma, T)

print
print("c_BS\tBlack-Scholes European call price:", c_BS)
print("p_BS\tBlack-Scholes European put price:", p_BS)

"""
S0s = numpy.arange(90.0, 120.0, 0.5)
cs_1D = [BlackScholesEuropeanCallPrice(S0, K, r, sigma, T=1.0 / 252.0) for S0 in S0s]
cs_3M = [BlackScholesEuropeanCallPrice(S0, K, r, sigma, T=0.25) for S0 in S0s]
cs_6M = [BlackScholesEuropeanCallPrice(S0, K, r, sigma, T=0.5) for S0 in S0s]
cs_1Y = [BlackScholesEuropeanCallPrice(S0, K, r, sigma, T=1.0) for S0 in S0s]

matplotlib.pyplot.plot(S0s, cs_1D, 'r-', lw=2)
matplotlib.pyplot.plot(S0s, cs_3M, 'y-', lw=2)
matplotlib.pyplot.plot(S0s, cs_6M, 'b-', lw=2)
matplotlib.pyplot.plot(S0s, cs_1Y, 'g-', lw=2)
matplotlib.pyplot.xlabel("$S_0$")
matplotlib.pyplot.ylabel("$c$")
matplotlib.pyplot.title("European call price versus $S_0$, $T = \\frac{1}{252}, \\frac{1}{4}, \\frac{1}{2}, 1$")
matplotlib.pyplot.grid(True)
matplotlib.pyplot.show()
"""

def BinomialTreeEuropeanCallPrice(S0, K, r, sigma, T, N=30):
   deltaT = T / N

   u = math.exp(sigma * math.sqrt(deltaT))
   d = 1.0 / u

   # Initialise our f_{i,j} tree with zeros
   fs = [[0.0 for j in xrange(i + 1)] for i in xrange(N + 1)]

   a = math.exp(r * deltaT)

   p = (a - d) / (u - d)
   oneMinusP = 1.0 - p

   # Compute the leaves, f_{N, j}
   for j in xrange(i+1):
       fs[N][j] = max(S0 * u**j * d**(N - j) - K, 0.0)

   for i in xrange(N-1, -1, -1):
       for j in xrange(i + 1):
           fs[i][j] = math.exp(-r * deltaT) * (p * fs[i + 1][j + 1] + oneMinusP * fs[i + 1][j])

   # print(fs)

   return fs[0][0]

c_BT = BinomialTreeEuropeanCallPrice(S0, K, r, sigma, T)
print("c_BT\tBinomial tree European call price:", c_BT)

def MonteCarloEuropeanCallPrice(S0, K, r, sigma, T, N=100, pathCount=5000):
   deltaT = T / N

   fs = [0.0 for i in xrange(pathCount)]

   for i in xrange(pathCount):
       S = S0
       epsilons = scipy.stats.norm.rvs(size=N)
       for j in xrange(N):
           S += r * S * deltaT + sigma * S * epsilons[j] * math.sqrt(deltaT)
       fs[i] = max(S - K, 0.0)

   return scipy.mean(fs)

c_MC = MonteCarloEuropeanCallPrice(S0, K, r, sigma, T)
print("c_MC\tMonte Carlo European call price:", c_MC)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
