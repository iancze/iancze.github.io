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

Then you can get \\(V_2(t)\\) by
$$
V_2 \leftrightharpoons S_2.
$$


TODO: the square showing the relationship between these quantities.

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

Thus far we have been talking about continuous functions. In the world of data, however, we're frequently dealing with discrete data points, which are presumed to be *samples* of some unknown function. Maybe you're the one designing the experiment to capture these data points, or maybe you've just been handed some dataset.

Concisely put, the *sampling theorem* states that under a certain condition, a function can be *completely* reconstructed from a set of discrete samples, without information loss. I.e., the set of discrete samples is fully equivalent to having access to the full set of function values. Today, this sampling theorem is most commonly known as the Nyquist-Shannon sampling theorem, also the Whittaker–Nyquist–Shannon theorem, or simply "the sampling theorem."

TODO: [Draw a bunch of data points, and sketch possible functions going through them, some oversampled and some undersampled].

If we were to try to use these data points to actually reconstruct a function, then what sort of constraint would we need to impose on the function? We'd want to place some constraint on its spectrum, i.e., that there are no higher frequency components oscillating around faster than our sampling points.

TODO: [Could split the function into higher and lower frequency components too, since linear]

### Aliasing

{{< figure src="sine-wave.png" caption="Given a set of discrete samples, there are a multitude of sine waves that , when the frequency of one sine wave is above 1/2 the sample rate. Credit: Wikipedia Pluke" >}}

Stroboscopic effect, also called rolling shutter effect. 

* [Youtube/SmarterEveryDay](https://www.youtube.com/watch?v=dNVtMmLlnoE&ab_channel=SmarterEveryDay).
* [Stationary helicopter](https://www.youtube.com/watch?v=smDpCsVVgPA&ab_channel=Edyourself).
* Exoplanet transits! [Dawson and Fabrycky 2010](https://ui.adsabs.harvard.edu/abs/2010ApJ...722..937D/abstract) find a shorter period for the transiting exoplanet 55 Cnc e, previously confounded by the timing of the RV observations.

{{< figure src="band-limited.png" caption="Credit: Bracewell Fig 10.2" >}}

When this criterion is met relative to the sampling spacing, the function is called *band-limited*. This comes about because of the band-pass in the frequency domain being limited.

If \\(s_c\\) is the critical frequency defining the band-limited nature of the signal, then so long as the function is sampled at equal intervals not exceeding \\(1/2 s_c\\) then the function is properly sampled.

Let's derive this result and use our understanding of the sampling/replicating theorem.

Recall that the "shah" function is an infinite series of delta functions spaced a unit dimension apart, and that it is its own Fourier transform
$$
\mathrm{shah}(x) \leftrightharpoons \mathrm{shah}(s).
$$

We will use \\(f(x) \mathrm{shah}(x)\\) to represent sampling of the function. As we had talked about in the last lecture, this had consequences for the Fourier transform
$$
f(x) \mathrm{shah}(x) \leftrightharpoons F(s) * \mathrm{shah}(s).
$$
If the delta functions of the shah get closer in the \\(x\\) domain, then they spread out in the Fourier domain, and vice-versa.


We can adjust the spacing of the samples by dilating or shrinking the shah function \\(\mathrm{shah}(x/\tau)\\). According to the similarity theorem, this has an effect in the frequency domain
$$
f(x) \mathrm{shah}(x/\tau) \leftrightharpoons F(s) * \tau \mathrm{shah}(\tau s).
$$


{{< figure src="sampling-demonstration.png" caption="Credit: Bracewell Fig 10.3" >}}

So lets come back and make a quantitative statement of the sampling theorem (Bracewell):

*A function whose Fourier transform is zero for \\(|s| > s_c\\) is fully specified by values spaced at equal intervals not exceeding \\(1/2 s_c\\).*



### Convolutional kernels

Now let's talk about how we would actually reconstruct the continuous function from a set of samples.

Multiply by boxcar in Fourier domain to completely truncate higher order terms. This is equivalent to sinc interpolation in the time domain.

---

Let's circle back to our discussion on transfer functions and convolutional kernels

A side note here that this discussion has huge implications for interpolation---creating a continuous representation from discrete points---which can also be thought of as a convolution. For example, whether we do nearest-neighbor, linear, or cubic, or a "band-limited" interpolation,

TOD: [Draw a bunch of points and then do linear or cubic interpolation]

will play an important role in the spectral properties of the resulting waveform. This is because each of these types of interpolation can be cast as convolution with a different type of kernel (tophat, triangle, sinc, etc), which have

This plays a role in interpolation.

Linear interpolation.

Sinc or "band-limited" interpolation.


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