---
layout: post
title:  "Music visualization in Python, part 1"
date:   2016-07-28 14:07:00 +0300
categories: 
---

This post is the first in a series, in which I plan to document the process of
creating a music visualizer in Python using NumPy, SciPy and other libraries
that may be needed. 

# Music visualization 

To keep things simple, most of the visualization will be done by processing
an image given as input. In addition, we will look into drawing the frequency
spectrum of the audio.

# Input and output
The first thing that we need to set up is audio input, image input and video
output. For reading audio, we will need .

The most mature library I could find for video IO was

[imageio](https://imageio.github.io/), which runs ffmpeg under the hood.

