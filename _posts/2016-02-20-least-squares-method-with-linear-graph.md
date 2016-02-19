---
layout: post
title: "Plot linear equations using Least Squares Method"
tags: python
---


{% highlight python linenos %}
import sys
import math
import matplotlib.pyplot as plt
import numpy as np

# dataset
X = [(2, 3), (4, 7), (9, 11)]

# Average
a = sum(a for a, _ in X)
n = sum(1 for a, _ in X)
b = sum(b for _, b in X)
ux = float(a / n)
uy = float(b / n)
print 'AVG: ', ux, uy


# Variance
xsum = float(0)
ysum = float(0)
for a, b in X:
    xsum += math.pow(a - ux, 2)
    ysum += math.pow(b - uy, 2)
vx = xsum / n
vy = ysum / n
print 'V: ', vx, vy


# Standard Deviation
sdx = math.sqrt(vx)
sdy = math.sqrt(vy)
print 'SD: ', sdx, sdy


# Covarience
c = float(sum(a * b for a, b in X) / n)
d = (ux * uy)
cov = c - d
print 'COV: ', cov


# A: Slope, B: Intercept
A = cov / vx
B = uy - (A * ux)
print 'A: ', A
print 'B: ', B


# Draw graph
x = np.linspace(0, 15)
y = A*x+B

plt.plot([a for a, _ in X], [b for _, b in X], 'o')
plt.plot(x, y, 'r-')
plt.axis([0,15, 0, 15])
plt.show()
{% endhighlight %}

The result is here:

[![LMS result](/imgs/2016-02-20.png)](/imgs/2016-02-20.png)
