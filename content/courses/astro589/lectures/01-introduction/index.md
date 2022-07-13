---
title: Introduction and Course Overview
date: 2022-08-24
publishdate: 2022-06-21
draft: true
---

# References and Resources for this lecture

Full reference information can always be found in the [syllabus]({{<relref "/courses/astro589/syllabus">}}), under "References Materials."

* *Essential Radio Astronomy* [Ch 1](https://www.cv.nrao.edu/~sransom/web/Ch1.html#S3): emission mechanisms, relevant astrophysical objects
* *Tools of Radio Astronomy*: radio windows, units, flux densities
* *Interferometry and Synthesis in Radio Astronomy*: single-dish observations, units, flux densities

# Course Overview

Welcome to Astro 589, a graduate seminar on radio astronomy and interferometric imaging!

* [Syllabus]({{<relref "/courses/astro589/syllabus">}}) overview regarding format, HW assignments, group project including proposal dates and presentation dates

# Astrophysics at radio wavelengths

## Emission mechanisms

* Atomic hyperfine splitting ("21-cm" line corresponding to neutral hydrogen)
* Radio synchrotron
* Thermal emission from cold (< 100 K) media
* Molecular emission lines, primarily from rotational transitions (e.g., CO \\(J=2-1\\))

{{< figure src="atmospheric_windows.jpg" link="https://www.cv.nrao.edu/~sransom/web/Ch1.html#S1" caption="Atmospheric windows for astronomy. Credit: ESA/Hubble (F. Granato) and Essential Radio Astronomy.">}}

The radio region of the spectrum is very transparent, especially compared to optical windows. Only towards very high radio/microwave frequencies (wavelengths about 1 mm), does the atmospheric transmission start to decline.

## Astrophysical objects

In truth, almost everything these days!

* quasars
* gamma ray burst (GRB) afterglows
* fast radio bursts (FRBs)
* pulsars
* supernovae remnants
* cosmic microwave background (CMB)
* galaxies, including molecules at high redshift sources
* the Sun
* planets (e.g., Jupiter, Uranus)
* protoplanetary disks
* molecular clouds (molecular emission)
* black hole accretion disks (EHT)

TODO: figures of relevant sources, with academic citations

# Single-dish radio telescopes

In this section, we'll cover existing single-dish radio telescopes and refresh our memory of basic telescope performance.

The same
$$
\theta \approx \frac{\lambda}{D}
$$
applies for radio antennas. Take a \\(\lambda = 1\\;\mathrm{cm}\\) observation, for example. Compared to an optical \\(\lambda = 500\\;\mathrm{nm}\\) telescope the same size, the resolution will be a factor of
$$
\frac{1\\;\mathrm{cm}}{500\\;\mathrm{nm}} = 20,000
$$
worse. Yikes!

Radio astronomers are constantly trying to find ways to increase angular (spatial) resolution.

* One way is to build bigger telescopes, such as the Green Bank Telescope (100m diameter)

{{< figure src="gbt.jpg" link="https://public.nrao.edu/gallery/green-bank-telescope/" caption="The Green Bank Telescope (100m diameter) operates at radio wavelegths. Credit: NRAO/AUI/NSF" >}}

* Another way is to work at higher frequencies (shorter wavelengths), e.g. sub-mm radio astronomy (IRAM 30m telescope)

{{< figure src="iram.jpg" link="https://www.iram-institute.org/EN/30-meter-telescope.php" caption="The IRAM 30m diameter telescope, which operates at sub-mm wavelengths. Credit: Wikipedia/IRAM-gre">}}

* A final way is to use *interferometry*, sometimes also at sub-mm wavelengths (ALMA)

It's easier to build larger telescopes at longer wavelengths because the tolerances required for the reflecting surface are less strict than at optical wavelengths. Though typically one must keep surface tolerances to within
$$
\sigma = \frac{\lambda_\mathrm{min}}{16},
$$
otherwise the efficiency of the antenna starts to decline substantially. For the 100 m diameter GBT operating at it's highest frequency (100 GHz) or 3 mm, this translates to \\(200\\;\mu\mathrm{m}\\), which is the thickness of two sheets of paper! That's quite an engineering challenge, and is the reason why large, steerable dishes are difficult to build.

Keeping telescopes fixed is one way to build a little bit bigger, such as [FAST](https://en.wikipedia.org/wiki/Five-hundred-meter_Aperture_Spherical_Telescope), which is a five hundred meter diameter fixed telescope in China. See also Arecibo, which unfortunately collapsed in December 2020. Eventually, though, the materials/engineering cost to building large single dish telescopes becomes prohibitive.

## Single dish observations

The "beam" or power pattern as a function of direction, of a receiving antenna can be calculated using the reciprocity theorems for transmitting and receiving antennas. We don't have time to cover it in this lecture, but the end result is that the far field electric field pattern \\(f(l)\\) is the Fourier transform of the electric field illuminating the aperture of the telescope \\(g(u)\\).

{{< figure src="uniform-dish.png" link="https://www.cv.nrao.edu/~sransom/web/Ch3.html#S1" caption="A schematic illustration of (top): Uniformly illuminated aperture (middle): The electric field pattern of the antenna, as a function of direction (bottom): The power pattern of the antenna, as a function of direction. Credit: Essential Radio Astronomy">}}

For large apertures, the nulls at \\( l = \pm 1, 2, \ldots\\) appear at the angles \\(\theta \approx \lambda/D, 2 \lambda/D, \ldots\\). In two dimensions, for a circular aperture, this is an Airy pattern.

{{< figure src="beam-pattern.png" caption="A beam power pattern plotten in polar coordinates, demonstrating that the antenna can pick up power from sidelobes at range of angles. Credit: Tools of Radio Astronomy.">}}

This is an idealized representation, but is still helpful. The beam can pick up power through sidelobes at a range of angles. Directional antennas help concentrate power in the main beam, but antennas with secondary stages (and thus supports, like most telescopes) create opportunities for ground radiation to reflect into the receiver.  The relationship between the aperture and the electric

You can think of single-dish telescopes (unless they have a sophisticated, multi-pixel receiver) essentially as single-pixel devices. So to make a map of the sky, you would need to raster scan the telescope across the region of interest, reading out antenna temperature as a function of RA, Dec. To make a good (i.e., scientifically accurate) map, you should focus on Nyquist sampling the sky to a uniform sensitivity, usually through a hexagonal pattern of dithering. More advanced instruments may have an array of "feeds" in a focal plane (mirroring a set of "pixels"), but this is still a small number of pixels compared to a typical CCD (e.g., 25 or 36).

## Temperatures, redux

In an earlier lecture, we gave definitions of "brightness temperature" and "antenna temperature" following Draine

* **brightness temperature**: the temperature corresponding to the specific intensity if we used the full form of the Planck formula
* **antenna temperature**: the temperature corresponding to the specific intensity if we were in the Rayleigh-Jeans domain. Added benefit that specific intensity is *linearly related* to antenna temperature and makes it easy to substitute one for the other.
$$
T_A(\nu) = \frac{c^2}{2 k \nu^2} I_\nu
$$

These definitions are still true, but also somewhat idealized, as we'll cover in a second.

In the field of radio astronomy, be aware that one frequently combines temperatures in other interesting ways. One can express random noise power in terms of an effective temperature
$$
P = k T \Delta \nu
$$
where \\(\Delta \nu\\) is the bandwidth of the observation. Here the power is equal to the noise power delivered to a **matched load** by a resistor at physical temperature \\(T\\). By matched load, we mean we connect a resistor to the input terminals of a linear amplifier. The fact that this resistor has some temperature (i.e., we haven't cooled it to absolute zero...) means that the thermal motion of the electrons will produce a random, variable current \\(i(t)\\) input to the amplifier. The mean value of this current is zero, but the root mean squared value is non-zero, and this represents a non-zero power. I.e., you can draw (some) power from a resistor at room temperature, purely from thermal motions. The situation is not dissimilar to the random walk of a particle in Brownian motion. For more details, see *Tools of Radio Astronomy*, Chapter 1.8.

* **Antenna temperature** \\(T_A\\) the component of the power received by the antenna from *cosmic sources*. It has the same interpretation as before (though we'll talk about beam dilution in a second).
* **Receiver temperature** \\(T_R\\) the component of the power from internal noise of the receiver components themselves, ground radiation, atmospheric emission, etc...
* **System temperature** \\(T_S = T_A + T_R\\) is the sum of receiver temperature and antenna temperature. It's the one power number coming out of the backend of your telescope. It's up to you to calibrate \\(T_R\\) accurately enough to measure \\(T_A\\).

In any observation, you will have your cosmic signal of interest and several contributions of noise (see *Essential Radio Astronomy*, Ch. 3.6.1)
$$
T_S=T_\mathrm{cmb}+T_\mathrm{rsb}+T_A + [1−\exp(−\tau_A)] T_\mathrm{atm}+T_\mathrm{spill}+T_r+ \ldots
$$
such as the CMB, other galactic background sources, the atmosphere, spillover radiation from the ground, the temperature of the radiometer itself (hopefully cryogenically cooled), etc.

In the limit that \\(T_A \ll T_S\\) (most astronomy situations, unfortunately!), we have
$$
S/N \approx C \frac{T_A}{T_S} \sqrt{\Delta \nu \Delta t}
$$
where \\(C\\) is a constant of proportionality greater than or equal to 1, and \\(\Delta t\\) is the integration time. If we let \\(\Delta \nu \approx 1\\;\mathrm{GHz}\\) and \\(\Delta t \approx 1\\;\mathrm{h}\\), then we can get \\(\sqrt{\Delta \nu \Delta t} \approx 10^6\\), allowing us to detect a signal which is less than \\(10^{-6}\\) the system noise. A great illustration of this capability is the COBE satellite that studied CMB anisotropies with brightness temperatures \\(< 10^{-7}\\) that of the system temperature. To achieve these contrasts, however, it's important to keep systematics under control, otherwise the S/N scaling won't hold!

## Beam dilution

In previous lectures, we've been talking about specific intensity \\(I_\nu(\Omega)\\) as a "known" quantity of direction (e.g., R.A., Dec.) and used antenna temperature \\(T_A\\) as a linear proxy for its specification. For the following discussion, we're going to move into the realm of observations, and discuss the ways \\(T_A\\) can be an unfaithful proxy for the "true" specific intensity distribution or brightness temperature. we'll use the symbol \\(T_b(\Omega)\\) to denote the "true" brightness/antenna temperature, assuming we're in the Rayleigh-Jeans limit, and redefine \\(T_A\\) to mean the response of the telescope to the cosmic radiation.

When we're doing observations, we don't always have access to the highest resolution version of \\(T_b(\Omega)\\), but rather we have access to a quantity which is the true \\(T_b(\Omega)\\) convolved with the beam of the telescope, which is the implication of \\(T_A\\) for this discussion.

#### large (fully resolved) source

[Draw a smooth cloud with the beam overlaid as a small circle].

In the limit that we are observing a source that subtends a solid angle much larger than the beam of the antenna,
$$
\Omega_S > \Omega_A
$$
the convolution of the beam doesn't matter, we're still sensing approximately the same \\(T_b(\Omega)\\) such that
$$
T_A(\Omega) \approx T_b(\Omega).
$$
If the source is in LTE, then we also have that \\(T_A \approx T\\).

### small (unresolved) source

[Draw a beam with a smaller source in the center]

If the source is more compact than the antenna beam,
$$
\Omega_S < \Omega_A,
$$
then the measured antenna temperature is basically the "true" intensity averaged over the area of the main beam. This lowers the measured antenna temperature by a factor
$$
\frac{T_A}{T_b} = \frac{\Omega_S}{\Omega_A}
$$
where the ratio \\(\frac{\Omega_S}{\Omega_A}\\) is called the **beam filling factor**.

For example, you could have a compact source with \\(T_b = 10^4\\) K, but if it only fills 1% of the beam solid angle then you would measure an antenna temperature of 100 K. If you took your observations at face-value (and assumed LTE), then you would incorrectly conclude that the source is 100x cooler than it actually is.

Beam dilution also applies to observations of sources that have *structure* on spatial scales below the observable limit, which, to be honest, is going to be most astrophysical sources of interest. For example, consider a gas filament in a star-forming region.

[Draw an approximation of a gas filament]

Radio-bright, spatially concentrated regions will be "smeared out" by the beam. If you wanted to use antenna temperature (and assume LTE) to estimate the physical conditions of the gas filament, you do so at the peril of measuring incorrect temperatures. The unfortunate reality here is that, without higher resolution images to guide you (which sometimes exist at optical or infrared wavelengths), it's quite difficult to estimate how badly your measurements are affected by beam dilution.

### point source

In the case of an unresolved *point source* with known flux \\(F_\nu\\), it's not that helpful to talk about brightness temperatures if we can't also estimate the solid angle that the source subtends \\(\Omega_S\\), since it is required to relate flux to specific intensity (or brightness temperature). [Sometimes we *can* estimate \\(\Omega_S\\) from theoretical arguments, even if the source is so small as to still be unresolvable, in which case we'd still use the above calculations.]

For point sources, it's easier to talk about antenna temperature using
$$
T_A = \frac{P_\nu}{k} = \frac{A_e F_\nu}{2 k}
$$
where \\(A_e\\) is the effective collecting area of the telescope. This gives us the strange but sometimes convenient relationship of talking about a telescope's sensitivity to a point source using "kelvins per jansky," which is proportional to its collecting area.

For more useful single-dish guidance, see [these notes](https://www.atnf.csiro.au/research/radio-school/2011/talks/Parkes-school-Fundamental-II.pdf) by James Jackson, or [Ch 3.1.6](https://www.cv.nrao.edu/~sransom/web/Ch3.html#S1.SS6) of *Essential Radio Astronomy*.

# TODO

* Delete cruft we don't need
* Refine to focus on units of images, and how resolution affects this
* Discussion about units (brightness temperature), resolution, sensitivity, applications
* [[20210502190821]] flux-densities

# Introduction to interferometric arrays, ALMA, VLA, SMA, VLBA

* Discuss what these are, not necessarily how they work

{{< figure src="alma.jpg" caption="The Atacama Large (Sub)millimeter Array, an interferometric array of 66 antennas operating at sub-millimeter wavelengths. The largest antennas in the array are only 12m in diameter, yet through interferometry, the array is able to obtain far higher spatial resolution than the largest single-dish antennas. Credit: NRAO/ESO/NAOJ/JAO">}}

* Also include VLA, EHT, ngVLA, VLBA, European network?

.
