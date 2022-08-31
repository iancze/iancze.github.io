---
title: The Fourier Transform II
date: 2022-09-07
publishdate: 2022-08-30
draft: true
---

Review of last time:

* Defined Fourier transform, inverse
* Introduced convolution, impluse symbol, and theorems

## Filters and transfer functions

Pg 200, Bracewell.

This lecture:

* Anything we didn't cover in the last one
* Nyquist sampling theorem
* Discrete Fourier transform
* DFT units

The DFT only knows that it was fed a set of equally spaced samples (n).

We can express these units in cycles/sample by dividing frequencies in cycles/set by the number of evenly spaced samples/set  – watch how the units cancel out – to obtain the frequency range
 from 0 to almost 1 cycle/sample in 1/

 cycles/sample increments (1/1024th in our example), as shown in Figure

 cycles/sample,
So with typical setups the DFT’s output can always be referred to units of cycles/set and cycles/sample.   In the absence of additional information, however, these units cannot be directly related to linear space or to other physical units.

<https://www.strollswithmydog.com/units-of-discrete-fourier-transform/>

* Windowing / response / filters
* 2D fourier transform

Can we tease/introduce the spectrogram figure here?
