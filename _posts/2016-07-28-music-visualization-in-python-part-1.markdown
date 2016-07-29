---
layout: post
title:  "Music visualization in Python, part 1"
date:   2016-07-28 14:07:00 +0300
categories: 
---

This post is the first in a series, in which I plan to document the process of
creating a music visualizer in Python using NumPy, SciPy and other libraries
that may be needed. The text assumes basic knowledge of Python, as well as the
basics of NumPy and SciPy. Some signal processing and image processing knowledge
will also be helpful, but not necessary, for understanding the text.

### Music visualization 
YouTube is a major channel for distributing and discovering new music. Since it
is a video streaming service, uploaded music needs to be accompanied with a
video. Although it's perfectly acceptable to create the video from a still
image, there is just something hypnotic about a picture that reacts to music.

Now, there is plenty of software that can be used to make beautiful
visualizations. This tutorial is all about reinventing the wheel, while 
creating some visualizations for my own music.

In these posts, I will distinguish two types of visualizations:
**post-processing effects** and **drawing-based visualizations**. An example of
a post-processing effect would be a blur that varies in intensity based on the
music signal, and a simple drawing-based visualization would be a bar graph that
shows the amplitude spectrum of the audio. In this first post, we will set up
the input and output.

### Input and output
The first thing we need to set up is IO -- we need audio input, image input and
video output. The first two are handled by `scipy.io.wavfile` and
`scipy.ndimage`:

{% highlight python %}
import scipy as sp
import scipy.io.wavfile
import scipy.ndimage

(Fs,audio)=sp.io.wavfile.read(audiofile) # Fs is the sampling rate
img=sp.ndimage.imread(imgfile)
{% endhighlight %}

For the purposes of this project, we don't need stereo audio, so let's
convert a stereo signal to mono:
{% highlight python %}
audio=0.5*audio[:,0]+0.5*audio[:,1]
{% endhighlight %}

For video IO, the most mature library I could find was
[imageio](https://imageio.github.io/), which runs ffmpeg and avlib under the
hood. We can instantiate a video writer like so:

{% highlight python %}
import imageio
fname='./output.mp4' # the output format is inferred from the extension
fps=30
writer=imageio.get_writer(fname,fps=fps)
{% endhighlight %}

Now we can write images to the output:
{% highlight python %}
writer.append_data(img) # append one frame to the video
writer.append_data(img) # append another frame 
writer.close() # close when done
{% endhighlight %}

### The processing loop
We will generate the frames to the video in a `for` loop. For that, we need the
number of frames, which can be calculated as 
{% highlight python %}
N=long(np.ceil(1.0*fps*len(audio)/Fs))
{% endhighlight %}

And here's a sketch of how our processing loop will look like:
{% highlight python %}
for k in range(N):
    # extract numeric features from audio used to control
    # the visualization
    ...

    # apply pre_fx
    ...

    # generate drawings
    ...

    # apply post-fx
    ...

{% endhighlight %}

### To be continued...
So in this post we only covered the boring stuff, but we now have a pipeline set
up for creating more interesting things. In the next post, I will discuss what
features to extract from the audio, and create a simple visualization. The code
for this project, which may not be entirely in sync with the blog, is available
on [my GitHub](https://github.com/eerlich/beatviz).

