---
title: Be nice to your PDF reader
date: 2021-06-21
publishdate: 2021-06-21
---

With the rise of "big data," it's become common for figures in journal articles to show several hundred or more points on a single graph. This, and the statistical insights that originate from such figures, are great. But what is not so great is when you try to read a paper containing one of these plots and your PDF reader hangs as it struggles to render every facet of your beautiful, vectorized plot.

I myself am <a href="https://ui.adsabs.harvard.edu/abs/2017ApJ...851..132C/abstract">guilty of this</a>.

If you are using Matplotlib to plot many dense plots (in astronomy this includes high resolution spectra, exoplanet discoveries, MCMC traces, and more) consider using the <code>rasterized</code> <a href="https://matplotlib.org/api/_as_gen/matplotlib.artist.Artist.set_rasterized.html#matplotlib.artist.Artist.set_rasterized">keyword option</a> to prepare your figures and significantly reduce the workload for the PDF readers of your future audience.

To do this, include <code>rasterized=True</code> to your <code>plot</code> commands. You'll also likely want to include a <code>dpi=600</code> option to your <code>savefig</code> command (or change this in your <code>matplotlibrc</code>), since the default values are usually so low as to result in pixelated plots. Here is a script to create both a default (fully vectorized) version and a rasterized version:

<script src="https://gist.github.com/iancze/09b83f9f27b179e63e3c688881761b63.js"></script>

<img src="https://sites.psu.edu/iczekala/files/2020/10/scatter-rasterized-1024x768.png" alt="Matplotlib point cloud " width="680" height="510" class="alignleft size-large wp-image-122" />

While the rasterized option does reduce the filesize (compare 0.8 Mb to the fully vectorized version at 1.5Mb), the main benefit is how quickly your PDF reader will render this figure when it is viewed in a publication. I created sample RNAAS documents using the two different versions, available here:

<a href="https://sites.psu.edu/iczekala/files/2020/10/fully_vectorized.pdf">Fully Vectorized</a>

<a href="https://sites.psu.edu/iczekala/files/2020/10/rasterized.pdf">Partially Rasterized</a>

Try downloading these locally and then time how long your PDF takes to render them. I noticed the fully vectorized version took a few seconds to render on my laptop, while the rasterized version displayed instantaneously. If your paper is composed of several of these kinds of plots, that load time will add up and distract your reader from all the wonderful science you've done!

The figure axes, tick labels, etc, are still vectorized, so they will look decent when zoomed. But the dense point cloud has been rasterized, meaning it is stored as an image rather than with the vectorized instructions on how to draw every single point. There's obviously a turnover point in plot complexity when this option becomes useful. You wouldn't want to make every simple line plot with the <code>rasterized=True</code> option, but somewhere north of a few thousand plots is where it really starts to help. The best way to monitor this is to check whether your PDF loads snappily or not. Your reader will be much more likely to continue scrolling through your paper if everything goes smoothly vs. if their PDF reader hangs as it loads every last point cloud. If you do really want to provide every last detail about the data to your audience, the best venue is to provide the actual data values that were used to make a plot via a machine readable file.

Thinking about figure file size now will also pay dividends to your future self. Many applications (postdoc, faculty, grants, etc...) will require a sample of your published work. Most application servers place a limit on the upload file size. 15Mb is typical, but I have seen some applications with limits as low as 2Mb. If you can configure your manuscript filesize so that it is only as large as it needs to be, you'll avoid the problem of having to remake your figures and manuscripts later to satisfy strict upload limits.