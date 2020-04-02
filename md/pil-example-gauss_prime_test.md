---
title: pil example gauss prime test (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'gauss prime test'

Functions in program: 
* `def main(size=None, minsize=32, maxsize=512, count=8, seed=None):`
* `def hide_number(value):`
* `def plot_prime_map(primes=None):`
* `def print_prime_map(primes=None):`
* `def build_fake_gaussian_primes_from_noise(dim):`
* `def build_fake_gaussian_primes_from_fake_primes(dim):`
* `def build_gaussian_primes(dim, natural_primes=None):`
* `def print_natural_primes(prime_table=None):`
* `def build_fake_natural_primes(limit):`
* `def build_natural_primes(limit):`

Modules used in program: 
* `import PIL.ImageDraw`
* `import PIL.Image`
* `import time`
* `import sys`
* `import random`
* `import math`
* `import gzip`
* `import base64`

## python gauss prime test

Python pil example: gauss prime test

```python
#! /usr/bin/env python3

import base64
import gzip
import math
import random
import sys
import time

# This requires Pillow, a fork of PIL.  Please uninstall PIL if you have
# it, and use "pip install pillow".
import PIL.Image
import PIL.ImageDraw

def build_natural_primes(limit):
    """Build a lookup table of the primes up to LIMIT.

build_natural_primes(1000)[n] is True iff n is prime."""
    rv = [True] * limit
    rv[0] = False
    rv[1] = False
    for n in range(2, limit):
        if rv[n]:
            for nc in range(2 * n, limit, n):
                rv[nc] = False
    return rv

#PRIME_PI = functools.reduce(
#    lambda accumulator, is_prime: 
#    accumulator + [accumulator[-1] + (1 if is_prime else 0)],
#    gaussprimetest.build_natural_primes(100)[1:],
#    [0])

def build_fake_natural_primes(limit):
    """Build a random table with the same density as the natural primes.

In other words, this has a similar distribution as the prime numbers, but
has random."""
    rv = [False] * limit
    for n in range(2, limit):
        logn = math.log(n)
        # This is d/dx (x/lnx).
        prime_density = (logn - 1) / (logn * logn)
        # In the region we care about (<10000), the x/lnx approximation
        # has an approximate error of 1.2; https://wolfr.am/vtEQmVDV
        # However, near the center it's a lot different, and that makes
        # a pretty clear artifact, so use a different factor there.
        corrected_prime_density = prime_density * (0.8 if n < 10 else 1.2)
        if random.random() < corrected_prime_density:
            rv[n] = True
    return rv

def print_natural_primes(prime_table=None):
    """Print the primes in the table, to test build_natural_primes."""
    if prime_table is None:
        prime_table = build_natural_primes(2048)
    for n in range(len(prime_table)):
        if prime_table[n]:
            print(n)

def build_gaussian_primes(dim, natural_primes=None):
    """Build a table of the Gaussian primes (first quadrant only).

The return value is a DIM x DIM array.  The entry for 
build_gaussian_primes(1000)[a][b] is True iff a+bi is a Gaussian prime.

This is based on the natural primes.  If you want to build fake
output, pass in an alternate version of natural_primes, in the format
of by build_natural_primes or build_fake_natural_primes.
    """
    # This is based on the algorithm suggested by
    # http://mathworld.wolfram.com/GaussianPrime.html.
    if natural_primes is None:
        natural_primes = build_natural_primes(2 * dim * dim)
    rv = [[False] * dim for _ in range(dim)]
    for r in range(1, dim):
        for i in range(1, dim):
            p = r**2 + i**2
            if natural_primes[p]:
                rv[r][i] = True
    for r in range(1, dim):
        if natural_primes[r] and (r % 4) == 3:
            rv[r][0] = True
            rv[0][r] = True
    return rv

def build_fake_gaussian_primes_from_fake_primes(dim):
    """Build a fake version of the Gaussian primes.

This uses the same algorithm as we use in build_gaussian_primes, but
bases it on a fake table of natural primes."""
    fake_primes = build_fake_natural_primes(2 * dim * dim)
    return build_gaussian_primes(dim, fake_primes)

def build_fake_gaussian_primes_from_noise(dim):
    """Build a fake version of the Gaussian primes that's just white noise."""
    rv = [[False] * dim for _ in range(dim)]
    density = .1
    for r in range(dim):
        for i in range(r):
            is_prime = random.random() < density
            rv[r][i] = is_prime
            rv[i][r] = is_prime
    return rv

def print_prime_map(primes=None):
    """Print the output of build_gaussian_primes in ASCII."""
    if primes is None:
        primes = build_gaussian_primes(32)
    dim = len(primes)
    for r in range(-dim + 1, dim):
        for i in range(-dim + 1, dim):
            if primes[abs(r)][abs(i)]:
                print("*", end='')
            else:
                print(" ", end='')
        print()

def plot_prime_map(primes=None):
    """Plot the output of build_gaussian_primes.  Returns a PIL Image."""
    if primes is None:
        primes=build_gaussian_primes(256)
    dim = len(primes)
    rv = PIL.Image.new("RGB", (dim*2-1, dim*2-1), (255,0,0))
    draw = PIL.ImageDraw.Draw(rv)
    offset = dim - 1
    for r in range(dim):
        for i in range(dim):
            if primes[r][i]:
                color = (255,255,255)
            else:
                color = (0,0,0)
            draw.point([(-r + offset, -i + offset),
                        ( r + offset, -i + offset),
                        (-r + offset,  i + offset),
                        ( r + offset,  i + offset)],
                       color)
    return rv

def hide_number(value):
    """Return a shell command that will print(the given value.)

The command will avoid making it trivial to know the value without a
computer's help (or manual intervention).
    """
    # (A reasonable alternative would be to pass the input through
    # "openssl bf -a -k .".)
    #
    # To obfuscate the output, we pass it through a compressor, primed
    # with random data.  We want our actual output (which will be at
    # the end of the compressed stream) to be a dictionary lookup, so
    # it needs to be multiple bytes.  So, we prepend a digit to our
    # final output to make it multiple bytes, and include that
    # somewhere in the priming stream.  We also have the random data
    # to prime the Huffman stage before we produce the 
    prefix = str(random.randrange(10))
    outstr = prefix + str(value)
    # A nonce length of 10 normally keeps the final command under 80
    # chars for single-digit inputs.
    nonce = [str(random.randrange(10)) for _ in range(11)] + [outstr]
    random.shuffle(nonce)
    padding = "".join(nonce)
    zipped = gzip.compress((padding + outstr + "\n").encode("ascii"))
    enc = base64.b64encode(zipped)
    cmd = "".join(["echo ", enc.decode("ascii"),
                   "|base64 -d|zcat|tail -c" + str(len(str(value)) + 1)])
    return cmd

def main(size=None, minsize=32, maxsize=512, count=8, seed=None):
    """Make a test to see if the real Gaussian primes have a pattern.

This will save eight PNG files; one of these has the actual plot of
Gaussian primes, but the others are fakes.  It will print(a command)
to display these using ImageMagick (use the space bar to loop among
them), and a command to reveal which one is real.
"""
    random.seed(seed)
    if size is None:
        size = random.randrange(minsize, maxsize)
    real_picture = random.randrange(count)
    for i in range(count):
        if i == real_picture:
            builder = build_gaussian_primes
        elif random.randrange(2) == 0:
            #builder = build_fake_gaussian_primes_from_fake_primes
            builder = build_fake_gaussian_primes_from_noise
        else:
            builder = build_fake_gaussian_primes_from_noise
        prime_plot = plot_prime_map(builder(size))
        prime_plot.save("gp%i.png" % i)
    print("display", *["gp%i.png" % i for i in range(count)])
    print(hide_number(real_picture))
    return 0

if __name__ == "__main__":
    sys.exit(main())

# Some test commands:
#print_natural_primes()
#
# These should be about equal:
#import statistics
#gaussprimetest.build_natural_primes(1024).count(True)
#statistics.mean([gaussprimetest.build_fake_natural_primes(1024).count(True) for _ in range(100)])
#
#print_prime_map()
#print_prime_map(build_fake_gaussian_primes_from_fake_primes(32))
#
#plot_prime_map().show()
#plot_prime_map(build_fake_gaussian_primes_from_fake_primes(256)).show()
#
# These should be about equal:
# (The first one takes several seconds:)
#functools.reduce(lambda accum, l: accum + l.count(True), gaussprimetest.build_gaussian_primes(1024), 0)
#statistics.mean([functools.reduce(lambda accum, l: accum + l.count(True), gaussprimetest.build_fake_gaussian_primes_from_noise(1024), 0) for _ in range(5)])
#statistics.mean([functools.reduce(lambda accum, l: accum + l.count(True), gaussprimetest.build_fake_gaussian_primes_from_fake_primes(1024), 0) for _ in range(5)])b


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
