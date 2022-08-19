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

* [3Blue1Brown](https://www.youtube.com/watch?v=spUNpyF58BY&ab_channel=3Blue1Brown)

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
e^{i x} = \cos x + i \sin x.
$$
And it's useful to keep in mind Euler's identity:
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

## Intro of FT (analytical)

Fourier series, complex sinusoids

* Linearity
* Transforms of some simple functions

## Fourier transform theories: similarity, convolution, multiplication

Note how FFTconvolve using python speeds up. Depends on the scaling, and size of convolutional kernel.

## Common FT transform pairs

## Sampling and Nyquist sampling theorem

## Discrete Fourier Transform

* Windowing + sidelobes (filters)
