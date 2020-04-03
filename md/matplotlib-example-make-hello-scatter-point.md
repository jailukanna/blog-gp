---
title: matplotlib example make-hello-scatter-point (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'make-hello-scatter-point'

Functions in program: 
* `def make_hello(N=1000, rseed=42):`

## python make-hello-scatter-point

Python matplotlib example: make-hello-scatter-point

```python
# 用matplotlib画hello的点集
# 1. 基于text生成一个png图像
# 2. 读取图像，随机取点阵，在png图像中有像素的点，就是一个目标点
# 3. 图像用scatter画出

# [from](https://jakevdp.github.io/PythonDataScienceHandbook/05.10-manifold-learning.html)
def make_hello(N=1000, rseed=42):
    # Make a plot with "HELLO" text; save as PNG
    fig, ax = plt.subplots(figsize=(4, 1))
    fig.subplots_adjust(left=0, right=1, bottom=0, top=1)
    ax.axis('off')
    ax.text(0.5, 0.4, 'hello', va='center', ha='center', weight='bold', size=85)
    fig.savefig('img/hello.png')
    plt.close(fig)
    
    # Open this PNG and draw random points from it
    from matplotlib.image import imread

    # imread得到的结果，按照左上角为原点，（y,x）的方式组织
    # 所以读取后，第一个维度装置，变为左下角为原点；第二个维度保持；取第三个维度的r通道
    # 判断是否存在数据即可；最终再装置一下，得到（x,y) 的排列
    src = imread('img/hello.png')
    data = src[::-1, :, 0].T

    rng = np.random.RandomState(rseed)
    X = rng.rand(4 * N, 2)

    # 乘以data.shape就是将[0, 1.0]的数据扩展到data的维度
    i, j = (X * data.shape).astype(int).T
    mask = (data[i, j] < 1)
    X = X[mask]

    # 找到之后，记得X的数据，x,y的范围都是[0, 1.0]，所以将y的长度记作1.0，x的长度需要扩展一下
    X[:, 0] = X[:, 0] * (data.shape[0] / data.shape[1])

    # 取得前N个数，再基于argsoft得到排序后的索引给，进而得到最终排序后的前N个有图像内容的数据点
    X = X[:N]
    return X[np.argsort(X[:, 0])]

X = make_hello()

# 将X离散到[0.0, 4.0]的好处就是可以指定color
# 如果指定离散的color数量，需用get_cmap
plt.scatter(
    X[:, 0], X[:, 1], 
    c=X[:, 0], 
    cmap='rainbow', #plt.cm.get_cmap('rainbow', 5));
    alpha=.5,
    edgecolors=None
)

# 保证X和Y轴的数据1:1的单位长度，调整图片的大小来适配
plt.axis('scaled');

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
