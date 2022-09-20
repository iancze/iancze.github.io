---
title: Interferometry in Practice
date: 2022-09-21
publishdate: 2022-09-18
---

## References for today

* [The Fourier Transform and its Applications](https://catalog.libraries.psu.edu/catalog/2010095) by R. Bracewell
* [Interferometry and Synthesis in Radio Astronomy](https://catalog.libraries.psu.edu/catalog/20789467) by Thompson, Moran, and Swenson, particularly Appendix 2.1
* [Essential Radio Astronomy](https://www.cv.nrao.edu/~sransom/web/xxx.html) by James Condon and Scott Ransom


## Review *of last time*

* Interpolation kernels
* Discrete Fourier Transform (DFT)
* Fast Fourier Transform (FFT)

## This time

The punchline of today is that the complex-valued visibility function \\(\mathcal{V}\\) is the 2D Fourier transform of an image on the celestial sphere (with a small field of view)

$$
I(l, m) \leftrightharpoons \mathcal{V}(u, v)
$$

and it is the visibility function that interferometers measure directly. The values of \\(u, v\\) for which interferometers are able to measure the visibility function depend on how the array of antennas is laid out and the spacing between them.

Most of today's lecture will follow Chapters 2 and 3 of [Interferometry and Synthesis in Radio Astronomy](https://catalog.libraries.psu.edu/catalog/20789467) by Thompson, Moran, and Swenson. First, we will introduce a two-element interferometer and it's response to a point source. Then, we'll complexify this a bit to talk about an extended source (but still in 1D). Then, we'll move on to discuss intensity distributions and the visibility function in the general case and then derive the relationship between \\(I(l, m) \leftrightharpoons \mathcal{V}(u, v)\\).

## Introduction to a 2-element interferometer

Consider this geometric situation

{{< figure src="elementary-interferometer.png" caption="Credit: TMS Fig 2.1" >}}

The antennas are spaced directly east-west, and they are observing a source in the *far-field*, i.e., the radiation from a distant cosmic source appears as a plane wave. First, we will consider the case of a *point-source*; later we will extend this formalism to spatially extended sources. We'll assume the primary beam of each antenna is large, such that they can observe radiation from a source located a wide range of \\(\theta\\) angles. We'll assume that we're observing in a narrow slice of frequency around \\(\nu\\), essentially monochromatic.


The wavefront from the source arrives at the right antenna some time 
$$
\tau_g = \frac{D}{c} \sin \theta
$$
before it reaches the left one. This is called the *geometric time delay*. 

Each antenna has its own signal voltage stream:
$$
V_1 = \sin 2 \pi \nu t
$$
and
$$
V_2 = \sin 2 \pi \nu (t - \tau_g).
$$

These streams are multiplied together in a *correlator* and then time-averaged over some interval. The output of the correlator is proportional to 
$$
F(t, \tau_g) = \sin (2 \pi \nu t) \sin 2 \pi \nu (t - \tau_g),
$$
which we can expand using our trig sum identities for sine to
$$
F(t, \tau_g) = \sin^2(2 \pi \nu t) \cos(2 \pi \nu \tau_g) - \sin(2 \pi \nu t)\cos(2 \pi \nu t) \sin(2 \pi \nu \tau_g).
$$

We can simplify this equation based on our knowledge that the correlator multiplies and then adds (integrates, typically for a few seconds). 
* The central frequency \\(\nu\\) is on the order of 10s of MHz to nearly a THz
* \\(\theta\\) (baked into \\(\tau_g\\)) is rotating at the Earth's rotational velocity, which is \\(10^{-4}\\;\mathrm{rad\\,s}^{-1}\\).
* \\(D\\) must be smaller than \\(10^7\\;\\)m for terrestrial baselines

This means that the rate of variation of \\(\nu \tau_g \ll \nu t\\) by several orders of magnitude.

So long as our averaging interval is \\(T \gg 1/\nu\\) (which is satisfied by a typical multi-second integration), 
$$
\langle \sin^2 (2 \pi \nu t) \rangle = 1/2
$$
and 
$$
\langle \sin(2 \pi \nu t)\cos(2 \pi \nu t) \rangle = 0
$$
so we're left with 
$$
F \propto \cos (2 \pi \nu \tau_g).
$$
We can also define \\(l = \sin \theta\\) and then we can write
$$
F \propto \cos (2 \pi \nu \tau_g) = \cos \left (\frac{2 \pi D l}{\lambda} \right ).
$$


This is called the *fringe function* of a two-element interferometer. We'll come back to discuss \\(l\\) in more detail in a moment, but for now you can think of it as a coordinate on the sky.

Let's draw the fringe pattern

TODO: draw as a linear relationship vs. \\(l\\), i.e., an oscillating sine wave
TODO: then include the fringe plot itself

{{< figure src="fringe.png" caption="The fringe function (plotted here as \\(|F|\\)) can be thought of as the directional power pattern of the interferometer in the case the antennas are isotropic. Credit: TMS Fig 2.2" >}}

So, you see that the 2-element interferometer has a sine/cosine sensitivity to the sky along the east-west axis. It has no sensitivity along the north-south direction.

## 2-element interferometer for a spatially resolved source

Now we'll consider a slightly more complex interferometer. 

* The antennas are (somewhat) directional
* They track the source as it moves across the sky, from the rotation of the Earth

The direction the antennas are pointed is called the *phase reference position*.

let 
$$
u = \frac{D \cos \theta_0}{\lambda} = \frac{\nu_0 D \cos \theta_0}{c}.
$$

$$
F(l) \propto \cos(2 \pi u l)
$$


---



We'll first develop the geometry and math of this relationship for small fields of view, so that you understand the result, at least in an abstract manner. Then we'll spend the latter part of the course working through how a radio interferometer like the VLA or ALMA works to actually sample the visibility function.

## Images

Thus far, we've been talking about 1 dimensional Fourier transforms. Now we'll talk about images and 2D Fourier transforms. Let's review our coordinates for images on the celestial sphere. 


### R.A. and Dec.

Declination's the easier one, in my opinion. No matter where you are, if you move in declination, you move along a great circle (i.e., a circle that actually traces the circumference of the celestial sphere). We measure this in terms of 0 (celestial equator) to +90 degrees at the north celestial pole and -90 degrees at the southern celestial pole. You can split a degree into 60 arcminutes and an arcminute into 60 arcseconds. 

TODO: celestial sphere figure

Right Ascension is the one that sometimes trips me up. Because the sky rotates, we have this system of marking it using sidereal *time*: 24 *hours* of right ascension, sometimes broken up into 60 *minutes*, and 60 *seconds*. Although they are still in multiples of 60, these *minutes* and *seconds* we use for right ascension do not have the same angular size as arcminutes and arcseconds (even if we are on the celestial equator). For example, let's say we have two points on the sky. 

    p1 = '00h42m00s', '+41d12m'
    p2 = '00h42m01s', '+41d12m'

Same declination, but their R.A. values differ by one second. Can anyone guess how many *arcseconds* separate these points? The answer is about 11.3 arcseconds.

TODO: plot with points labeled, and distance between them

Usually, when we are talking about observing a smaller source (like a protoplanetary disk, or galaxy), we point in some direction towards that object and then define a small little postage stamp, commonly in units of \\(\Delta \delta \\) and \\(\Delta \alpha \cos \delta\\), in which case, the units describing the image are arcseconds. We're still talking about spherical astronomy though, so this isn't necessarily limited to small fields of view. We could have \\(\Delta \delta\\) be several degrees, for example. 

TODO: Figure: point relative to center, and then Delta directions coming off of it.

### Direction cosines

In a moment, we're going talk about the mechanics of interferometers observing the celestial sphere and how these relate to Fourier transforms. Before we talk about that, though, there's a concept I want to introduce while we're still talking about units for images.

Practically speaking, most images we might make with ALMA or the VLA will have a small field of view (< 1 arcminute). In this regime, it simplifies a lot to talk about "flat" images, i.e., image planes that are tangent to the field center. In 1D, it would look like this

{{< figure src="direction-cosine.png" caption="The concept of the direction cosine. Credit: Fig 3.3 TMS" >}}

The direction cosines are
$$
l = \sin(\Delta \alpha \cos \delta) 
$$
and 
$$
m = \sin(\Delta \delta)
$$
relative to the phase center. You can see from the figure that it is the direction cosine that is actually tracking the position on the tangent plane, where we are defining our image.

Because they are outputs from trigonometric functions, they are technically unitless. Though l and m are technically unitless and measures of *linear* distance, for small angular extent, they could also be considered to have units of radians. So it will be common that we refer to 
$$
l = \sin(\Delta \alpha \cos \delta) \approx \Delta \alpha \cos \delta
$$
and
$$
m = \sin(\Delta \delta) \approx \Delta \delta.
$$

This probably sounds pointless, since we just arrived back at the same units we started with. Hopefully the reasons why we might wish to use these units will become apparent after we cover more about the interferometer. And remember that you can always *exactly* convert from direction cosine back to angular usits (e.g. \\(\Delta \alpha \cos \delta\\)) by doing \\(\sin^{-1}\\), it's just that for most small angles we'll be dealing with, this operation is essentially an identity function. All of this goes out the window when we consider wide-field imaging (which we won't have time to talk about in this course, though you might consider as a topic for a course project). 

### Images and Fourier Transforms of Images

We usually talk about images on the celestial sky as \\(I_\nu(\alpha, \delta)\\), i.e., some specific intensity parameterized as a function of angular position. When we're using interferometers to observe a small field of view, we will usually write this as 
$$
I_\nu(l, m)
$$
where \\(l, m\\) are measured as the direction cosines relative to some phase center, given by \\(\alpha, \delta\\). E.g., the phase center will be 

    p1 = '00h42m00s', '+41d12m'

and the direction cosines will be Cartesian offsets from that center, on the image tangent plane.

Now, let us define the *visibility function* \\(\mathcal{V}\\) as the 2D Fourier transform of the image plane 
$$
\mathcal{V}(u,v) = \int \int I_\nu(l,m) \exp \\{ - 2 \pi i (ul + vm)\\}\\, \mathrm{d}l\\, \mathrm{d}m.
$$

\\(u\\) and \\(v\\) are called spatial frequencies. If we considered that \\(l\\) and \\(m\\) had units of arcseconds (or radians) for small angular extent, then \\(u\\) and \\(v\\) could also be interpreted in units of "cycles per arcsecond" or "cycles per radian," where \\(u\\) points in the east-west direction and \\(v\\) points in the north-south direction.

What are the units of \\(\mathcal{V}\\) itself? We can get at this by looking at the units of \\(I_\nu(l,m)\\) and how we carried out the Fourier transform integral. 

If we parameterized our image using \\(\mathrm{Jy} / \mathrm{arcsec}^2\\) and we integrated over \\( \mathrm{d}l\\, \mathrm{d}m\\) (both assuming they had units of arcsec), then \\(\mathcal{V}\\) must have units of Jy. I.e., you can think of it sort of like the flux being observed at that angular scale. The visibility function is complex-valued, so if you want to discuss "power" at some angular scale then you should consider \\(|\mathcal{V}|^2\\) .

All of the Fourier transform theorems we discussed in 1D (shift, similarity, convolution/multiplication, etc...) have 2D equivalents.

## Two-element interferometer

Now that we've introduced the mathematical formalism relating the image-plane to the visibility-plane
$$
I(l, m) \leftrightharpoons \mathcal{V}(u, v),
$$
let's get a bit more practical and examine how radio/sub-mm interferometers like the VLA and ALMA actually sample the visibility function.

A very good reference for what follows is [Essential Radio Astronomy](https://www.cv.nrao.edu/~sransom/web/xxx.html) by James Condon and Scott Ransom, in particular Chapter 3.

### Two-element interferometer

We'll focus our discussion around the two-element interferometer, since even arrays of antennas (like ALMA, containing 66 individual antennas) can be understood by breaking them down (conceptually) into this fundamental unit. At first we'll keep the reference frame generic just by using vector quantities. But once we've introduced the concept then we'll get specific about the coordinate system and (re)introduce \\(l,m,u,v)\\).

Consider two antennas separated by some distance \\(\vec{b}\\) simultaneously observing a source. Let the (unit) direction vector towards the source be given by \\(\hat{s}\\). Unless the source is directly overhead, a wavefront originating from the source will arrive at the antennas at slightly different times, corresponding to a geometric time delay \\(\tau_g\\). 

Each of these antennas is equipped with its own receiver hardware that records voltage as a function of time. Unfortunately, we don't have time to cover much about receiver design in this course, but you should be aware that there are many different types of receiver technology in use across the radio spectrum and this is an active area of research and development. The type of receiver used at 100 GHz (band 3) are very different from those used at 700 GHz (band 9).

{{< figure src="interferometer.png" link="https://www.cv.nrao.edu/~sransom/web/Ch3.html#S7" caption="A schematic of a two-element interferometer. Credit: Essential Radio Astronomy">}}

We can relate the voltage streams from these two antennas \\(V_1\\) and \\(V_2\\) by a single voltage amplitude \\(V\\) and a time delay. 
$$
V_1 = V \cos \left [ \omega (t - \tau_g )\right]
$$
and 
$$
V_2 = V \cos (\omega t).
$$

The two voltage streams are then **correlated** (multiplied together and then averaged) to form the output stream \\(R\\)
$$
R = (V^2/2) \cos (\omega \tau_g)
$$


For now, we'll call this a *cosine* correlator because of the cosine dependence of the correlation stream.

The fringe pattern in the lower right of this figure is what you would get if you kept the antennas stationary and let a point source drift across the field of view. The broad envelope is the attenuation from the primary beam response of the antennas as the source enters and then leaves the field of view. The fringe frequency depends on the observing frequency and the baseline separating the antennas.

Interestingly, the time averaged response of a multiplying interferometer has a mean of 0. This has the consequence that uncorrelated noise power from very extended sources (such as the CMB) will average to 0, therefore, the interferometer is not sensitive to radiation on those large spatial scales. This is called *spatial filtering*.

### Multiple antennas

With multiple pairs of antennas, we can build up an improved point source response, because the fringe patterns of the individual two-element interferometers add up.

{{< figure src="building_a_beam.png" link="https://www.cv.nrao.edu/~sransom/web/Ch3.html#S7" caption="How the fringe patterns of the individual two-element interferometers in an array build up to create a beam. Each shaded circle represents an antenna spaced out along a 1D line. Credit: Essential Radio Astronomy.">}}

In the limit of many antennas, the beam usually looks like something centralized, approximately Gaussian, but usually containing many sidelobes (hence, why radio astronomers call this a "dirty beam").

The resolution of the beam is on the order of \\(\lambda/b\\), where \\(b\\) is the projected baseline of the longest antenna pair. Note that this resolution strongly depends on the relative number of baselines at long/short separations, since what you're really talking about is sensitivity at certain spatial scales. We'll return to this in the next lecture. It won't suffice to just have a single long-baseline pair of antennas if most other antenna pairs are at much shorter baselines.

### Complex correlators

Now let's move beyond a discussion of point sources and consider what we'll call a "slightly extended source," something that is larger than the dirty beam but smaller than the primary beam of each telescope.

Instantaneous field of view of an interferometer is the same as the primary beam of each telescope, treated as a single dish (see previous section). Each single dish antenna is still seeing the same thing as before, it's just that we have a correlator backend that's doing things with the signals, allowing us to create a *synthesized beam* that is considerably smaller than the size of the primary beam. For example, at 220 GHz (band 6), ALMA has a primary beam of about 20 arcseconds in diameter. However, it's common to make synthesized beams on the size of 0.1 arcseconds or smaller.


TODO: develop formalism either with ERA or TMS to cover coordinates moving from correlator to l, m, u, v.

The cosine correlator we discussed has some limitations, namely that the quantity
$$
R_c = \int I(\hat{s}) \cos(2 \pi \vec{b} \cdot \hat{s}/\lambda) d\Omega
$$
term is only sensitive to the *even* (symmetric) component of a source.

[Draw cosine curve centered around \\(\vec{b} \cdot \hat{s} = 0 \\), show that there is ambiguity in the response.]

We can decompose any function into a sum of its even and odd (antisymmetric) parts. To capture the *odd* parts of the source, we need a different kind of correlation,
$$
R_s = \int I(\hat{s}) \sin(2 \pi \vec{b} \cdot \hat{s}/\lambda) d\Omega
$$
which can be obtained by inserting a \\(\pi/2 = 90^\circ\\) phase delay in the output of one antenna.

When we combine cosine and sine correlators (in hardware, this is all the same system), we have what is called a *complex correlator*. It is usually more convenient to treat the cosine and sine components simultaneously using Euler's formula and we have
$$
\mathcal{V} = R_c - i R_s
$$
or
$$
\mathcal{V} = \int I(\hat{s}) \exp(- i 2 \pi \vec{b} \cdot \hat{s}/\lambda) d\Omega.
$$
The quantity \\(\mathcal{V}\\) is called the *visibility* and it is a complex number. It is the fundamental data product from an interferometer, and, like all data, it is usually measured with some noise. 



### Coordinates

The equation we've just derived is fairly general, but let's expand this a bit to the astronomical coordinate systems we're more used to.

Rather than referring to the source brightness distribution with \\(I_\nu(\hat{s})\\), usually we'll refer to some *phase center*. [Typically this is the direction the antennas are pointed (i.e., center of primary beam), but doesn't need to be and can be changed after the fact in software.]

We can define the direction cosines in a tangent plane, such that we have \\(l = \sin(\Delta \alpha \cos \delta)\\) and \\(m = \sin(\Delta \delta)\\), where \\(\alpha\\) and \\(\delta\\) are R.A. and Dec., respectively.

{{< figure src="thompson_uv.png" caption="The relationship between baseline orientation, source position, and direction cosines \\(x = l\\) and \\(y = m\\). Credit: Tools of Radio Astronomy from Thompson 1982.">}}

We can rewrite the projected baseline in terms of its east-west \\(u\\) and north-south \\(v\\) components, where

$$
u = \left [ \frac{\vec{b} \cdot \hat{s}}{\lambda} \right ]_\mathrm{east-west}
$$

and

$$
v = \left [ \frac{\vec{b} \cdot \hat{s}}{\lambda} \right ]_\mathrm{north-south}.
$$

So we have that the image domain coordinates \\(l, m\\) have corresponding Fourier plane coordinates \\(u, v\\), called spatial frequencies.

And the relationship between source brightness distribution and the visibility function is given by
$$
\mathcal{V}(u,v) = \int \int I(l,m) \exp \\{ - 2 \pi i (ul + vm)\\} dl dm.
$$
This is a two-dimensional Fourier transform.

It turns out that the measured spatial frequencies correspond directly to the projected baseline vector lengths, if the baseline is measured in multiples of the observing frequency.

Longer baselines measure higher spatial frequencies, which correspond to finer-scale image plane features.







* TODO: talk about fringes, UV plane, and baselines. Show sine-wave fringes.
* TODO: develop easy FFT between uv plane population and fringes, with MPoL?



TMS pg 93, 95.
van Cettert-Zernike theorem (15.1.1 TMS)
* fringe-patterns

* Complex noise and measurement
For more on this, see the [MPoL notes on likelihood functions](https://mpol-dev.github.io/MPoL/rml_intro.html).



