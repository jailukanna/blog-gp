---
title: PIL example load fuze (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'load fuze'


Modules used in program: 
* `import PIL.Image`
* `import trimesh`
* `import numpy as np`
* `import pprint`

## python load fuze

Python PIL example: load fuze

```python
#!/usr/bin/env python

import pprint

import numpy as np
import trimesh
import PIL.Image

# trimesh.util.attach_to_log()

np.set_printoptions(precision=3, suppress=True)

scene = trimesh.Scene()

mesh = trimesh.load('./fuze.obj', preprocess=False)
scene.add_geometry(mesh)

transform = trimesh.transformations.rotation_matrix(np.deg2rad(90), [1, 0, 0])
transform = trimesh.transformations.translation_matrix([0, 0.3, 0.05]) @ transform
camera = trimesh.scene.Camera(
    resolution=(320, 240),
    fov=(60, 45),
)
scene.add_geometry(trimesh.creation.axis(origin_size=0.01), transform=transform)

r, c = np.meshgrid(np.arange(320), np.arange(240))
x = (r - camera.K[0][2]) / camera.fov[0]
y = (c - camera.K[1][2]) / camera.fov[1]
z = np.full(x.shape, 1, dtype=float)

xyz = np.dstack((x, y, z))

ray_dests = xyz.reshape(-1, 3)
ray_dests = trimesh.transformations.transform_points(ray_dests, matrix=transform)

ray_origin = np.array([0, 0.3, 0.05], dtype=float)
ray_directions = ray_dests - ray_origin
#
locations, index_ray, index_tri = mesh.ray.intersects_location(
    ray_origins=[ray_origin] * len(ray_directions),
    ray_directions=ray_directions,
    multiple_hits=False,
)

faces = mesh.faces[index_tri]
vs = mesh.vertices[faces]
vts = mesh.metadata['vertex_texture'][faces]
uv = []
for v_loc, v, vt in zip(locations, vs, vts):
    T = vt @ np.linalg.inv(v.T)
    vt_loc = T @ v_loc
    uv.append(vt_loc[[0, 1]])
uv = np.array(uv, dtype=float)

texture = PIL.Image.open('./fuze_uv.jpg')
colors = trimesh.visual.uv_to_color(uv, texture)

image = np.zeros((240, 320, 4), dtype=np.uint8)
c = c.reshape(-1)
r = r.reshape(-1)
image[c[index_ray], r[index_ray]] = colors

PIL.Image.fromarray(image).save('rendered.png')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
