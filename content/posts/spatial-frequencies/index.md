---
title: Spatial Frequency Visualization
date: 2024-07-19
publishdate: 2024-07-19
---

Here are some animations I made for my [Contemporary Astrophysics Module]({{< relref "/courses/AS5003/_index.md" >}}) that illustrate the concept of spatial frequency.

In the left panel is an image of East Sands Beach and the town of St Andrews (West Sands and the River Eden are in the background) on a windy day, late in the Scottish "summer." The right panel uses a collection of sinusoids to represent the spatial frequencies present in the image. The spatial dimension of the sinusoids is depicted at the same scale of the image. The amplitudes the sinusoids represent the amplitude of each spatial frequency present in the image.

## Low pass filter

{{<video src="low-pass.mp4" >}}

* [Download link](low-pass.mp4) (right click and save as)

In this video, the image is convolved with a low-pass filter, whose cutoff frequency gradually decreases (in truth, the convolution operation is actually carried out as a multiplication in the Fourier domain). You can see that the amplitudes of the high spatial frequency sinusoids are attenuated as the image gradually loses resolution. By the end, the scale of the remaining features in the image are comparable to the periods of the non-zero sinusoids.


## High pass filter

{{<video src="high-pass.mp4" >}}

* [Download link](high-pass.mp4) (right click and save as)

In this video, the image is convolved with a high-pass filter, whose cutoff frequency gradually increases (in truth, the convolution operation is actually carried out as a multiplication in the Fourier domain). As the low spatial frequency features are attenuated, you can see the image lose its broader, more spatially uniform regions like the sky, clouds, and sea. The higher spatial frequency features remain, such as the outlines of buildings and vegetation.
