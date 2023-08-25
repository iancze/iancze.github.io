---
title: The Fourier Transform II
date: 2023-09-15
publishdate: 2023-08-25
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

Two examples are low-pass and high-pass filters.

{{< figure src="filter-pass.png" link="https://en.wikipedia.org/wiki/Filter_(signal_processing" caption="Examples of different filter transfer functions \\(T(f)\\). Credit: Wikipedia/SpinningSpark">}}

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

* [Youtube/SteveBrunton](https://www.youtube.com/watch?v=FcXZ28BX-xE&ab_channel=SteveBrunton) on The Sampling Theorem

Thus far we have been talking about continuous functions. As astrophysicists, though, we're frequently dealing with discrete data points, which are presumed to be *samples* of some unknown function. Maybe you're the one designing the experiment to capture these data points, or maybe you've just been handed some dataset.

{{< figure src="lines.png" caption="Say you are given a set of (noisless) samples that look like this. What do you think the function should look like in between the points? Credit: Ian Czekala" >}}

Concisely put, the *sampling theorem* states that under a certain condition, a function can be *completely* reconstructed from a set of discrete samples---without information loss. I.e., the set of discrete samples is *fully equivalent* to having access to the full set of function values. Today, this sampling theorem is known as the Nyquist-Shannon sampling theorem, the Whittaker–Nyquist–Shannon theorem, or simply "the sampling theorem."

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

### Compressed sensing 

You may have heard of "compressed sensing," which is one of major signal processing results of the last few decades. The idea is that you can reconstruct a functional form using far fewer samples than required for the Nyquist rate, using some dictionary of functional forms, or knowledege that the signal may be sparse. You can, indeed, perfectly reconstruct the signal through optimization using the \\(L_1\\) norm. If you don't want to make the assumption that your signal is sparse, though, it's a good idea to sample at the Nyquist rate.

## Extra: Fourier series

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

### Fourier transform of sine and cosine

Let's put together a number of the theorems that we've discussed to build up our understanding of what a Fourier series looks like in the Fourier domain.

In the previous lecture we introduced the Fourier transform theorem for a delta function located at the origin

$$
F(s) = \int_{-\infty}^{\infty} \delta(0) \exp (-i 2 \pi x s)\\,\mathrm{d}x = 1
$$
which is a constant.

We also introduced the shift theorem, which says
$$
f(x - a) \leftrightharpoons \exp(- 2 \pi i a s) F(s).
$$

We can couple these together and write a relationship
$$
\delta(x - a) \leftrightharpoons \exp(-2 \pi i a s).
$$

Now, let's see if we can use this Fourier pair to derive the Fourier pairs for cosine and sine. In your quantum physics or partial differential equations classes, you probably used the Euler identity to write cosine and sine like
$$
\cos 2 \pi a s = \frac{e^{i 2 \pi a s} + e^{-i 2 \pi a s}}{2}
$$
and
$$
\sin 2 \pi a s = \frac{e^{i 2 \pi a s} - e^{-i 2 \pi a s}}{2i}
$$

So we can do a bit of rearranging and arrive at

$$
\cos \pi x \leftrightharpoons \mathrm{even}(x) = \frac{1}{2}\delta \left (x +  \frac{1}{2} \right) + \frac{1}{2}\delta \left (x -  \frac{1}{2} \right)
$$
and 
$$
\sin \pi x \leftrightharpoons i\mathrm{odd}(x) = i \frac{1}{2}\delta \left (x +  \frac{1}{2}\right) - i \frac{1}{2}\delta \left (x -  \frac{1}{2}\right).
$$

where these symbols are the even and odd impulse pairs.

{{< figure src="even-odd-impulse.png" caption="The Fourier transform pairs of cosine and sine as the even and odd impulse pairs, respectively. Credit: Bracewell Fig 6.1">}}

Now we're well on our way to determining the Fourier spectrum of a Fourier series. The Fourier series is just a sum of sines and cosines at different frequencies. The Fourier transform is a linear operator, so we can just add together the contributions from each component in the Fourier domain
$$
a_0 + \sum_1^\infty (a_n \cos 2 \pi n f x + b_n \sin 2 \pi n f x)
$$

We arrive at the result that the spectrum of a Fourier series is a collection of delta functions whose locations and amplitudes correspond to the frequencies and values of the Fourier coefficients, respectively.

{{< figure src="line-spectra.png" link="https://www.tutorialspoint.com/what-is-fourier-spectrum-theory-and-example#:~:text=The%20graph%20plotted%20between%20the,spectrum%20of%20a%20periodic%20signal.&text=Amplitude%20Spectrum%20%E2%88%92%20The%20amplitude%20spectrum,of%20Fourier%20coefficients%20versus%20frequency" caption="The amplitude of the line spectra corresponding to a Fourier series. Credit: TutorialsPoint">}}

Now that we've derived the Fourier spectrum of a Fourier series, we can see at least two reasons why this represents an extreme situation of the Fourier transform:
1. The input waveform is strictly periodic
2. The input waveform is infinite in duration

As we talked about in the last lecture, these conditions are violated in the real world. If we limit the duration of the sine wave to a finite duration (say, by multiplication of a truncated Gaussian (because technically the Gaussian is also non-zero over an infinite domain), which we call a *window function*), then we see what happens in the Fourier domain: the delta functions are broadened by convolution with the Fourier transform of the window function.

{{< figure src="broadening-line.png" caption="Left: the dotted line represents a broad window function to eventually make the waveform finite in duration. Right: this has the effect of broadening the delta functions by convolution with the Fourier transform of the window function. Credit: Bracewell Fig 10.12" >}}


## The Discrete Fourier transform (DFT)

Now we'll talk about how we deal with samples of data. We'll stick with the same example that we're dealing with a function of time. But rather than \\(t\\), having units of seconds, we'll simply label each data point by an index \\(m\\) which takes on non-negative, integer values like \\(m = 0, 1, \ldots, N\\).

{{< figure src="DFT-samples.png" caption="Credit: Bracewell Fig 11.2" >}}

The forward discrete Fourier transform (DFT) is
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
m = 0, 1, \ldots, N - 1.
$$

So, at its most abstract, the DFT takes in a bunch of \\(N\\) samples spaced \\(\Delta x\\) apart and returns \\(N\\) samples corresponding to the Fourier components. The frequency of each component corresponds is given by \\(k/N\\) in units of "cycles per sampling interval."

I.e., so if we had \\(N = 8\\) samples, then the \\(k=3\\) frequency component returned from the DFT would be equal to \\(3/8\\) cycles per "the interval between samples."

On its own, the DFT doesn't provide any information about what type of variable \\(x\\) is or what the spacing is. But there is hope. We can make this concrete, we just have to be careful. Using our example of a time series with \\(N=8\\) samples \\(\\{f_m\\}\\), say we know that \\(\Delta x = 0.1\\) seconds,
$$
\Delta x = x_{m+1} -x_m
$$

The spacing in the frequency domain will be 1/8 cycles per 0.1 seconds, or 1.25 Hz.

### DFT as a matrix operation

Thus far we have just been talking about a "set" of samples. We can also think of the collection of data as a vector
$$
\mathbf{f} =
    \begin{bmatrix}
    f_0 \\\\
    f_1 \\\\
    \vdots \\\\
    f_{N-1} \\\\
    \end{bmatrix}
$$

and the frequency samples as a vector too

$$
\mathbf{F} =
\begin{bmatrix}
    F_0 \\\\
    F_1 \\\\
    \vdots \\\\
    F_{N-1} \\\\
    \end{bmatrix}.
$$

We've now mentioned that the Fourier transform is a linear operator a few times. Another way of demonstrating this is that we could write the DFT as a matrix multiplication
$$
\mathbf{F} = \mathbf{W} \mathbf{f}.
$$

If we look back at the definition of the DFT
$$
F_k = \sum_{m=0}^{N-1} f_m \exp \left ( - 2 \pi i \frac{m k}{N} \right)
$$
hopefully we can see how this might be cast in matrix form. If we define the quantity
$$
\omega = e^{- 2 \pi i / N}
$$
then we can write
$$
W = \begin{bmatrix}
1 & 1 & 1 & \ldots & 1 \\\\
1 & \omega & \omega^2 & \ldots & \omega^{N - 1} \\\\
1 & \omega^2 & \omega^4 & \ldots & \omega^{2(N-1)} \\\\
\vdots & \vdots & \vdots & \ddots & \vdots \\\\
1 & \omega^{N-1} & \omega^{2(N-1)} & \ldots & \omega^{(N-1)(N-1)}\\\\
\end{bmatrix}
$$

This formulation can be really useful if you're into forward modeling using linear models. Because we can write the DFT as a matrix multiplication, it can essentially be just another linear transformation to your linear model. Determining the "best-fit" parameters can still be done analytically. We'll talk about this a bit more when we get to the lecture on Bayesian inference.

{{< figure src="DFT-graphical.png" caption="The elements of the DFT matrix represented as samples of complex exponentials. Credit: Wikipedia/Glogger">}}

## Fast Fourier Transform (FFT)

* [Youtube/SteveBrunton](https://www.youtube.com/watch?v=E8HeD-MUrjY&ab_channel=SteveBrunton) on the Fast Fourier Transform

The Fast Fourier Transform or FFT is an algorithm (class of algorithms) for computing the discrete Fourier transform. Practically speaking, if you're going to perform a Fourier transform on discrete data with a computer, you will almost certainly use an FFT algorithm, so FFT and DFT end up being synonymous.

If we have a data array of length \\(N\\), the complexity of the DFT is \\(\mathcal{O}(N^2)\\), while the complexity of the FFT is only \\(\mathcal{O}(N \log N)\\). This can make a huge difference in computational time if N is large. Say if you have an array of \\(N = 4096\\) datapoints, then you could be looking at a factor of 1000 speedup.

The development and implementation of FFT packages turned the DFT from something that was too slow for many practical applications into a formidable analysis tool. This really enabled the widespread usage of the Fourier transform in signal manipulation and data analysis.  I don't think anyone would argue with you that strongly if you said that the FFT is the most important algorithm of the last century [Cornell's top ten algorithms of the 20th century](http://pi.math.cornell.edu/~ajt/presentations/TopTenAlgorithms.pdf).

There are many different FFT algorithms out there. The most popular one is the [Cooley-Tukey](https://en.wikipedia.org/wiki/Cooley%E2%80%93Tukey_FFT_algorithm) algorithm and relies on the factorization of a size \\(N\\) DFT matrix into \\(N_1\\) smaller DFTs of sizes \\(N_2\\) in a recursive manner. Therefore there is a (historical) preference towards array sizes that powers of \\(2\\). However, there are algorithms out that still give \\(\mathcal{O}(N \log N)\\) even for prime values of \\(N\\), so it's not much of a constraint in practice.

We don't have time to go into much more detail about the FFT algorithm here, but hopefully you can see from the structure of the \\(\mathbf{W}\\) matrix that there are plenty of opportunities to make the calculation more efficient by factoring and caching values.

Now, lets look at some code examples of how we might actually use the FFT in Python.

* [Jupyter Notebook Slides](FFT-lecture.slides.html)
