---
title: The Fourier Transform II
date: 2022-09-07
publishdate: 2022-08-30
draft: true
---

## References for today
* [The Fourier Transform and its Applications](https://catalog.libraries.psu.edu/catalog/2010095) by R. Bracewell
* [Interferometry and Synthesis in Radio Astronomy](https://catalog.libraries.psu.edu/catalog/20789467) by Thompson, Moran, and Swenson, particularly Appendix 2.1
* [Fourier Analysis and Imaging](https://catalog.libraries.psu.edu/catalog/34517505) by R. Bracewell
* [Wikipedia on Nyqist-Shannon sampling theorem](https://en.wikipedia.org/wiki/Nyquist%E2%80%93Shannon_sampling_theorem)


## Review *of last time*

* Defined Fourier transform, inverse
* Introduced convolution, impluse symbol, and theorems

## Where are we headed *today*?

* Finish up Fourier transform theorems
* Nyquist sampling theorem
* Discrete Fourier Transform
* If time: windowing and sidelobes

## Continuing from last time: *Fourier transform theorems*

### Convolution/multiplication

The convolution of two functions corresponds to the multiplication of their Fourier transforms.

If

$$
f(x) \leftrightharpoons F(s)
$$

and

$$
g(x) \leftrightharpoons G(s)
$$

then

$$
f(x) * g(x) \leftrightharpoons F(s)G(s).
$$

This is an *extremely* useful theorem. At least in my career, this, and concepts related to sampling, have been the ones I have used the most often. You may have already used this theorem (numerically) if you've ever carried out a convolution operation using FFTconvolve functions in Python, which can be dramatically faster than directly implementing the convolution, at least for certain array sizes.

### Rayleigh's theorem (Parseval's theorem for Fourier Series)

The amount of energy in a system is the same whether you calculate it in the time domain or in the frequency domain.

The integral of the mod-squared of a function is equal to the integral of the mod-squared of its spectrum

$$
\int_{-\infty}^\infty |f(x)|^2\\,\mathrm{d}x  = \int_{-\infty}^\infty |F(s)|^2\\,\mathrm{d}s.
$$

### Autocorrelation theorem

The autocorrelation of a function is

$$
f(x) \* f(x) = \int_{-\infty}^\infty f^{\*}(u) f(u + x)\\,\mathrm{d}u
$$

and it has the Fourier transform \\(|F(s)|^2\\).

If you've ever worked with (stationary) Gaussian processes (e.g., squared-exponential, Matern, etc...), you might recognize this relationship between the autocorrelation (the kernel function) and the power spectrum of the Gaussian process.

### The derivative theorem

If

$$
f(x) \leftrightharpoons F(s)
$$

then

$$
f^\prime(x) \leftrightharpoons i 2 \pi s F(s).
$$

### Using the transform pairs

Now that we have a few basic transform pairs, and some of the transform theorems, you can mix and match these to build up a library of new transform pairs. You will explore this in the problem set.

### Definite integral

The zero-valued frequency of a Fourier transform is equal to the definite integral of a function over all space

$$
\int_{-\infty}^\infty f(x)\\,\mathrm{d}x = F(0).
$$

I.e., to compute the area under the curve, you can just read off the zero-frequency value of the Fourier transform.

### First moment

The first moment of \\(f(x)\\) about the origin is

$$
\int_{-\infty}^\infty x f(x)\\,\mathrm{d}x
$$

Using the derivative theorem (not shown), you can determine that the first moment \\(\bar{f}\\) is

$$
\bar{f} = \int_{-\infty}^\infty x f(x)\\,\mathrm{d}x = \frac{F^\prime(0)}{-2 \pi i}
$$
i.e., the slope of the Fourier transform evaluated at \\(s = 0\\), times \\(-1/2 \pi i\\).

### Second moment

Second moment (moment of inertia) is given by

$$
\int_{-\infty}^\infty x^2 f(x)\\,\mathrm{d}x = -\frac{1}{4 \pi^2} F^{\prime\prime}(0).
$$

{{< figure src="second-moment.png" caption="Credit: Bracewell Fig 8.4">}}

Similar arguments extend to variance \\(\sigma^2\\) as we had mentioned with regards to the uncertainty principle.

## Smoothness and compactness

In general, the smoother a function is in the time domain, the more compact its Fourier transform will be in the frequency domain.

Smoother functions will have a larger number of continuous derivatives.  Something like the Gaussian envelope \\(\exp(-\pi x^2)\\) is "as smooth as possible" and therefore its Fourier transform (also a Gaussian envelope) is as compact as possible.


## Filters and transfer functions

### Time domain 

We can say that we have some (electrical) waveform 
$$
V_1(t) = A \cos 2 (\pi f t)
$$ which is a single-valued function of time. You can think of this as a voltage time-series or another physical quantity. By definition, the waveform is real.

Let's put on our electrical/acoustical/mechanical engineering hats for a moment and consider that a *filter* is a physical system with an input an and output, e.g., something that is transmitting vibrations or oscillations, like our waveform.

If we feed our waveform into a *linear* filter, we get output
$$
V_2(t) = B \cos (2 \pi f t + \phi).
$$

The output is still a waveform, but its amplitude and its phase have changed. These changes are likely frequency dependent, too.

TODO: [Draw an example of the waveform being attenuated and shifted in phase].

We can specify the filter by a frequency-dependent quantity \\(T(f)\\) called the *transfer function*. It is a complex-valued function (having both an amplitude and a phase) and is given by
$$
T(f) = \frac{B}{A}e^{i \phi}.
$$

Interesting, perhaps, but maybe not immediately obviously useful. Let's introduce the *spectrum* and we'll circle back to the transfer function.

### Frequency domain

The *spectrum* of the waveform is the Fourier transform of \\(V(t)\\), which we'll call \\(S(f)\\) in this section. We've broken slightly from our \\(f \leftrightharpoons F\\) notation, but \\(V \leftrightharpoons S\\) is a classic in the signal processing and electrical engineering fields.

The "spectrum" here is just the Fourier transform quantity, it can definitely be complex-valued. 

The "spectra" that we typically talk about in astrophysics are measurements of the electromagnetic spectrum---you've probably never come across as one that's complex-valued, right? What's going on here?

Consider the *units* of a flux measurement \\(F_\nu\\) of the electromagnetic spectrum. In lecture 1, we covered that the cgs units of flux are
$$
\mathrm{ergs}\\;\mathrm{s}^{-1}\\;\mathrm{cm}^{-2}\\;\mathrm{Hz}^{-1}.
$$

The clue is in the \\(\mathrm{ergs}\\;\mathrm{s}^{-1}\\) part, which we could also write in terms of "watts" if we wanted to be strictly S.I. about it. When we are measuring the electromagnetic spectrum, we are actually measuring the **power** spectral density, \\(|F(\nu)|^2\\). The absolute squared means
$$
|F|^2 = F F^*
$$
i.e., the quantity \\(|F(\nu)|^2\\) is real-valued, and is the reason why you never hear about measurements of the electromagnetic spectrum containing imaginary values!

In this course, at least, we'll try to be explicit about which spectrum we're referring to. When we actually mean power spectrum, we'll try to call it as such. Otherwise, "spectrum" will refer to a quantity like \\(S\\).

Now, let's revisit our filter example, where we had input and output \\(V_1(t)\\) and \\(V_2(t)\\) respectively. From our discussion, we also have

$$
V_1 \leftrightharpoons S_1
$$
and
$$
V_2 \leftrightharpoons S_2.
$$

The transfer function concept is *especially* useful when we think about the *spectrum* of the waveforms, because we have
$$
S_2(f) = T(f) S_1(f).
$$

I.e., the spectrum of the output waveform is simply the spectrum of the input waveform *multiplied* by the transfer function. 

TODO: the square showing the relationship between these quantities.

Now let's think of digital signal processing, where you wanted to practically apply a filter to some waveform to get a new waveform. As we just outlined, you could acquire \\(V_1(t)\\), Fourier transform it to access its spectrum \\(S_1(f)\\), multiply by the transfer function \\(T_(f)\\), and then do the inverse Fourier transform to get \\(V_2(t)\\). Is there a way to do this *directly* in the time domain? 

Convolutional filtering in the time domain. You can think of a running average as a type of convolutional filter. It has effects in the spectral domain too.


So the filter can be defined by convolution with \\(I(t)\\) in the time domain
$$
V_2(t) = I(t) * V_1(t)
$$
or by multiplication via a transfer function in the Fourier domain
$$
S_2(f) = T(f) S_1(f) 
$$
and then you can get \\(V_2(t)\\) by
$$
V_2 \leftrightharpoons S_2.
$$


This plays a role in interpolation.

Linear interpolation. 

Sinc or "band-limited" interpolation.

## Nyquist sampling

* Role of compressed sensing to go "sub-Nyquist" sampling

## The Discrete Fourier transform (DFT)

### Units of the DFT 

The DFT only knows that it was fed a set of equally spaced samples (n).

We can express these units in cycles/sample by dividing frequencies in cycles/set by the number of evenly spaced samples/set  – watch how the units cancel out – to obtain the frequency range
 from 0 to almost 1 cycle/sample in 1/

 cycles/sample increments (1/1024th in our example), as shown in Figure

 cycles/sample,
So with typical setups the DFT’s output can always be referred to units of cycles/set and cycles/sample.   In the absence of additional information, however, these units cannot be directly related to linear space or to other physical units.

<https://www.strollswithmydog.com/units-of-discrete-fourier-transform/>

* Windowing / response / filters

