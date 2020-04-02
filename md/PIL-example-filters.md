---
title: PIL example filters (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'filters'

Functions in program: 
* `def process(image_name, transformation_class_map, queue):`
* `def transform_queue(image, queue, bijection):`
* `def threshold(n):`
* `def exp_map(n):`
* `def log_map(n):`
* `def quadratic_map(n):`
* `def sine_map(n):`
* `def prime_factor_dist_convolve(scalar_list):`
* `def counter_to_coordinate(d):`
* `def primes(n):`
* `def mean(scalar_list):`
* `def median(scalar_list):`
* `def collatz_map(n, minimum = 0, maximum = 255):`
* `def collatz(n):`

Modules used in program: 
* `import time, sys, random, math, functools, collections`
* `import PIL.Image, numpy, scipy.misc, scipy.ndimage, scipy.spatial`

## python filters

Python PIL example: filters

```python
# much (some) of this code isn't mine
import PIL.Image, numpy, scipy.misc, scipy.ndimage, scipy.spatial
import time, sys, random, math, functools, collections

# image = PIL.Image.open("4.2.06.tiff") # sailboat on lake
# image = PIL.Image.open("lena.tiff") # lena
pixel_definition = ["r", "g", "b"] # definition of pixel

# neighborhood_kernel = numpy.array([[1, 1, 1],
#    					             [1, 1, 1],
#    								 [1, 1, 1]])
# 
neighborhood_kernel = numpy.ones((5,5))

self_kernel = numpy.array([[1]])

# some example transformations

def collatz(n):
	if n == 0: return 0
	i = 0
	while n != 1:
		i += 1
		if n % 2 == 0:
			n = n // 2
		else:
			n = 3 * n + 1
	return i

collatz_list = [min(collatz(n), 255) for n in range(256)]

def collatz_map(n, minimum = 0, maximum = 255):
	return collatz_list[int(n[0])]

def median(scalar_list):
	return sorted(scalar_list)[len(scalar_list) // 2]

def mean(scalar_list):
	return sum(scalar_list) // len(scalar_list)

def primes(n):
    primfac = []
    d = 2
    while d * d <= n:
        while (n % d) == 0:
            primfac.append(d)  # supposing you want multiple factors repeated
            n /= d
        d += 1
    if n > 1:
       primfac.append(n)
    return list(map(int, primfac))

def counter_to_coordinate(d):
	# d might look like e.g. n = 12, d = {2:2 , 3: 1}
	dimension = max(some_primes)
	coordinate = [0] * (dimension + 1)
	for index, (prime, times) in enumerate(zip(d, d.values())):
		coordinate[prime] = times
	return coordinate

prime_factors = [primes(n) for n in range(256)] # e.g. prime_factors[12] = [2, 2, 3]
some_primes = list(set.union(*list(map(set, prime_factors))))
coordinate = lambda n: counter_to_coordinate(dict(collections.Counter(prime_factors[n])))
cache = {i: numpy.array(coordinate(i)) for i in range(len(prime_factors))}
nums = numpy.array(list(cache.values()))
grid = scipy.spatial.distance.cdist(nums, nums, 'cityblock')

def prime_factor_dist_convolve(scalar_list):
	middle = scalar_list[len(scalar_list)//2]
	prime_space_distances = numpy.array([grid[middle][x] for x in scalar_list])
	norm = numpy.linalg.norm(prime_space_distances)
	if norm == 0: return middle
	normalized_distances = prime_space_distances / norm
	products = numpy.array([normalized_distances[i] * scalar_list[i] for i in range(len(scalar_list))])
	return sum(products)

sine_cache = [255 * math.sin((n) * (math.pi / 2) / 255) for n in range(256)]
def sine_map(n):
	return sine_cache[int(n[0])]

quadratic_cache = [255 * (n / 255) ** 2 for n in range(256)]
def quadratic_map(n):
	return quadratic_cache[int(n[0])]

log_cache = [0] + [math.log(n) for n in range(1, 256)]
def log_map(n):
	return log_cache[int(n[0])]

exp_cache = [(255/148) * math.e ** n for n in range(256)]
def exp_map(n):
	return exp_cache[int(n[0])]

def threshold(n):
	return 0 if 128 < n else 255

pixel_transformations = functools.partial(scipy.ndimage.generic_filter, footprint(= self_kernel))
window_transformations = functools.partial(scipy.ndimage.generic_filter, footprint(= neighborhood_kernel))

def transform_queue(image, queue, bijection):
	pixel_system = numpy.array(image) # each pixel_system[y, x] = 3vector <R, G, B>
	RGBimages = [pixel_system[:,:,index] for index, color in enumerate(pixel_definition)] # split into RGB images
	
	# guarantee every transformation has a partial type class
	transformed_images = [] # will contain transformed RGB images
	for image in RGBimages:
		for transformation in queue:
			transformation_partial = list(filter(lambda key: transformation in bijection[key], bijection.keys()))[0] # each transformation maps to one partial; filter away irrelevant partials
			image = transformation_partial(image, transformation)
		transformed_images.append(image)
	return numpy.uint8(numpy.dstack(transformed_images)) # make one RGB image from the R, G, B planes

def process(image_name, transformation_class_map, queue):
	image = PIL.Image.open(image_name)
	result = transform_queue(image, queue, transformation_class_map)
	image = PIL.Image.fromarray(result)
	image.show()
	image.save(''.join(list(map(lambda f: f.__name__ + '_', queue))) + str(time.time()) + '_' + image_name)

if __name__ == '__main__':
	transformation_class_map = {pixel_transformations: set([collatz_map,
							  	   						    sine_map,
							  	   						    quadratic_map,
							  	   						    log_map, 
							  	   						    exp_map,
							  	   						    threshold]),
  
							    window_transformations: set([median,
							  							     mean,
														     random.choice,
														     prime_factor_dist_convolve])}
	queue = [sine_map] * 3 + [prime_factor_dist_convolve]
	process(sys.argv[1], transformation_class_map, queue)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
