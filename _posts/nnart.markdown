---
layout: post
title:  "Nearest neighbor art"
date:   2017-01-23
categories: 
---

Recently I found this wallpaper depicting a deer stag, and wanted to 
recreate the effect. Here's the original (left) and my attempt (right): 

TODO: identify original artist

### The algorithm  
Looking at the picture, my initial hunch for an algorithm was something like this:

1. Generate a set of random points such that brighter locations are more likely

2. Draw lines between pairs of points according to some rule

Clearly, the rule by which the lines are generated would be critical for the 
quality of the result. I made a further guess that the lines are drawn based 
on some nearest neighbor relationships. In particular, to generate a visually
coherent picture, there shouldn't be too many lines crossing the darker regions.
Hence, I ended up with the following rule:

* From each point, draw lines to its K nearest neighbors, in the three dimensional
space formed by the x and y coordinates and the intensity.

The effect of including the intensity dimension is that the points are not
connected only to their immediate neighbors in the x-y plane, thus yielding
broader strokes.

### Generating points
Generating random points based on image brightness is simple: interpret the image
as a two-dimensional histogram, and sample from that. In practice, sampling from
arbitrary distributions can be done via the following procedure:

1. Generate uniform random number u between 0 and 1
2. Compute the random number via x = F^{-1}(u), where F is the inverse cumulative
density function (CDF) of the desired distribution.

Step 1 is easy, but how do we do step 2 starting with the histogram (the image)?
First, we need to convert the histogram into an empirical CDF. This is just the
cumulative sum of the histogram, scaled so that the largest element is 1. F^{-1}(u)
is then computed as the first element of the empirical CDF that is equal to or larger 
than u. (TODO: intuition!)

### Drawing lines
Next, we find the nearest neighbors. This can be done using `KDTree` in Scipy.

### Examples

The Python code for generating these pictures is available on [my
GitHub](https://github.com/eerlich/nnart).

