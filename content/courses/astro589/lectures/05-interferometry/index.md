---
title: Interferometry in Practice
date: 2022-09-21
publishdate: 2022-08-21
draft: true
---

## References for today

* [The Fourier Transform and its Applications](https://catalog.libraries.psu.edu/catalog/2010095) by R. Bracewell
* [Interferometry and Synthesis in Radio Astronomy](https://catalog.libraries.psu.edu/catalog/20789467) by Thompson, Moran, and Swenson, particularly Appendix 2.1
* [Essential Radio Astronomy](https://www.cv.nrao.edu/~sransom/web/xxx.html) by James Condon and Scott Ransom


## Review *of last time*

* Interpolation kernels
* Discrete Fourier Transform (DFT)
* Fast Fourier Transform (FFT)
* 

## This time

* 2D images and Fourier transforms
* image and Fourier coordinates for astronomy 
* definition of the Visibility function
* two-element interferometer
* complex noise and measurement
* UV coverage
* Earth-aperture synthesis

## Images

Thus far, we've been talking about 1 dimensional Fourier transforms. Now we'll talk about images and 2D Fourier transforms. 

We'll start by covering units for images. 


### R.A. and Dec.

From now on we'll mostly be concerned with imaging the celestial sphere. 

Declination's the easier one. No matter where you are, if you move in declination, you move along a great circle (i.e., a circle that actually traces the circumference of the celestial sphere). We measure this in terms of 0 (celestial equator) to +90 degrees at the north celestial pole and -90 degrees at the southern celestial pole. You can split a degree into arc minutes and then arc seconds, and these arc seconds are indeed the same ones we've been talking about so far. 

TODO: celestial sphere figure

Right Ascension is the one that trips me up sometimes. Because the sky rotates, we have this system of referring to it using 24 *hours* of right ascension, 60 *minutes*, and 60 *seconds*. Although they are still in multiples of 60, these *minutes* and *seconds* of right ascension do not have the same angular size. For example, let's say we have two points on the sky. 

    p1 = '00h42m00s', '+41d12m'
    p2 = '00h42m01s', '+41d12m'

So, same declination, but their R.A. values differ by one second. Can any one say how many *arcseconds* separate these points? The answer is about 11.3 arcseconds.

TODO: plot with points labeled, and distance between them

Usually, when we are talking about observing a smaller source (like a protoplanetary disk, or galaxy), we point in some direction towards that object and then define a small little postage stamp, commonly in units of $\Delta \delta$ and $\Delta \alpha \cos \delta$, in which case, the units describing the image are arcseconds. We're still talking about spherical astronomy here, so this isn't necessarily limited to small fields of view. We could have $\Delta \delta$ be several degrees, for example. 

TODO: Figure: point relative to center, and then Delta directions coming off of it.

### Direction cosines

We want to talk about *images* and the Fourier transforms of those images. In a moment, we're going talk about the mechanics of interferometers observing the celestial sphere and how these relate to Fourier transforms. Before we talk about that, though, there's a concept I want to introduce while we're still talking about units for images.

Practically speaking, for most images we're going to be making with interferometers like ALMA and the VLA, our field of view will be reasonably small (< 1 arcminute). In this regime, it makes a lot of sense to talk about "flat" images, i.e., image planes that are tangent to the field center. In 1D, it would look like this

{{< figure src="direction-cosine.png" caption="The concept of the direction cosine. Credit: Fig 3.3 TMS" >}}

The direction cosines are
$$
l = \sin(\Delta \alpha \cos \delta) 
$$
and 
$$
m = \sin(\Delta \delta)
$$
relative to the phase center. Because they are outputs from trigonometric functions, they are technically unitless. You can see from the figure that it is the direction cosine that is actually tracking the position on the tangent plane, where we are defining our image.

Though l and m are technically unitless, for small angular extent, they could also be considered to have units of radians. So it will be common that we . 

All of this goes out the window when we consider wide-field imaging (which we won't have time to talk about in this course, though you might consider for a group project). These types of ap



'The coordinate system .l;m/ defined above is a convenient one in which to present an intensity distribution. It corresponds to the projection of the celestial sphere onto a plane that is a tangent at the field center, as shown in Fig. 3.3. The distance of any point in the image from the .l; m/ origin is proportional to the sine of the corresponding angle on the sky, so for small fields, distances on the image are closely proportional to the corresponding angles. The same relationship usually applies to the field of an optical telescope.'

TMS pg 93.

The problem




The problem we're going to run into here, though, is 




When we'll be talking about Most of the 

* Definition of direction cosines l, m
* Definition of visibility function as 2DFT of image
TMS pg 93, 95.
van Cettert-Zernike theorem (15.1.1 TMS)

* u, v coordinates
Though l and m are technically unitless, for small angular extent, they could also be considered to have units of radians, implying that u and v can also be interpreted in units of cycles per radian.


* introduction of q coordinate, and some polar angle

* Examples of 2D FTs
* Discussion of numerical implementation of 2D FTs
* Images are real, therefore the real part of the visibility function must always be even and the imaginary part odd. The visibility function is Hermitian.
* Structure of "natural" images, what the amp, phase, etc.. looks like for real scenes

and then revisit some questions that also apply to the DFT:

* What have we assumed about the periodic nature of the samples?
* Windowing / response / filters


* two-element interferometer
* Complex noise and measurement
* UV coverage
* Earth-aperture synthesis

Calculations:

* relationship between arcseconds, direction cosine, spatial frequency,
* fringe-patterns
