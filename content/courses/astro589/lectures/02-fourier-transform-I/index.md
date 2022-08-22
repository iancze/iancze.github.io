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

Strictly speaking, none of these functions has a Fourier transform. What do we mean that they do not have Fourier transforms?
$$
F(s) = \int_{-\infty}^{\infty} f(x) e^{-i 2 \pi x s}\\,\mathrm{d}x
$$
does not converge for all values of \\(s\\). Consider \\(\sin(t)\\) integrated from \\((-\infty, \infty)\\)... it'll just keep oscillating about 0.

None of these waveforms are actually physically possible, though, because a waveform \\(\sin(t)\\) would have need to been switched on an infinite time ago, a step function would need to be maintained for an infinite time, and an impulse would need to be infinitely large for an infinitely short time.

So, what are the conditions for the existence of Fourier transforms.

1. The integral of \\(|f(x)|\\) from \\(-\infty\\) to \\(\infty\\) exists.
2. Any discontinuities in \\(f(x)\\) are finite.

In a physical circumstance, these conditions would be violated when there is infinite energy.

### Transforms in the limit

Though we just outlined functional forms whose Fourier transforms do not *strictly* exist, we can find a way to think such that the transforms of these functions do exist in a practical sense. This is by considering them *in the limit*.

Consider a periodic function whose transform would strictly not exist, such as \\(P(x) = \sin(x) \\), since
$$
\int_{-\infty}^\infty |P(x)|\\,\mathrm{d}x
$$
would not converge.

We could modify this function ever so slightly by multiplication with a *very* broad Gaussian envelope \\(\exp(-\alpha x^2)\\), where \\(\alpha\\) is a small positive number, then this modified version may have a transform. As we let \\(\alpha \rightarrow 0\\), then we approach \\(P\\) in the limit.
$$
\int_{-\infty}^\infty |e^{-\alpha x^2} P(x)|\\,\mathrm{d}x
$$

As \\(\alpha \rightarrow 0\\) the transform may still not exist for all \\(s\\), however, we can still be quite productive with the sequence of transforms that do exist as we approach this limit. Therefore, we can practically use the Fourier transform for all physical systems that we might consider. We'll revisit this in more detail when we talk about Fourier series, line spectra, and the sampling theorem.

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

These relationships are extremely useful for quickly ascertaining the basic nature of the Fourier transform of any function you may encounter, as well as planning how to numerically compute transforms using `fft` or `rfft` packages.

**Complex conjugates**

If we have the complex conjugate of function \\(f(x)\\), denoted by \\(f^*(x)\\), then we have

$$
f^{\*}(x) \leftrightharpoons F^{\*}(-s)
$$

## Transforms of some simple functions

Let's practice by taking the Fourier transforms of some functions.

### Boxcar

The rectangle or "boxcar" function is
{{< figure src="rectangle.png" caption="The rectangle, or boxcar function. Credit: Bracewell Ch. 3" >}}

Let's calculate the Fourier transform. The function itself is simple, so this is mainly an exercise in choosing the right limits
$$
F(s) = \int_{-\infty}^{\infty} f(x) e^{-i 2 \pi x s}\\,\mathrm{d}x
$$

$$
F(s) = \int_{-1/2}^{1/2} e^{-i 2 \pi x s}\\,\mathrm{d}x
$$

We can use Euler's formula to write
$$
F(s) = \int_{-1/2}^{1/2} \cos(2 \pi x s) + i \sin(2 \pi x s) \\,\mathrm{d}x
$$
and visually seen that the \\(\sin\\) term would eventually cancel itself out, or we could have relied upon the fact that we know \\(\Pi(x)\\) is an even function (\\(O(x) = 0\\)) and used
$$
F(s) = 2 \int_0^\infty E(x) \cos(2 \pi x s)\\,\mathrm{d}x - 2 i \int_0^\infty O(x) \sin (2 \pi x s)\\,\mathrm{d}x.
$$
to yield
$$
F(s) = 2 \int_0^{1/2} \cos(2 \pi x s)\\,\mathrm{d}x
$$

$$
F(s) = 2 \Big |_0^{1/2} \frac{\sin 2 \pi x s}{2 \pi s} = \frac{\sin \pi s}{\pi s} = \mathrm{sinc}(s)
$$

