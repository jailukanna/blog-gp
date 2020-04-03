---
title: matplotlib example show agent (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'show agent'

Functions in program: 
* `def show_agent(state, next_state, action, screen):`

Modules used in program: 
* `import matplotlib.backends.backend_agg as agg`
* `import matplotlib.gridspec as gridspec`
* `import matplotlib`

## python show agent

Python matplotlib example: show agent

```python
import matplotlib
matplotlib.use('Agg')
from matplotlib import pylab as plt
import matplotlib.gridspec as gridspec
import matplotlib.backends.backend_agg as agg
from matplotlib.ticker import FuncFormatter, MaxNLocator

# You need pygame and matplotlib: !pip install pygame matplotlib
# You need to create a pygame screen as well
# Something like this:
#     pygame.init()
#     screen = pygame.display.set_mode(VIEW_RESOLUTION, pygame.DOUBLEBUF)
# Then you pass the screen as an argument to `show_agent`
#
# If you want to wath every frame, just add the call to the code
# after performing an action


STACK_SIZE = 4
FRAME_SKIP = 1
VIEW_RESOLUTION = 1280, 720
ACTIONS = {
    0: '↑',
    1: '↓',
    2: '←',
    3: '→',
}

def show_agent(state, next_state, action, screen):
    fig = plt.figure(0, figsize=(VIEW_RESOLUTION[0]/96, VIEW_RESOLUTION[1]/96), dpi=96)

    for i in range(4):
        ax = plt.subplot2grid((9, 2), ((i // 2) * 2, i % 2), rowspan=2)
        ax.imshow(state[:, i, :, :].transpose(1, 2, 0))
        ax.set_title('State - %d' % (3 - i))

    for i in range(4):
        ax = plt.subplot2grid((9, 2), (4 + (i // 2) * 2, i % 2), rowspan=2)
        ax.imshow(next_state[:, i, :, :].transpose(1, 2, 0))
        ax.set_title('Next State - %d' % (3 - i))

    a = np.zeros((1, 4))
    a[0, action] = 1
    ax = plt.subplot2grid((9, 2), (8, 0), colspan=2)
    ax.imshow(a, cmap='gray')
    ax.xaxis.set_major_formatter(FuncFormatter(tick_formatter))
    ax.xaxis.set_major_locator(MaxNLocator(integer=True))

    fig.tight_layout()

    canvas = agg.FigureCanvasAgg(fig)
    canvas.draw()
    renderer = canvas.get_renderer()
    raw_data = renderer.tostring_rgb()

    size = canvas.get_width_height()

    surf = pygame.image.fromstring(raw_data, size, "RGB")
    surf_pos = surf.get_rect()
    screen.blit(surf, surf_pos)
    pygame.display.update()
    plt.close(fig)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
