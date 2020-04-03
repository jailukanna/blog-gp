---
title: matplotlib example snowfall (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'snowfall'


Modules used in program: 
* `import matplotlib.patches as mpatches`
* `import matplotlib.path as mpath`
* `import matplotlib.animation as animation`
* `import matplotlib.patheffects as path_effects`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib`

## python snowfall

Python matplotlib example: snowfall

```python
import matplotlib
import matplotlib.pyplot as plt
import numpy as np
import matplotlib.patheffects as path_effects
import matplotlib.animation as animation
import matplotlib.path as mpath
import matplotlib.patches as mpatches
from matplotlib.transforms import BboxTransform, Bbox


class SeasonsGreetings(object):
    def __init__(self, axes, message, n_flakes=1200, dt=1/25.):
        self.ax = axes
        self._message = message

        self._n_flakes = n_flakes
        self._full_descent = 1.0001
        self._low_cutoff = -0.0001
        self._x_accn_max = 0.55
        self._dt_factor = (dt ** 2) * 0.15
        self.setup_scene()

    def setup_scene(self):
        self.ax.format_coord = lambda *args, **kwargs: "'Tis the season to be jolly, Fa la la la la, la la la la."
        self.create_flakes()
        self.create_message()
        self.create_holly()
        self.create_groundcover()
        fig.canvas.mpl_connect('resize_event', self.update_holy_position)

    def create_flakes(self):
        """Create the flake data, and initialise random flake locations."""
        flake_dtype = [('x', np.float), ('y', np.float), ('x_orig', np.float),
                       ('x_velocity', np.float), ('mass', np.float)]
        # Initialise the snow randomly.
        n_flakes = self._n_flakes
        flakes = np.empty(n_flakes, dtype=flake_dtype).view(np.recarray)
        flakes.x = flakes.x_orig = np.random.uniform(-0.05, 1.05, n_flakes)
        flakes.y = np.zeros(n_flakes) + np.random.uniform(self._low_cutoff, self._low_cutoff + self._full_descent, n_flakes)
        flakes.x_velocity = np.random.normal(0.3, self._x_accn_max, n_flakes)
        flakes.mass = np.clip(np.abs(np.random.normal(0, 0.12, n_flakes)) + 0.55, 0, 1)
        self.flake_data = flakes

        colors = np.ones((flakes.x.size, 4), dtype=np.float)
        colors[:, 3] = flakes.mass ** 2
        # Draw the flakes for the first time. The scatter will be updated in the animate loop.
        self.flakes = self.ax.scatter(flakes.x, flakes.y, s=flakes.mass**2 * 30, marker='*',
                                      c=colors, edgecolor='none', transform=self.ax.get_figure().transFigure)

    def advance_flakes(self):
        """Update self.flake_data by one timestep."""
        flakes = self.flake_data
        # Move the flakes according to their mass and horizontal acceleration.
        # This is not real physics - it just looks good.
        flakes.y -= 9.8 * self._dt_factor * (flakes.mass + 0.4) ** 2
        flakes.x -= flakes.x_velocity * self._dt_factor * (flakes.mass + 0.4) ** 2

        restart = flakes.y < self._low_cutoff
        flakes.y[restart] = self._low_cutoff + self._full_descent
        flakes.x[restart] = flakes.x_orig[restart]
        flakes.x_velocity[restart] = np.random.normal(0.3, self._x_accn_max, np.count_nonzero(restart))
        return flakes

    def create_message(self):
        fig = self.ax.get_figure()
        t1 = fig.text(0.5, 0.5, self._message, fontsize=50, weight=1000, ha='center', va='center')
        wavy_text = path_effects.PathPatchEffect(offset=(0, 1), facecolor='white', edgecolor='white', linewidth=1.5)
        wavy_text.patch.set_sketch_params(scale=2, length=30, randomness=1)
        effects = [wavy_text, path_effects.PathPatchEffect(edgecolor='white', linewidth=1.1,
                                                           facecolor='black')]
        t1.set_path_effects(effects)
        self.text = t1

    def create_holly(self):
        # Holly and berries using cubic Bezier curves, adapted from www.createcricutdesigns.com
        holly_verts = np.array([[334, 317, 291, 260, 264, 285, 251, 266, 278, 288, 281, 277, 277, 277, 277, 278, 260, 246, 234, 209, 203, 127, 150, 122, 100, 156, 178, 190, 210, 239, 299, 297, 299, 303, 305, 307, 310, 316, 322, 327, 327, 320, 311, 345, 376, 396, 437, 473, 508, 478, 481, 495, 469, 441, 412, 401, 387, 373, 373, 373, 373, 373, 366, 356, 369, 382, 397, 381, 375, 405, 363, 347, 334, 0, 338, 339, 339, 307, 324, 323, 306, 338, 338, 0, 356, 419, 455, 455, 454, 454, 418, 356, 356, 0, 282, 282, 282, 282, 267, 198, 153, 152, 198, 267, 282, 0],
                                [92, 145, 164, 172, 203, 221, 289, 288, 293, 300, 305, 313, 321, 324, 326, 329, 324, 314, 302, 320, 348, 350, 388, 418, 450, 441, 458, 482, 454, 431, 432, 416, 401, 388, 388, 388, 388, 388, 386, 382, 398, 410, 421, 430, 448, 505, 484, 494, 506, 459, 427, 398, 380, 385, 315, 330, 336, 337, 336, 335, 335, 324, 314, 310, 300, 297, 298, 266, 230, 178, 160, 127, 92, 0, 157, 158, 158, 233, 300, 300, 233, 157, 157, 0, 363, 394, 467, 467, 468, 468, 395, 364, 363, 0, 364, 365, 365, 365, 367, 378, 420, 419, 377, 366, 364, 0]]).T
        holly_codes = np.array([1, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 79, 1, 2, 4, 4, 4, 2, 4, 4, 4, 79, 1, 4, 4, 4, 2, 4, 4, 4, 2, 79, 1, 4, 4, 4, 4, 4, 4, 2, 4, 4, 4, 79])
        berries_verts = [np.array([[315, 300, 287, 287, 287, 300, 315, 331, 343, 343, 343, 331, 315, 0],
                                  [339, 339, 351, 366, 381, 393, 393, 393, 381, 366, 351, 339, 339, 0]]).T,
                         np.array([[300, 285, 274, 274, 274, 285, 300, 314, 325, 325, 325, 314, 300, 0],
                                  [297, 297, 308, 322, 336, 347, 347, 347, 336, 322, 308, 297, 297, 0]]).T,
                         np.array([[348, 334, 322, 322, 322, 334, 348, 362, 374, 374, 374, 362, 348, 0],
                                  [306, 306, 318, 331, 345, 356, 356, 356, 345, 331, 318, 306, 306, 0]]).T]
        berries_codes = [np.array([1, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 79]),
                         np.array([1, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 79]),
                         np.array([1, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 79])]

        # Turn the verts and codes into a Path for the holly.
        holly_path = mpath.Path(holly_verts, holly_codes)

        # Create a bounding box for the desired location of the holly.
        self._holly_target_bbox = Bbox.unit()
        # Now update the desired location based on the location of the text.
        self.update_holy_position()

        # Construct the transform which takes us from holly coordinates to pixels.
        holly_to_pixel_trans = BboxTransform(holly_path.get_extents(), self._holly_target_bbox)

        # Construct a path effect which has a 2px snow border at the top.
        simple_snow_effect = [path_effects.withStroke(foreground='white', offset=(0, 1), linewidth=2)]

        # Construct a patch for the holly leaves, and attach the path effect.
        holly = mpatches.PathPatch(holly_path, facecolor='#008000', edgecolor='black',
                                   transform=holly_to_pixel_trans, path_effects=simple_snow_effect)
        # Add the patch to the axes.
        self.ax.add_patch(holly)

        # For each of the berries, create the appropriate path, and patch, and add to the axes.
        for berry_verts, berry_codes in zip(berries_verts, berries_codes):
            berry_path = mpath.Path(berry_verts, berry_codes)
            berry = mpatches.PathPatch(berry_path, facecolor='red', edgecolor='black',
                                       transform=holly_to_pixel_trans, path_effects=simple_snow_effect)
            self.ax.add_patch(berry)

    def update_holy_position(self, event=None):
        """Compute the desired bounding box for the holly in figure (pixel) coordinates."""
        # Get the bounding box of the text.
        text_extent = self.text.get_window_extent(self.ax.get_renderer_cache())
        x_min, y_min = text_extent.xmax - 20, text_extent.ymin - 10
        x_max, y_max = x_min + 100, y_min + 100
        self._holly_target_bbox.set_points(np.array([[x_min, y_min], [x_max, y_max]]))

    def create_groundcover(self):
        def smooth(y, window_size):
            box = np.ones(window_size) / float(window_size)
            return np.convolve(y, box, mode='same')
        n_pts, smooth_size = 60, 20
        x = np.linspace(-0.5, 1.5, n_pts)
        y = smooth(np.clip(np.random.poisson(1, n_pts) * 0.2, 0, 0.15), window_size=smooth_size)
        self.ax.fill_between(x, y, 0, color='white', transform=self.ax.transAxes)

    def animate_initial_step(self):
        # Setup the figure without snow, the snow will be added with blitting later on.
        self.flakes.set_visible(False)
        return [self.flakes]

    def animate_step(self, i):
        # This is where the magic happens.
        self.flakes.set_visible(True)

        flake_data = self.advance_flakes()

        self.flakes.set_offsets(np.vstack([flake_data.x, flake_data.y]).T)
        return [self.flakes]


if __name__ == '__main__':
    if plt.get_backend().lower() in ['macosx', 'agg']:
        raise RuntimeError('Sorry, the greeting does not work on the {} backend.'.format(plt.get_backend()))
    if matplotlib.__version__ < '1.4':
        raise RuntimeError('Sorry, the greeting does not work on matplotlib < 1.4.')

    fig = plt.figure(facecolor='black', dpi=100, figsize=(9.5, 6))
    ax = plt.axes([0, 0, 1, 1], xlim=(0, 1), ylim=(0, 1), axis_bgcolor='black')
    ax.set_axis_off()
    plt.draw()
    greetings_card = SeasonsGreetings(ax, message="Season's greetings\nfrom matplotlib")
    ani = animation.FuncAnimation(fig, greetings_card.animate_step,
                                  init_func=greetings_card.animate_initial_step,
                                  interval=25, blit=True)
    plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
