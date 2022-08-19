---
title: The Fourier Transform I
date: 2022-08-31
publishdate: 2022-08-19
draft: true
---

**Reference Materials for this lecture**:

* [The Fourier Transform and its Applications](https://catalog.libraries.psu.edu/catalog/2010095) by R. Bracewell
* [Interferometry and Synthesis in Radio Astronomy](https://catalog.libraries.psu.edu/catalog/20789467) by Thompson, Moran, and Swenson, particularly Appendix 2.1
* [Fourier Analysis and Imaging](https://catalog.libraries.psu.edu/catalog/34517505) by R. Bracewell

**Useful (and entertaining!) introductions to the topic**:

* Youtube: [3Blue1Brown](https://www.youtube.com/watch?v=spUNpyF58BY&ab_channel=3Blue1Brown)

## Complex Numbers

* [Wikipedia Reference](https://en.wikipedia.org/wiki/Complex_number#Polar_complex_plane)

A complex number \\(z\\) is one in the form of \\(z = a + b i\\), where \\(i\\) is the imaginary unit. The imaginary unit satisfies the equation
$$
i^2 = -1.
$$

So a complex number has both real (\\(a\\)) and imaginary (\\(b\\)) components to it. We can represent this as a plot on the Cartesian plane:

{{< figure src="complex-cartesian.png" link="https://en.wikipedia.org/wiki/File:Complex_number_illustration.svg" caption="Representation of a complex number on the Cartesian plane. Credit: Wolfkeeper" >}}

Alternatively, we can also represent a complex number on the polar plane, using an amplitude \\(r\\) and phase \\(\varphi\\):

{{< figure src="complex-polar.png" link="https://en.wikipedia.org/wiki/File:Complex_number_illustration_modarg.svg" caption="Representation of a complex number on the polar plane. Credit: Kan8eDie, based on Wolfkeeper" >}}

$$
z = r e^{i \varphi} = r (\cos \varphi + i \sin \varphi).
$$

This is closely related to Euler's formula
$$
e^{i x} = \cos x + i \sin x
$$
for a complex sinusoid. It's useful to keep in mind Euler's identity:
$$
e^{i \pi} + 1 = 0.
$$

It's possible to convert from the Cartesian form to polar form and vice-versa:
$$
r = |z| = \sqrt{a^2 + b^2}
$$
and
$$
\varphi = \arg(z) = \arg(a + bi)
$$
which is most easily carried out using the `arctan2` function, to avoid quadrant ambiguities.
$$
\varphi = \mathrm{arctan2}(b, a).
$$

You can go from polar back to Cartesian by writing
$$
z = r e^{i \varphi} = r (\cos \varphi + i \sin \varphi)
$$
and then doing
$$
a = \Re(z) = r \cos \varphi
$$
and
$$
b = \Im(z) = r \sin \varphi.
$$

## The Fourier Transform

References include a mix of

* Ch 2 of [The Fourier Transform and its Applications](https://catalog.libraries.psu.edu/catalog/2010095) by R. Bracewell
* Appendix 2.1 of [Interferometry and Synthesis in Radio Astronomy](https://catalog.libraries.psu.edu/catalog/20789467) by Thompson, Moran, and Swenson

First, we'll introduce the equations and then explain what's going on.

**Forward transform**:
$$
F(s) = \int_{-\infty}^{\infty} f(x) e^{-i 2 \pi x s}\\,\mathrm{d}x
$$
also called the "minus-\\(i\\)" transform.

**Inverse transform**:
$$
f(x) = \int_{-\infty}^{\infty} F(s) e^{i 2 \pi x s}\\,\mathrm{d}s
$$
also called the "plus-\\(i\\)" transform.

Note that there are alternate conventions for the Fourier transform pairs, which vary as to whether the \\(2 \pi\\) factor appears in the exponent or as a pre-factor. We prefer the notation we've provided here because we find it much easier to keep track of \\(2\pi\\) factors.

**Successive transforms**:

We can show that
$$
f(x) = \int_{-\infty}^{\infty} \left [ \int_{-\infty}^{\infty} f(x) e^{-i 2 \pi x s}\\,\mathrm{d}x \right ] e^{i 2 \pi x s}\\,\mathrm{d}s,
$$
i.e., successive transformations yield back the original function. We don't have time to walk through the proof, but it's available in [TMS Section A2.1](https://catalog.libraries.psu.edu/catalog/20789467).

Therefore, we have
$$
f(x) \leftrightharpoons F(s),
$$
where the \\(\leftrightharpoons\\) denotes a Fourier transform pair. Sometimes you might also see the notation \\(\leftrightarrow\\). Generally, if we have functions like \\(f\\) or \\(g\\), then we denote their Fourier transform pairs with captial letters (e.g., \\(F\\), \\(G\\))).

**How to think about the domains**
The Fourier transform maps domains from \\(x\\) to \\(s\\), where \\(s\\) has units of "cycles per unit of \\(x\\)". E.g., if \\(x\\) is in units of time (seconds), then \\(s\\) has units of cycles/second, commonly known as hertz. The FT can also be applied to spatial distance coordinates (e.g., \\(x\\) in meters) and spatial coordinates on the sky (e.g., \\(x\\) in arcseconds).

So let's say someone is playing a chord on a musical instrument. If \\(f(x)\\) represents the time-series of pressure values near your ear, then \\(F(s)\\) represents the notes that are being played (we would probably be most interested in something like \\(|F(s)|^2\\) for power as a function of frequency, like a spectrogram).

[Musical Spectrogram link](https://musiclab.chromeexperiments.com/spectrogram/)

### Conditions on existence

Given the strong evidence via physical systems that the Fourier transform of particular time-series exists (e.g., spectrums for waveforms, antenna radiation patterns), there are actually some mathematical functions whose Fourier transforms do not exist.

Physical possibility (in physical systems) is a sufficient condition for the existence of its transform.

Sometimes, though, we consider waveforms like

* \\(\sin(t)\\): harmonic wave
* \\(H(t)\\): Heaviside step
* \\(\delta(t)\\): impulse

Strictly speaking, none of these functions has a Fourier transform. None of them is actually physically possible, either, because a waveform \\(\sin(t)\\) would have need to been switched on an infinite time ago, a step function would need to be maintained for an infinite time, and an impulse would need to be infinitely large for an infinitely short time.

What do we mean that they do not have Fourier transforms?
$$
F(s) = \int_{-\infty}^{\infty} f(x) e^{-i 2 \pi x s}\\,\mathrm{d}x
$$
does not converge for all values of \\(s\\).

So, what are the conditions for the existence of Fourier transforms.

1. The integral of \\(|f(x)|\\) from \\(-\infty\\) to \\(\infty\\) exists.
2. Any discontinuities in \\(f(x)\\) are finite.

In a physical circumstance, these conditions would be violated when there is infinite energy.

### Transforms in the limit

Consider a periodic function whose transform would strictly not exist, such as \\(P(x) = \sin(x) \\), since
$$
\int_{-\infty}^\infty |P(x)|\\,\mathrm{d}x
$$
would not converge.

If we modified this slightly, though, by multiplication with a very broad Gaussian envelope \\(\exp(-\alpha x^2)\\), where \\(\alpha\\) is a small positive number, then this modified version may have a transform. As we let \\(\alpha \rightarrow 0\\), then we approach \\(P\\) in the limit.
$$
\int_{-\infty}^\infty |e^{-\alpha x^2} P(x)|\\,\mathrm{d}x
$$

The transform may not exist for all \\(s\\), though, but we can still be quite productive with the sequence of transforms that do exist as we approach this limit. Therefore, we can practically use the Fourier transform for all physical systems that we might consider. We'll revisit this in more detail with the sampling theorem, line spectra, and a Fourier series.

### Oddness, Evenness, and Complex Conjugates

{{< figure src="even-odd.png" caption="An even function \\(E(x)\\) and an odd function \\(O(x)\\), followed by their sum. Credit: The Fourier Transform and Its Applications, Bracewell, Figs 2.2 and 2.3." >}}

**Even functions** have
$$
E(-x) = E(x)
$$
and are symmetrical.

**Odd functions** have
$$
O(-x) = -O(x)
$$
and are antisymmetrical.

The sum of even and odd functions is, in general, neither even nor odd.

Any function \\(f(x)\\) can be split unambiguously into it's odd and even parts, though, where
$$
E(x) = \frac{1}{2} \left [ f(x) + f(-x) \right]
$$
and
$$
O(x) = \frac{1}{2} \left [ f(x) - f(-x) \right]
$$
and so we have
$$
f(x) = E(x) + O(x),
$$
where both \\(E\\) and \\(O\\) are in general complex-valued.

Evenness and oddness are very useful because we can these definitions to re-write the Fourier transform as
$$
F(s) = 2 \int_0^\infty E(x) \cos(2 \pi x s)\\,\mathrm{d}x - 2 i \int_0^\infty O(x) \sin (2 \pi x s)\\,\mathrm{d}x.
$$

We see that

1. If a function is even, its transform is even
2. If a function is odd, its transform is odd

and other results summarized by Bracewell in Chapter 2:

{{< figure src="ft-relationships.png" caption="If a function has the characteristics in the left column, then its Fourier transform has the characteristics in the right column. Credit: Bracewell Ch. 2" >}}

These relationships are extremely useful for quickly ascertaining the basic nature of the Fourier transform of any function you may encounter.

**Complex conjugates**

If we have the complex conjugate of function \\(f(x)\\), denoted by \\(f^*(x)\\), then we have

$$
f^{\*}(x) \leftrightharpoons F^{\*}(-s)
$$

## Transforms of some simple functions

Let's practice by taking the Fourier transforms of some functions.

**TODO**: Write out derivation of boxcar function and sinc.

**TODO**: Cover the Gaussian and completing the square.

## Convolution

The convolution of two functions \\(f(x)\\) and \\(g(x)\\) is defined as
$$
\int_{-\infty}^\infty f(u) g(x - u)\\,\mathrm{d}u
$$
and is frequently written using the \\(*\\) symbol. The convolution produces a new function, so we have
$$
h(x) = f(x) * g(x).
$$

Convolution as a process is very useful to think of graphically

{{< figure src="convolution-1.png" caption="A graphical representation of the convolution of two functions \\(f(x)\\) and \\(g(x)\\). The \\(g\\) function is flipped, shifted to \\(x\\), and then multiplied against \\(f\\). The value of the convolution \\(h(x)\\) is given by the integral of the multiplied product. Credit: Bracewell Ch. 3" >}}

{{< figure src="convolution-2.png" caption="In general, convolution by most functions (e.g., boxcars, Gaussians, etc...) results in a smoothing out of high-frequency structure. Credit: Bracewell Ch. 3" >}}

{{< figure src="convolution-3.png" caption="Convolution can also be thought of as a superposition of characteristic contributions. I.e., the final function \\(h(x)\\) has grabbed value from nearby regions of \\(f(x)\\), modulated by the envelope of \\(g(x)\\). This paradigm is very useful for understanding interpolation, smoothing, and kernel density estimation (KDE). Credit: Bracewell Ch. 3 ">}}

Convolution is commutative

$$
f \* g = g \* f
$$

and associative

$$
f \* (g \* h) = (f \* g) \* h
$$

and distributive over addition

$$
f \* (g + h) = f \* g + f \* h.
$$

It is a [linear operator](https://undergroundmathematics.org/glossary/linear-operator#:~:text=A%20function%20f%20is%20called,x%20and%20all%20constants%20c.), just like the Fourier transform.

## Fourier transform theories: similarity, convolution, multiplication

* Linearity
* Note how FFTconvolve using python speeds up. Depends on the scaling, and size of convolutional kernel.

## Common FT transform pairs

## Sampling and Nyquist sampling theorem

* The impluse symbol \\(\delta\\).
* Sifting property TMS A2.11
$$
f(x) = \int_{-\infty}^{\infty} f(x^\prime) \delta(x^\prime - x)\\,\mathrm{d}x^\prime.
$$

## Discrete Fourier Transform

* Windowing + sidelobes (filters)
