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
$$
F(s) = \int_{-\infty}^{\infty} f(x) e^{-i 2 \pi x s}\\,\mathrm{d}x
$$

* Note that because \\(x\\) and \\(s\\) appear in the argument of the exponential, their product must be dimensionless. This means that they will also have inverse units, e.g., "seconds" and "cycles per second."
* Introduced convolution, impluse symbol, and theorems

## Where are we headed *today*?

* Finish up Fourier transform theorems
* Nyquist sampling theorem
* Discrete Fourier Transform

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

This is an *extremely* useful theorem. At least in my career, this, and concepts related to sampling, have been the ones I have used the most often. You may have already used this theorem (numerically) if you've ever carried out a convolution operation using [scipy.signal.fftconvolve](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.fftconvolve.html) in Python, which can be dramatically faster than directly implementing the convolution, at least for certain array sizes.

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

and it has the Fourier transform
$$
f(x) \* f(x) \leftrightharpoons |F(s)|^2.
$$

Thus, the power spectrum is the Fourier transform of the autocorrelation function. It can also be computed directly by taking the "mod-squared" of \\(F(s)\\),

$$
|F|^2 = F F^*.
$$

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
V_1(t) = A \cos (2 \pi f t)
$$ which is a single-valued function of time. You can think of this as a voltage time-series or another physical quantity. By definition, the waveform is real.

Let's put on our electrical/acoustical/mechanical engineering hats for a moment and consider that a *filter* is a physical system with an input an and output, e.g., something that is transmitting vibrations or oscillations, like our waveform.

{{< figure src="filter.png" caption="How a filter changes the amplitude and phase of an waveform. Credit: Ian Czekala" >}}

If we feed our waveform into a *linear* filter, we get output
$$
V_2(t) = B \cos (2 \pi f t + \phi).
$$

The output is still a waveform, but its amplitude and its phase have changed. These changes are likely frequency dependent, too.

We can specify the filter by a frequency-dependent quantity \\(T(f)\\) called the *transfer function*. It is a complex-valued function (having both an amplitude and a phase) and is given by
$$
T(f) = \frac{B}{A}e^{i \phi}.
$$

Interesting, perhaps, but maybe not immediately obviously useful. Let's introduce the *spectrum* and then circle back to the transfer function.

### Obtaining \\(V_2\\) using the frequency domain

The *spectrum* of the waveform is the Fourier transform of \\(V(t)\\), which we'll call \\(S(f)\\) in this section. We've broken slightly from our \\(f \leftrightharpoons F\\) notation, but \\(V \leftrightharpoons S\\) is a classic in the signal processing and electrical engineering fields, so we'll at least build familiarity with it in this example.

The "spectrum" here is just the Fourier transform quantity, it can definitely be complex-valued. 

The "spectra" that we typically talk about in astrophysics are measurements of the electromagnetic spectrum---you've probably never come across as one that's complex-valued, right? What's going on here?

Consider the *units* of a flux measurement \\(F_\nu\\) of the electromagnetic spectrum. In lecture 1, we covered that the cgs units of flux are
$$
\mathrm{ergs}\\;\mathrm{s}^{-1}\\;\mathrm{cm}^{-2}\\;\mathrm{Hz}^{-1}.
$$

The clue is in the \\(\mathrm{ergs}\\;\mathrm{s}^{-1}\\) part, which we could also write in terms of "watts" if we wanted to be strictly S.I. about it. When we are measuring the electromagnetic spectrum, we are actually measuring the **power** spectral density, \\(|F(\nu)|^2\\). The absolute squared means the quantity \\(|F(\nu)|^2\\) is real-valued, and is the reason why you never hear about measurements of the electromagnetic spectrum containing imaginary values!

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

Once you have \\(S_2\\), then you can get \\(V_2(t)\\) from
$$
V_2 \leftrightharpoons S_2.
$$

### Obtaining \\(V_2\\) using the time domain

Now let's think of digital signal processing, where you wanted to practically apply a filter to some waveform to produce a new waveform. As we just outlined, you could acquire \\(V_1(t)\\), Fourier transform it to access its spectrum \\(S_1(f)\\), multiply by the transfer function \\(T_(f)\\), and then do the inverse Fourier transform to get \\(V_2(t)\\). Is there a way to do this *directly* in the time domain? What if you don't have the complete waveform all at once?

The answer is provided by the convolution theorem for Fourier transforms. Since the transfer function is applied via a multiplication in the Fourier domain, we could equivalently carry out the same operation by a convolution in the time domain.

The convolutional kernel would be
$$
I(t) \leftrightharpoons T(f)
$$
and we'd have 
$$
V_2(t) = I(t) * V_1(t).
$$