Note that we (and Bracewell) define
$$
\mathrm{sinc}(s) = \frac{\sin \pi s}{\pi s}
$$
this is called the normalized sinc function, and (IMO) is the most useful because of it's nice Fourier pair relationships. There is also the "unnormalized" sinc function, which doesn't have the factors of \\(\pi\\) in it, but we won't use that in this course. The **normalized sinc function** has the properties that

* its peak is equal to 1 \\(\mathrm{sinc}(0) = 1\\)
* its "nulls" are located at non-zero integer values of \\(n\\) for \\(\mathrm{sinc}(n)\\)
* its integral from \\(-\infty,\infty\\) is equal to 1

So we have the Fourier pair:
$$
\Pi(x) \leftrightharpoons \mathrm{sinc}(s)
$$
The unit rectangle (or boxcar) function has the Fourier transform pair of a normalized sinc function.

### Gaussian

How about the Fourier transform of a Gaussian function?
$$
f(x) = \exp \left ( -\frac{x^2}{2 a ^2} \right )
$$

$$
F(s) = \int_{-\infty}^{\infty} \exp \left ( -\frac{x^2}{2 a ^2} \right ) \exp (-i 2 \pi x s)\\,\mathrm{d}x
$$

$$
F(s) = \int_{-\infty}^{\infty} \exp \left ( -\frac{x^2}{2 a ^2} -i 2 \pi x s \right )\\,\mathrm{d}x
$$

Usually, when I see something like this, my standard approach is to start browsing books of integrals like [Gradshteyn and Ryzhik](https://en.wikipedia.org/wiki/Gradshteyn_and_Ryzhik) for ideas about how I might rearrange the integrand and successfully evaluate the integral. I'd usually just go for this. But, in this case, we can actually do something using a trick you probably learned in jr. high school, called [completing the square](https://www.mathsisfun.com/algebra/completing-square.html).

Ok, so we've got terms in the exponent with \\(x^2\\) and \\(x\\), and an equation that looks like
$$
a x^2 + bx + c = 0
$$
which we want to turn into something that looks like
$$
a(x + d)^2 + e = 0.
$$

The answer is that
$$
d = \frac{b}{2a}
$$
and
$$
e = c - \frac{b^2}{4 a}
$$
such that our rearranged exponent becomes
$$
-\left [ \frac{x^2}{2 a ^2} + i 2 \pi x s \right ] = -[(x - 2i \pi a^2 s)^2 + 4 \pi^2 a^4 s^2]/2a^2
$$
Why was this useful? Well, the integral is over \\(x\\), so we can pull out all terms that do not depend on \\(x\\), giving us a rearranged integral of
$$
F(s) = \exp(-2 \pi^2 a^2 s^2 ) \int_{-\infty}^\infty \exp \left (-\frac{(x - 2 i \pi a^2 s)^2}{2 a^2} \right)\\,\mathrm{d}x
$$

Here it is helpful to remember your Gaussian integration formulas such that
$$
\int_{-\infty}^\infty e^{-a(x + b)^2}\\,\mathrm{d}x = \sqrt{\frac{\pi}{a}}.
$$

Thus, the integral contributes another factor of \\(\sqrt{2 \pi} a\\) and the final result is

$$
F(s) = \sqrt{2 \pi} a \exp(-2 \pi^2 a^2 s^2 )
$$

What functional form is this? This is another Gaussian, though the normalization and standard deviation are a bit different! So, we see that the Gaussian function is a Fourier transform pair with itself.

Hopefully now you have a taste of how to compute Fourier transforms. At its most basic, it's just a matter of setting up and evaluating the integral. For many function forms, you can use integration strategies to arrive at analytic solutions. As a practical matter, in the next lecture we'll see how we can evaluate this integral numerically.

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

## The impulse symbol and Shah function

* The impluse symbol \\(\delta\\).
* Sifting property TMS A2.11
$$
f(x) = \int_{-\infty}^{\infty} f(x^\prime) \delta(x^\prime - x)\\,\mathrm{d}x^\prime.
$$
* Do the FT of a delta function, and show that it's a constant

## Fourier transform theorems properties: similarity, convolution, multiplication

There are several useful properties of the Fourier transform that you'll want to familiarize yourself with. See Bracewell Ch. 6 or TMS A2.1.2.

* Note how FFTconvolve using python speeds up. Depends on the scaling, and size of convolutional kernel.

## Common FT transform pairs

## Sampling and Nyquist sampling theorem
