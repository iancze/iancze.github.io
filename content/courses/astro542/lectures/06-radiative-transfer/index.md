---
title: "Radiative Transfer"
date: 2021-09-03
publishdate: 2021-09-01
---

* Draine Ch. 7
* Lecture notes on [Radiative Transfer in Astrophysics](https://www.ita.uni-heidelberg.de/~dullemond/lectures/radtrans_2012/index.shtml) by C.P. Dullemond.
* *Radiative Processes* by Rybicki and Lightman

Radiative transfer describes the propagation of radiation through absorbing and emitting media.

In Wednesday's lecture, we introducted specific intensity \\(I_\nu\\) and photon occupation number \\(n_\gamma(\nu)\\).

Photon occupation number 
$$
n_\gamma(\nu) = \frac{c^2}{2 h \nu^3} I_\nu
$$
"dimensionless" number of photons per mode per polarization (per solid angle per frequency bin).


In LTE (blackbody),
$$
I_\nu = B_\nu(T) = \frac{2 h\nu^3}{\exp(h \nu / kT) - 1}
$$
and 
$$
n_\gamma = \frac{1}{\exp(h \nu / k T) - 1}.
$$

## Gallery of temperatures!

**Now don't assume LTE**

* Brightness temperature \\(T_B(\nu)\\) (units K) is a way to characterize a *specific intensity* (or surface brightness). Given some specific intensity \\(I_\nu\\), it is defined as the temperature such that a blackbody at that temperature has that specific intensity, i.e.,
$$
T_B(\nu) = \frac{h \nu / k}{\ln[1 + 2 h \nu^3/c^2 I_\nu]}
$$

On it's own, quoting a brightness temperature *says nothing about whether the astrophysical source is in LTE*. In fact, brightness temperature is commonly used to describe sources that are far from LTE or isotropic (masers, GRBs, etc.) You could alternatively describe the specific intensity (or surface brightness) in units of Jy/ster, for example, since that is what the Planck function provides (given some temperature).

* Antenna temperature \\(T_A\\) (units K) is just like brightness temperature, only we make the assumption that we're in the Rayleigh-Jeans limit of \\(k T_A \gg h \nu\\) or \\(n_\gamma \gg 1\\) so we have 
$$
T_A(\nu) = \frac{c^2}{2 k \nu^2} I_\nu
$$

Again, an antenna temperature says *nothing about whether the astrophysical source is in LTE*, it is a convenient way to describe a specific intensity or surface brightness. If you're going to use temperature as a shorthand for specific intensity (without needing to appeal to LTE temperature), antenna temperature is a good one to use because it is *linear* in intensity.

**BUT**, if a source *is* in LTE with temperature \\(T\\), then we will have \\(T_B = T\\), and if we're observing at radio wavelengths, \\(T_A \approx T\\) as well.

Remember from last lecture we said that *in LTE* we could calculate the relative level populations between two states using the Boltzmann distribution
$$
\frac{n_u}{n_l} = \frac{g_u}{g_l}e^{(E_l - E_u)/kT}
$$
*Given some relative population* of two levels, we can define a quantity called excitation temperature
$$
\frac{n_u}{n_l} = \frac{g_u}{g_l}e^{(E_l - E_u)/kT_\mathrm{exc}}.
$$
So an excitation temperature (in K) is a way to communicate the relative population of two levels. In LTE, \\(T_\mathrm{exc} = T\\).

## Radiative transfer

{{< figure src="fig_7_1.jpg" caption="Draine Figure 7.1. Geometry for radiative transfer." >}}

Equation of radiative transfer. Consider a ray or beam of radiation. Neglect scattering.
$$
d I_\nu = - I_\nu \kappa_\nu ds + j_\nu ds
$$
* The first term is the net change in \\(I_\nu\\) due to absorption and stimulated emission.
* The second term is the change in \\(I_\nu\\) due to spontaneous emission by the material in the path of the beam

## Emission and absorption coefficients

* \\(\kappa_\nu\\) is attenuation coefficient, and has dimensions of [1/cm]. Usually positive, except for masers/lasers, where it's negative
* \\(j_\nu\\) is emissivity, with units power per unit volume per frequency per unit solid angle

For something with energy levels (and randomly oriented in space)
$$
j_\nu = \frac{1}{4 \pi} n_u A_{ul} h \nu \phi_\nu
$$
where \\(\phi_\nu\\) is the normalized line profile from the previous lecture.