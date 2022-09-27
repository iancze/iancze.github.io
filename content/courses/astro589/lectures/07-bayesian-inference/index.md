---
title: Bayesian Inference and Model Fitting
date: 2022-10-05
publishdate: 2022-09-21
draft: true
---

* Forward-modeling with a *generative* model
* Complex-valued noise and measurement


* Complex noise and measurement
For more on this, see the [MPoL notes on likelihood functions](https://mpol-dev.github.io/MPoL/rml_intro.html).
* Images are real, therefore the real part of the visibility function must always be even and the imaginary part odd. The visibility function is Hermitian.
* Structure of "natural" images, what the amp, phase, etc.. looks like for real scenes

Intro-to:

* Image Synthesis: Dirty image [[20210223144503]] image-synthesis-dirty
* UV coverage and point spread functions
* Dirty image, idea of convolution of PSF + source
* Sidelobes
* Missing spatial frequencies + images

### Gridding

* Brief introduction to "gridding"
* [[20210204160702]] gridding
* [[20210625182413]] nufft
* Convolutional resampling
* Uniform vs. Natural weighting/gridding
* Briggs robust weighting
* and effects on PSF, sensitivity

Can we drive home the idea that we have different sensitivity to different spatial scales? This is where the spectrogram concept really comes in handy.