To summarize, 
{{< figure src="square.png" caption="Credit Bracewell, Chapter 9.">}}


### Determining \\(I(t)\\)

If you had some system already in place, and you wanted to determine \\(I(t)\\) experimentally, what is one way you could do it? What waveform could you send the system?

One simple option would be to send 
$$
V_1(t) = \delta(t),
$$
then
$$
V_2(t) = I(t) * \delta(t) = I(t).
$$

## Nyquist-Shannon sampling

Thus far we have been talking about continuous functions. As astrophysicists, though, we're frequently dealing with discrete data points, which are presumed to be *samples* of some unknown function. Maybe you're the one designing the experiment to capture these data points, or maybe you've just been handed some dataset.

TODO: [Draw a bunch of data points, and sketch possible functions going through them, some oversampled and some undersampled].

Concisely put, the *sampling theorem* states that under a certain condition, a function can be *completely* reconstructed from a set of discrete samples---without information loss. I.e., the set of discrete samples is *fully equivalent* to having access to the full set of function values. Today, this sampling theorem is known as the Nyquist-Shannon sampling theorem, the Whittaker–Nyquist–Shannon theorem, or simply "the sampling theorem."

TODO: [Sketch possible functions going through them, some oversampled and some undersampled].

If we were to try to use these data points to actually reconstruct a function, then what sort of constraint would we need to impose on the function? We'd want to place some constraint on its spectrum, i.e., that there are no higher frequency components oscillating around faster than our sampling points.

Before we dive into the derivation of the sampling theorem, let's first take another look at what can go wrong when you undersample a time series. 

### Aliasing

{{< figure src="sine-wave.png" caption="If the true signal is given by the solid line, but we *undersample* it, then the sine-wave we naively reconstruct from the samples would have the wrong frequency. Here we would say that the higher frequency signal has been aliased into the lower frequency range. Credit: Wikipedia Pluke" >}}

This is also called the Stroboscopic effect. There are some nice examples online:

* [Youtube/SmarterEveryDay](https://www.youtube.com/watch?v=dNVtMmLlnoE&ab_channel=SmarterEveryDay).
* [Stationary helicopter](https://www.youtube.com/watch?v=smDpCsVVgPA&ab_channel=Edyourself).
* Exoplanet transits! [Dawson and Fabrycky 2010](https://ui.adsabs.harvard.edu/abs/2010ApJ...722..937D/abstract) find a shorter period for the exoplanet 55 Cnc e, previously confounded by the timing of the RV observations.

### Derivation of sampling theorem

Now, let's use our understanding of Fourier transforms and the sampling/replicating function to develop a precise formulation of the sampling theorem.

Recall that the "shah" function is an infinite series of delta functions spaced a unit dimension apart, and that it is its own Fourier transform
$$
\mathrm{shah}(x) \leftrightharpoons \mathrm{shah}(s).
$$

Via the similarity theorem, if the delta functions of the shah get closer in the \\(x\\) domain, then they spread out in the Fourier domain, and vice-versa.

We can adjust the spacing of the samples by dilating or shrinking the shah function by some factor. Here, we'll write this as the sampling interval \\(\Delta x = \tau\\) or the sampling frequency \\(1/\tau\\). 

According to the similarity theorem, adjusting the sampling frequency in the \\(x\\) domain has the following effect in the frequency domain
$$
\mathrm{shah}(x/\tau) \leftrightharpoons \tau \mathrm{shah}(\tau s).
$$

For example, if \\(\tau = 0.2\\), then we have

{{< figure src="panel-b.png" caption="The shah function is its own Fourier transform. Via the similarity theorem, if we compress the shah function in the time domain (left), we expand it in the Fourier domain (right). Credit: Bracewell, Fig 10.3">}}

Now let's consider a function and its Fourier transform

{{< figure src="function.png" caption="A generic function (left) and its Fourier transform (right). We say that this function is 'band-limited' because its Fourier transform is 0 for all frequencies below some *cutoff frequency* \\(|s| < s_c\\). Credit: Bracewell Fig 10.2" >}}

As before, we will use multiplication by the shah function to represent sampling of the function. 

{{< figure src="sampled-function.png" caption="Left: the sampled version of \\(f\\), which has the Fourier transform on the right. So long as the sampling frequency exceeds twice the cutoff frequency, the Fourier transform 'islands' do not overlap (top two rows). Credit: Bracewell Fig 10.3" >}}

The non-overlappingness of the 'islands' is the key to properly sampling a function, and we'll see why in a moment when we talk about reconstruction. But first, let's make a quantitative statement of the sampling theorem (Bracewell):

If \\(s_c\\) is the cutoff frequency defining the band-limited nature of the signal, then so long as the function is sampled at equal intervals not exceeding \\(\Delta x = 1/(2 s_c)\\) then the function is properly sampled, i.e.
$$
\frac{1}{\tau} \geq 2 s_c.
$$


### Restoration of signal kernels

Now let's talk about how we would actually reconstruct the continuous function from a set of samples. Let's re-examine our plot of the Fourier domain

{{< figure src="reconstruction.png" caption="Credit: Bracewell Fig 10.3" >}}

The "function" on the left is technically not the same (continuous) function that we started with, it is a discrete representation of it. We did just say, though, that if the function was band-limited, then these samples contained all of the same information as if we had access to the full function. So how do we go from these samples back to the full function?

Let's look at the Fourier side of this plot and compare it to the original Fourier side. The main difference is that *this* Fourier plot has repeating 'islands' at progressively higher frequencies, essentially to infinity. How can we get rid of these higher frequency islands?

The answer is to multiply by a boxcar function in Fourier domain, completely truncating these higher order terms. Then, we can do the inverse Fourier transform and recover the original, continuous function.

What is the analogous operation for the time-domain? This is the same thing as we discussed with the transfer function. Since it was a multiplication in the Fourier domain, it is a convolution in the time domain. And the convolutional kernel is the Fourier transform of the boxcar, which is a sinc function.

So, to *exactly* reconstruct a band-limited function from a set of samples, we do sinc-interpolation. 

{{< figure src="sinc-interpolation.png" link="https://www.dsprelated.com/freebooks/pasp/Windowed_Sinc_Interpolation.html" caption="Credit: DSP related." >}}

### Undersampling and aliasing 

If we didn't sample the function at a sufficiently high rate, then we would have overlapping islands. Essentially, the higher frequency components of the Fourier islands are "folded-over" back into the range of frequencies *we thought* was band-limited, resulting in a corrupted signal.

In an alias, a higher frequency signal is masquerading as a lower-frequency signal.

{{< figure src="aliased.png" caption="Credit: Bracewell Fig 10.3" >}}

### Convolutional kernels

Now that we've developed our understanding of band-limited functions, sampling, and restoration, let's circle back to our discussion on transfer functions and convolutional kernels.

We just said that in order to restore a continuous function from a set of samples, we needed to do sinc-interpolation. What happens if we do one of the more commonly used forms of interpolation, like nearest neighbor or linear?

{{< figure src="nearest-neighbor-vs-linear.png" link="https://octave.sourceforge.io/octave/function/interp1.html" caption="Credit: Octave" >}}

We can cast each of these as convolution with a different kind of kernel.

{{< figure src="interpolation-kernels.png" link="https://www.researchgate.net/figure/Interpolation-kernels-in-one-dimension-The-width-of-the-support-is-shown-below_fig3_10624120" caption="Credit Jeffrey Tsao." >}}

The nearest neighbor kernel will have a corresponding Fourier transform of a sinc and the linear kernel will have a \\(\mathrm{sinc}^2\\). These functions all have non-zero values above the cutoff frequency. Of these, *only* the sinc function has zero amplitude beyond the cutoff frequency.

So reconstruction of a function using linear interpolation, for example, will introduce higher-frequency features, like these sharp transitions around each data point. Depending on what you're doing the interpolation for, sometimes this matters, sometimes it doesn't. Later on, when we talk about "gridding" of visibility data from radio interferometers, the type of interpolation used will have a big impact on the dynamic range of the resulting images.

**Q**: Why is sinc interpolation generally *not* used in practice? Because the kernel size is large/infinite, you're actually using all of the data points in each and every interpolation. Compare that to linear interpolation, which only uses the two nearest points.


### Compressed sensing 

You may have heard of "compressed sensing," which is one of major signal processing results of the last few decades. The idea is that you can reconstruct a functional form using far fewer samples than required for the Nyquist rate, using some dictionary of functional forms, or knowledege that the signal may be sparse. You can, indeed, perfectly reconstruct the signal through optimization using the \\(L_1\\) norm. If you don't want to make the assumption that your signal is sparse, though, it's a good idea to sample at the Nyquist rate.

## Fourier series

You probably first encountered Fourier series as part of your calculus course and later on as part of a partial differential equations course. 
Say we have some periodic function \\(g(x)\\), then the Fourier series associated with this is
$$
a_0 + \sum_1^\infty (a_n \cos 2 \pi n f x + b_n \sin 2 \pi n f x)
$$
where the Fourier coefficients are determined by
$$
a_0 = \frac{1}{T} \int_{-T/2}^{T/2} g(x) \\,\mathrm{d}x
$$

$$
a_n = \frac{1}{T} \int_{-T/2}^{T/2} g(x) \cos 2 \pi n f x \\,\mathrm{d}x
$$

$$
b_n = \frac{1}{T} \int_{-T/2}^{T/2} g(x) \sin 2 \pi n f x \\,\mathrm{d}x
$$

i.e., we've projected the function onto its basis set of sines and cosines.

Already, I'm sure you are starting to see the close connection with what we've discussed of the Fourier transform. Traditionally, Fourier series are used as a jumping off point for the discussion of the Fourier transform.

In the last lecture, however, we signaled our intention to take the opposite approach, whereby we skipped over Fourier series and *started* with the idea that Fourier transforms exist because we observe physical systems which exhibit their behavior. Now, let's unify the discussion and demonstrate the Fourier series as an extreme situation of the Fourier transform.

Let's revisit the FT of a sine wave example, presented as two delta functions. Or FT of a cosine wave.

{{< figure src="cosine.png" caption="Credit: Bracewell, appendix" >}}
{{< figure src="sine.png" caption="Credit: Bracewell, appendix" >}}

TODO: Figures: hand draw, then Adobe.

Strictly periodic. Convolution with some function with Shah to create an infinitely periodic function.

This is as though you've taken the same transform of F and then multiplied it by the Shah to create discrete samples of the spectrum, called line spectra. Because 

Line spectra.

## The Discrete Fourier transform (DFT)

Now we'll talk about how we deal with samples of data. We'll stick with the same example that we're dealing with a function of time. But rather than \\(t\\), having units of seconds, we'll simply label each data point by an index \\(m\\) which takes on non-negative, integer values like \\(m = 0, 1, \ldots, N\\). The forward discrete Fourier transform (DFT) is 
$$
F_k = \sum_{m=0}^{N-1} f_m \exp \left ( - 2 \pi i \frac{m k}{N} \right)
$$
and we could compute \\(F_k\\) for \\(k = 0, 1, \ldots, N-1\\).

Here, the discrete index variable \\(k\\) has replaced the continuous-frequency variable \\(s\\), just like \\(m\\) replaced the continuous-time variable \\(t\\).

The inverse discrete Fourier transform is
$$
f_m = \frac{1}{N} \sum_{k=0}^{N -1} F_k \exp \left ( 2 \pi i \frac{m k}{N}\right).
$$
Like the continuous-Fourier transform, one of the differences from the forward is the \\(+i\\) in the exponential. The other is the inclusion of the normalization pre-factor.

Note: depending on whom you talk to, you'll see a wide variety of conventions as to where the normalization prefactor goes and where the \\(2 \pi\\) lives. The convention presented here is the same one used by the [Python/NumPy package](https://numpy.org/doc/stable/reference/routines.fft.html#module-numpy.fft) and the [Julia/AbstractFFTs.jl](https://juliamath.github.io/AbstractFFTs.jl/stable/api/#Public-Interface) package, so it *should* be the one you encounter most frequently.

Like the continuous Fourier transform, if you take the DFT of a set of samples and then take the iDFT of that, you will end up with the original set of samples.

### Units of the DFT 

The DFT only knows/assumes that it was fed a set of equally spaced samples
$$
f_m = f(x_m)
$$
where 
$$
m = 0, 1, \ldots, N.
$$

There isn't any information about what type of variable \\(x\\) is or what the spacing 
$$
\Delta x = x_{m+1} -x_m 
$$
is.

So, at its most abstract, the DFT takes in a bunch of \\(N\\) samples spaced \\(\Delta x\\) apart and returns \\(N\\) samples corresponding to the Fourier components. The spacing \\(\Delta k\\) of the frequency axis is in cycles/sample.

To make this concrete, let's say we have a time series of samples \\(\\{f_m\\}\\) and we know that \\(\Delta x = 0.1\\) seconds.

The spacing in the frequency domain will be 1/()

What 
To make th

See also [fftfreq](https://numpy.org/doc/stable/reference/generated/numpy.fft.fftfreq.html#numpy.fft.fftfreq).

We can express these units in cycles/sample by dividing frequencies in cycles/set by the number of evenly spaced samples/set  – watch how the units cancel out – to obtain the frequency range
 from 0 to almost 1 cycle/sample in 1/

 cycles/sample increments (1/1024th in our example), as shown in Figure

 cycles/sample,
So with typical setups the DFT’s output can always be referred to units of cycles/set and cycles/sample.   In the absence of additional information, however, these units cannot be directly related to linear space or to other physical units.

<https://www.strollswithmydog.com/units-of-discrete-fourier-transform/>


* What have we assumed about the periodic nature of the samples?

* Windowing / response / filters

Next week we'll cover the FFT, and we'll talk more about how to use the FFT to take transforms of functions when we need to care about the units.

We'll also discuss how one "packs" the array of samples.


## Figure list:

* Filter in/out
* 