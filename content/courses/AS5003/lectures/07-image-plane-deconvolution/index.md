---
title: Image Plane Deconvolution (CLEAN)
date: 2023-09-26
publishdate: 2023-09-26
---

### References

* [Synthesis Imaging in Radio Astronomy II](https://leo.phys.unm.edu/~gbtaylor/astr423/s98book.pdf): Lecture 7: Imaging by Briggs, Schwab, and Sramek and Lecture 8: Deconvolution by Cornwell, Braun, and Briggs
* [Molecules with ALMA at Planet-forming Scales (MAPS). II. CLEAN Strategies for Synthesizing Images of Molecular Line Emission in Protoplanetary Disks](https://ui.adsabs.harvard.edu/abs/2021ApJS..257....2C/abstract) by Czekala et al. 2021


### Outline

* Recap of visibility datasets and the sampling function
* Image plane implications of sampling--the dirty image
* Noise and "weighting"
* The CLEAN image deconvolution algorithm


## Visibility datasets

Recall from last time that the visibility function is the Fourier transform of the sky brightness distribution

$$
\mathcal{V}(u,v) \leftrightharpoons I(l,m)
$$

and that each baseline (pair of antennas) of an interferometric array corresponds to a sample of the visibility function at a specific \\(u,v\\) point. The \\(u,v\\) point corresponds to the length of the *projected* baseline in multiples of the observing wavelength and is the *spatial frequency* of the image plane that is being sampled.

For a large array with > 50 antennas, like ALMA, you get nearly 1000 unique *instantaneous* baselines, from each pairwise combination of antennas in the array. As the earth rotates, you can quickly acquire new projected baseline samples.

## Sampling function

If you do radio interferometry, you will very often see the baseline distributions plotted as a series of \\(\delta\\) functions on the \\(u,v\\) plane. 

TODO: draw approximate plot with points

This is called the sampling function

$$
S(u,v) = \sum_{k=1}^M \delta(u - u_k, v - v_k).
$$

And, we can write down the sampling of the visibility function as 

$$
S(u, v) \times \mathcal{V}(u, v).
$$

If you recall from our Fourier transform distribution, this sampling function is also called the *transfer function*. It allows certain spatial frequencies through the interferometric system. Both terminologies are used in the radio astronomy community.

### Image plane implications

Now, let's discuss the image-plane ramifications of the sampling operation. We started with \\(I(l,m)\\), the "true" sky brightness, i.e., the one you would observe if you had a perfectly sensitive telescope with infinite resolving power.

$$
\mathcal{V}(u,v) \leftrightharpoons I(l,m)
$$

but we've *multiplied* it by the sampling distribution 

$$
S(u, v) \times \mathcal{V}(u, v).
$$

Remember how we talked about interferometers as *spatial filters*? We just showed how this conceptually works in the Fourier domain, the \\(u,v\\) coverage provided by the antenna spacings *is* the transfer function.

We can also show that interferometers are spatial filters by considering the image plane implications. This same operation implies that the true sky brightness is *convolved* by something in the image plane
$$
I(l, m) \* B_D(l, m) \leftrightharpoons S(u, v) \times \mathcal{V}(u, v).
$$

The quantity \\(B_D(l,m,)\\) is called the dirty beam. We'll soon talk about the CLEAN algorithm, so the dirty/clean terminology will soon make sense. But first, let's just think about this for a second. This dirty beam is the same thing we showed in previous lectures, and can also be thought of as the sum of the fringe functions.


If you take an image, convolve it with the dirty beam, and then take its Fourier transform, you'll see that you will have visibility samples only at the spatial frequencies corresponding to your baselines.

Another way to think of the dirty beam is as the *impulse response* of the interferometric system. Let's assume we are observing a point source \\(I(l,m) = \delta(l,m)\\). The visibility function corresponding to a point source is a constant, so we have

$$
\delta(l,m) \* B_D(l,m) = S(u,v) \times \mathrm{constant}
$$

$$
B_D(l,m) \leftrightharpoons S(u,v).
$$

So, another way we can say the same thing is that the dirty beam is the *point spread function* (PSF) of the interferometer, and it is given by the Fourier transform of the sampling function, which is set by the configuration of the baselines within the array.

## The Dirty Image

In last week's lecture, we talked a bit about the actual visibility data products, a discrete set of (noisy) visibility samples

$$
\mathbf{V} = \\{V_1, V_2, \ldots, V_M\\}^M_{k=1}.
$$

The idea is that each was sampled with some (complex) noise draw

$$
V_i = \mathcal{V}(u_i, v_i) + \epsilon
$$

such that
$$
\epsilon_\Re \sim \mathcal{N}(0, \sigma)
$$

$$
\epsilon_\Im \sim \mathcal{N}(0, \sigma)
$$

$$
\epsilon = \epsilon_\Re + i \epsilon_\Im.
$$

And then we said that radio interferometers commonly represent the uncertainty on each visibility measurement by a "weight" \\(w_i\\), where

$$
w_i = \frac{1}{\sigma_i^2}.
$$


### More details about the sampling function

The fact that each visibility measurement is made in the presence of noise means that we should be taking this into account in our sampling function. So a more sophisticated sampling function looks like

$$
S(u, v) = \sum_{k=1}^M T_k D_k w_k \delta(u - u_k, v - v_k).
$$

In addition to weighting each sample by its inverse variance \\(w_k\\) (something you might do to take a statistical average), there are other factors you might fiddle with, like a "taper" \\(T_k\\) and density weight \\(D_k\\). For now, you can just think of them as equal to 1.0.

Now, let's think about how we would take our sampled visibilities \\(\mathbf{V} = \\{V_1, V_2, \ldots, V_M\\}^M_{k=1}\\) and  make an image from them. Well, the way to do this looks like an inverse Fourier transform

$$
I_D(l, m) = C \sum_{k=1}^{N^\dagger} T_k D_k w_k V_k \exp \\{2 \pi i (u_k l + v_k m) \\}.
$$
with normalization constant
$$
C = 1 / \sum_{k=1}^{N^\dagger} T_k D_k w_k.
$$

where \\(N^\dagger = 2 N\\) is the set of visibilities that includes their complex conjugates such that the sampling is Hermitian. This image that results is called the *dirty image*, and we denote it with a "D" subscript.

Continuing with the dirty beam concept, we also have

$$
I_D(l,m) = I(l, m) \* B_D(l, m) \leftrightharpoons S(u, v) \times \mathcal{V}(u, v).
$$

### A quick note about the "ill-defined" imaging problem and weighting choices

Making images from Fourier samples is generally an ill-defined inverse process, which is only complicated in the presence of noise. What do we mean by ill-defined? 

In the forward process, we collect samples of the visibility function at specific \\(u,v\\) values \\(S(u, v) \times \mathcal{V}(u, v)\\). To make a dirty image, we take the inverse Fourier transform of those values to produce *an* image. This is the inverse process.

**The ill-defined nature of imaging** (even in the presence of noise): Think about what's happening in the forward process. If the visibility function had non-zero amplitudes at some \\(u,v\\) values that we didn't sample, then these never enter our dataset (obviously). The inverse process itself doesn't include these values (obviously), and by their omission, assumes that their amplitudes are equal to zero. 

So, as far as our interferometer is concerned, it can't distinguish between a set of degenerate image brightness distributions on the sky so long as they have the exact same \\(\mathcal{V}(u,v)\\) values at the sampled \\(u,v\\) points. The unsampled \\(\mathcal{V}(u,v)\\) locations can take on arbitrary values and still result in the exact same dataset. 


Another way of saying the same thing is to think of an interferometer as a *spatial filter*, i.e., its transfer function (the array configuration) only allows measurements of certain spatial frequencies to enter the dataset. But most images contain power at many spatial frequencies, including those that have been filtered out. So, if you just try to make an image with the spatial frequencies in your dataset, your image will most likely be missing some spatial frequencies that would be there in actuality.

#### Weighting choices

The whole discussion about the ill-defined inverse problem applies *even if* we sampled the visibility function perfectly with no noise, so long as there are still unsampled \\(u,v\\) points that contain significant visibility "power."

The problem gets even more complicated when we consider measurement noise and the fact that array configurations usually sample some parts of \\(u,v\\) space better than others. In my opinion, this is really why various weighting schemes are as popular as they are. The common way to do this is by tuning the \\(D_k\\) and \\(T_k\\) terms. As we'll show later in the slides, tuning the \\(D_k\\) terms provide a tradeoff between:

* **natural weighting** maximizing point source sensitivity at the cost of spatial resolution (broader beam) 
* **uniform weighting** maximizing spatial resolution (narrow beam) at the cost of point source sensitivity (higher RMS noise floor in the image)
* "Briggs" **robust weighting** a tradeoff between these two regimes, ranging from (-2 to 2). The tradeoff is non-linear, so coming from natural weighting, for example, good spatial resolution can be gained with only modest sacrifices in point source sensitivity. Or, vice versa, coming from uniform weighting, good sensitivity to point sources can be gained with only modest losses in resolution.

Typically, a reasonable starting point with ALMA observations is to use some in-between value of Briggs weighting like -0.5, 0.0, or 0.5. Different weighting choices can change your sensitivities on different spatial scales.

The \\(T_k\\) terms are for applying a *taper*, whereby one downweights longer baseline observations.

## CLEAN

We've talked about how

$$
I_D(l,m) = I(l, m) \* B_D(l, m) \leftrightharpoons S(u, v) \times \mathcal{V}(u, v).
$$

Specifically, the *dirty image* is the *convolution* of the true sky brightness \\(I\\) with the dirty beam \\(B_D\\). We know what the dirty beam is to high precision.

First, it's important to note that convolution is a *lossy* procedure, you (irrevocably) lose information. For example, consider applying a Gaussian blur to an image. The high resolution information in that image has been lost.

CLEAN is an *image deconvolution* algorithm. We just said that convolution is a lossy procedure, so, how does the algorithm get that information back? What follows are my own opinions about the CLEAN algorithm, its use cases I'm most familiar with in the protoplanetary disk community, and its limitations.

TODO: draw a 1D cut of the dirty beam

The short answer is that CLEAN can help restore an image *up to a point*. The thing that CLEAN is best at is removing the effect of those nasty sidelobes from a dirty beam, and replacing them with a more Gaussian beam response that is usually easier to work with. CLEAN will not give you "super-resolution" access to lost spatial frequencies that you have lost, but it can help you make better looking images, and ones that have better dynamic range.

### Iterative processes

CLEAN is a procedure that iteratively builds up a model image. To carry this out on the whiteboard, I'm going to do things in 1D. In a moment we'll show an example with 2D images in the slides.

TODO: draw in 1D a dirty image of a few point sources, a representation of the dirty beam, and a blank model image

Before you start, we'll define a quantity called the CLEAN beam. It's usually chosen to be a Gaussian fit to the main lobe of the beam.

* 1. First, we identify the *peak* location in the dirty image.
* 2a. Then, we subtract some fraction of the flux times the dirty beam from this location. This dirty image becomes a "residual image" now.
* 2b. At the same time, we add a \\(\delta\\) function at corresponding location in the model image with the same amplitude as the flux we subtracted. So, if we subtracted 0.1 Jy of flux in the dirty beam, then we would add a \\(\delta\\) function with amplitude of 0.1 Jy in the model image.
* 2c. You can think of these steps as equivalent, because a \\(\delta\\) function times the dirty beam gives you back the dirty beam. These steps are *also* equivalently carried out in the visibility domain.
* 3. Go back to step 1, and repeat with the next-highest *peak* location. Continue this loop until the peak flux in the image drops below some threshold
* 4. Once this threshold is reached, the CLEANing is done. The final step is to put everything back together. The model image is convolved with the CLEAN beam to form the restored image. This "smooths out" the model image to some resolution limit, and hides imperfections on smaller scales.
* 5. The remainder of the residual image is added back to the restored image to give a sense of the "noise" in the image.

### Limitations

CLEAN is *procedural*. What this means is that you set parameters that guide the above process and then carry on until some termination criterion is reached. This could also be part of an interactive process. There is no guarantee that the CLEANed image is unique, either.

In my opinion, CLEAN is best at removing the sidelobe effects of the dirty beam, improving the dynamic range of your image, and possibly detecting fainter (point-like) sources that would have been hidden by the sidelobes of other brighter point sources.

In the above example, we said that we would use a \\(\delta\\) function to build up a model image. You may have already identified this choice of basis set or "CLEAN component" as a potential limitation. This works great for fields of point sources, but what about extended sources? It turns out that it actually works *OK* for extended sources, so long as you have many of them. This adds to the computational time, though, and is why I think we're now currently in an interesting place, approaching the limitations of CLEAN.

There are other extensions to CLEAN (called multi-scale), which use CLEAN components of varying sizes, like little Gaussian blobs. This can help substantially over using just \\(\delta\\) functions. For very resolved structures, though, you still run into the problem that your components aren't sufficiently like the morphology of the source you're trying to deconvolve.
