---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "My Article"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2020-07-06T03:07:46+08:00
lastmod: 2020-07-06T03:07:46+08:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---



## Ex 7.2

By considering both the Friedmann and acceleration equations, and assuming a pressureless universe, demonstrate that in order to have a static universe we must have a closed universe with a positive vacuum energy. Using either physical arguments or mathematics, demonstrate that this solution must be unstable.

**Answer.**  

弗里德曼方程：
$$
\frac{\dot{a}^2}{a^2}+\frac{kc^2}{a^2}=\frac{8\pi G}{3}\rho + \frac{\Lambda c^2}{3}\tag{1}
$$
加速度方程：
$$
\frac{\ddot{a}}{a} = \frac{\Lambda c^2}{3}-\frac{4\pi G}{3}\left(\rho+3\frac{p}{c^2}\right)\tag{2}
$$
考虑率无压力的宇宙，$p=0$，$(2)$式变为：
$$
\frac{\ddot{a}}{a} = \frac{\Lambda c^2}{3}-\frac{4\pi G}{3}\rho\tag{3}
$$
$(3)$式代入$(1)$式，消去$\Lambda$，有：
$$
\frac{\ddot{a}}{a}-\frac{\dot{a}^2}{a^2}-\frac{kc^2}{a^2}+4\pi G\rho=0\tag{4}
$$
对静态宇宙，$\dot{a}=\ddot{a}=0$，$(4)$式化为：
$$
\frac{kc^2}{a^2} = 4\pi G\rho\tag{5}
$$
对于物质$\rho\gt 0$，因此:
$$
k=4\pi G\rho a^2/c^2\gt 0
$$
因此$k\gt 0$，**对应一个封闭的宇宙**。

将$(3)$式代入$(1)$式消去$\rho$，整理可得：
$$
\frac{2\ddot{a}}{a}+\frac{\dot{a}^2}{a^2}+\frac{kc^2}{a^2}-\Lambda c^2=0\tag{6}
$$
对静态宇宙，$\dot{a}=\ddot{a}=0$，$(6)$式可得：
$$
\frac{kc^2}{a^2}-\Lambda c^2=0\tag{7}
$$
$(7)$带回$(6)$式，整理可得：
$$
\ddot{a} = -\frac{\dot{a}^2}{2a}
$$
两边对时间求导，有
$$
\dddot{a}=-2\dot{a}\ddot{a}\frac{1}{2a}+\dot{a}^2\frac{1}{2a^2}\cdot\dot{a}=-\dot{a}^3\frac{1}{2a^2}-\dot{a}^3\frac{1}{2a^2}=-\frac{\dot{a}^3}{2a^2}
$$
当微扰使$\dot{a}\gt 0$时，$\dddot{a}\lt 0$，$\ddot{a}=\int\dddot{a}\mathrm{d}t+\ddot{a}_{t=0}<0$，对应不稳定平衡，因此**这样的静态宇宙解是不稳定**的。



## Plot Figures

已知$H_0 = 73\ \mathrm{km/s/Mpc}$，$\Omega_M = 0.3$，$\Omega_\Lambda = 0.7$，$a_0=1$，令$k=1$，有：

<img src="/media/hoshizora/DATA/MySite/My_Website/content/post/my-article.assets/H_and_omega.png" style="zoom: 67%;" />

源码：

```python
import numpy as np
import matplotlib.pyplot as plt


# define parameters
c = 3e5         # km/s
H0 = 73.0       # km/s/Mpc
OME_M = 0.3
OME_L = 0.7
a0 = 1.0
k = 0

# define redshift
z = np.linspace(-0.5, 6.5, 100)

# calculate
E = np.sqrt(OME_M*(1+z)**3 + OME_L + (1-OME_M-OME_L)*(1+z)**2)
H = H0*E
H2 = H0*E/(1+z)
ome_M = OME_M*((1+z)/E)**2
ome_L = OME_L/E**2
ome_R = 1 - ome_M - ome_L
ome_K = -k*(c/a0/H0)**2*(z/z)
ome_T = 1-ome_K

# create figure
plt.figure()
ax = plt.gca()

# plot H(z)
ax.plot(z, H, 'c-', alpha=0.8)
ax.plot(z, H2, 'm-', alpha=0.8)

# plot omega(z)
ax2 = ax.twinx()
ax2.plot(z, ome_T, 'r--', alpha=0.7)
ax2.plot(z, ome_M, 'g--', alpha=0.7)
ax2.plot(z, ome_R, 'y--', alpha=0.7)
ax2.plot(z, ome_L, 'b--', alpha=0.7)

# draw label
ax2.text(4.12, 0.47, r'$H(z)$', fontdict=dict(color='c', alpha=0.8))
ax2.text(4.31, 0.08, r'$H(z)\ \frac{1}{1+z}$', fontdict=dict(color='m', alpha=0.8))
ax2.text(3.23, 0.94, r'$\Omega_T(z)$', fontdict=dict(color='r', alpha=0.7))
ax2.text(3.81, 0.24, r'$\Omega_M(z)$', fontdict=dict(color='g', alpha=0.7))
ax2.text(2.08, 0.57, r'$\Omega_R(z)$', fontdict=dict(color='y', alpha=0.7))
ax2.text(0.20, 0.62, r'$\Omega_{\Lambda}(z)$', fontdict=dict(color='b', alpha=0.7))

ax.set_xlabel(r'$z$')
ax.set_ylabel(r'$H(z)$')
ax2.set_ylabel(r'$\Omega(z)$')

plt.show()
plt.close()

```

