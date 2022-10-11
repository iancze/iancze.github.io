---
title: Image Plane Deconvolution (CLEAN)
date: 2022-10-12
publishdate: 2022-10-08
---

### References

* [Synthesis Imaging in Radio Astronomy II](https://leo.phys.unm.edu/~gbtaylor/astr423/s98book.pdf): Lecture 7: Imaging by Briggs, Schwab, and Sramek and Lecture 8: Deconvolution by Cornwell, Braun, and Briggs

### Outline

* Concept of "direct Fourier transform" and the image that results
* Brief mention of how you adjust the weights
    * Briggs robust weighting
    * and effects on PSF, sensitivity
    * Can we drive home the idea that we have different sensitivity to different spatial scales? This is where the spectrogram concept really comes in handy.
* However, this image that we produce isn't unique
    * any of the fourier modes could be changed (show image w/ saturn/ALMA logo)

* CLEAN process
    * Image-plane deconvolution via CLEAN (w/ point sources)
    * w/ extended structure

* Definition of likelihood function: sort of free from it
* Examining the CLEAN model as your non-parametric sky model
* Criticisms of CLEAN
    * procedural, in practice
    * Basis sets are not great, computational barrier in the limit of many components.

Keynote pres: 
    * show how the beam shape and size changes based upon how you weight the visibilities
    * show image w/ saturn/ALMA logo to demonstrate unchanging likelihood
    * erroneous components at the end with my movie with HD 163296

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

Now, let's discuss the image-plane ramifications of the sampling operation. We started with \\(I(l,m,)\\), the "true" sky brightness, i.e., the one you would observe if you had a perfectly sensitive telescope with infinite resolving power.

$$
\mathcal{V}(u,v) \leftrightharpoons I(l,m)
$$

but we've *multiplied* it by the sampling distribution. This means that the true sky brightness is *convolved* by something in the image plane

$$
I(l, m) \* B_D(l, m) \leftrightharpoons S(u, v) \times \mathcal{V}(u, v).
$$

the quantity \\(B_D(l,m,)\\) is called the dirty beam. We'll soon talk about the CLEAN algorithm, so the dirty/clean terminology will soon make sense. But first, let's just think about this for a second. This dirty beam is the same thing we showed in previous lectures, and can also be thought of as the sum of the fringe functions.

Remember how we talked about interferometers as *spatial filters*? We just showed how this conceptually works in the Fourier domain, the \\(u,v\\) coverage provided by the antenna spacings *is* the transfer function.

But, we can also show that interferometers are spatial filters by considering the image plane implications. If you take an image, convolve it with the dirty beam, and then take its Fourier transform, you'll see that you will have visibility samples only at the spatial frequencies corresponding to your baselines.

Another way to think of the dirty beam is as the *impulse response* of the interferometric system. Let's assume we are observing a point source \\(I(l,m) = \delta(l,m)\\). The visibility function corresponding to a point source is a constant, so we have

$$
\delta(l,m) \* B_D(l,m) = S(u,v) \times \mathrm{constant}
$$

$$
B_D(l,m) \leftrightharpoons S(u,v).
$$

So, another we can say this is that the dirty beam is the *point spread function* (PSF) of the interferometer, and it is given by the Fourier transform of the sampling function, which is set by the configuration of the baselines within the array.

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

### A quick note about the "ill-defined" imaging problem and weighting choices

Making images from Fourier samples is generally an ill-defined inverse process, which is only complicated in the presence of noise. What do we mean by ill-defined? 

When we observe an astrophysical source with an interferometer. In the forward process, we collect samples of the visibility function at specific \\(u,v\\) values \\(S(u, v) \times \mathcal{V}(u, v)\\). To make a dirty image, we take the inverse Fourier transform of those values to produce *an* image. This is the inverse process.

**The ill-defined nature of imaging** (even in the presence of noise): Think about what's happening in the forward process. If the visibility function had non-zero amplitudes at some \\(u,v\\) values that we didn't sample, then these never enter our dataset (obviously). The inverse process itself doesn't include these values (obviously), and by their omission, assumes that their amplitudes are equal to zero. 

So, as far as our interferometer is concerned, it can't distinguish between a set of degenerate image brightness distributions on the sky so long as they have the exact same \\(\mathcal{V}(u,v)\\) values at the sampled \\(u,v\\) points. The unsampled \\(\mathcal{V}(u,v)\\) locations can take on arbitrary values and still result in the exact same dataset. 

Another way of saying the same thing is to think of an interferometer as a *spatial filter*, i.e., its transfer function (the array configuration) only allows measurements of certain spatial frequencies to enter the dataset. But most images contain power at many spatial frequencies, including those that have been filtered out. So, if you just try to make an image with the spatial frequencies in your dataset, your image will most likely be missing some spatial frequencies that would be there in actuality.

#### Weighting choices

The whole discussion about the ill-defined inverse problem applies *even if* we sampled the visibility function perfectly with no noise, so long as there are still unsampled \\(u,v\\) points that contain significant visibility "power."

The problem gets even more complicated when we consider measurement noise and the fact that array configurations usually sample some parts of \\(u,v\\) space better than others. In my opinion, this is really why various weighting schemes have developed. The common way to do this is by tuning the \\(D_k\\) and \\(T_k\\) terms. As we'll show later in the slides, these provide a tradeoff between:

* **natural weighting** maximizing point source sensitivity at the cost of spatial resolution (broader beam) 
* **uniform weighting** maximizing spatial resolution (narrow beam) at the cost of point source sensitivity (higher RMS noise floor in the image)
* "Briggs" **robust weighting** a tradeoff between these two regimes, ranging from (-2 to 2). The tradeoff is non-linear, so coming from natural weighting, for example, good spatial resolution can be gained with only modest sacrifices in point source sensitivity. Or, vice versa, coming from uniform weighting, good sensitivity to point sources can be gained with only modest losses in resolution.

Typically, a reasonable starting point with ALMA observations is to use some in between value of Briggs weighting.
