---
title: Scattering and Absorption by Small Particles
date: 2021-10-11
draft: true
---

References
* Draine Ch. 22
* Ryden and Pogge Ch. 6.2

## Cross sections and efficiency factors

In analogy to our previous setups with scattering cross sections, which we wrote as \\(\sigma\\), we'll define other cross sections that are useful for talking about grains and radiative transfer. At least in this chapter, Draine labels them by \\(C\\), and many of them have a dependence on wavelength. They still have units of area, \\(\mathrm{cm}^2\\).

* **absorption cross section** \\(C_\mathrm{abs}(\lambda)\\)
* **scattering cross section** \\(C_\mathrm{sca}(\lambda)\\)
* **extinction cross section**
$$
C_\mathrm{ext}(\lambda) = C_\mathrm{abs}(\lambda) + C_\mathrm{sca}(\lambda)
$$


### A brief note about radiative transfer with scattering included 

For a population of identical dust grains with number density \\(n\\), the extinction cross section is related to the attenuation coefficient by the relation 
$$
\kappa_\lambda = n C_\mathrm{ext}(\lambda).
$$

If you remember back to our radiative transfer lecture, we neglected scattering in nearly all applications. Now, we're including scattering for dust, and that means that radiative transfer gets **hard**. We can still calculate the optical depth
$$
\tau_\nu = \int_0^s \kappa_\lambda d s,
$$
however, to calculate an emergent intensity we can't just plug it into the integral form of the RTE like we did before. \\(\boldsymbol{x}\\) and \\(\boldsymbol{n}\\) are vectors. Here is an example including the sink and source terms due to scattering.

The sink term is added simply by using \\(\kappa_\mathrm{ext}\\), which includes the attenuation due to scattering. The problem starts to get gnarly when we consider the *source* term due to scattering. Basically, light can be scattered *into* the ray under consideration (attenuating the ray from which it originated). The modified RTE looks like

$$
\frac{d I}{d s}(x, n, \lambda) = - \kappa_\mathrm{ext}(x, \lambda) I(x, n, \lambda) + j_*(x, n, \lambda) + \kappa_\mathrm{sca}(x, \lambda) \int_{4 \pi} \Phi(n, n^\prime, x, \lambda) I(x, n^\prime, \lambda) d \Omega^\prime
$$

where \\(\Phi\\) term is a scattering phase function, which describes the probability that a photon originally propagating in direction \\(\boldsymbol{n}^\prime\\) and scattered at position \\(\boldsymbol{x}\\) will have \\(\boldsymbol{n}\\) as its new propagation direction.

This is called an *integro-differential* equation. It sounds hard because it is, and all radiation fields in all positions in all directions (and for all wavelengths) are coupled! For more information on the complexities of radiative transfer including dust, see the review article [Steinacker ARA&A](https://ui.adsabs.harvard.edu/abs/2013ARA%26A..51...63S/abstract).

### Back to terms 

The **albedo** 
$$
\omega \equiv \frac{C_\mathrm{sca}(\lambda)}{C_\mathrm{abs}(\lambda) + C_\mathrm{sca}(\lambda)} = \frac{C_\mathrm{sca}(\lambda)}{C_\mathrm{ext}(\lambda)}.
$$
An albedo of 1 means that scattering dominates the extinction (shiny silver thing), an albedo of 0 means that absorption dominates (pure black).

For a given direction of incidence to a fixed grain, you need two angles \\(\theta, \phi\\) to fully specify the scattering direction, since it can be in any direction in 3D space. However, if we are talking about spherical grains, or a large ensemble of randomly oriented grains, then we only need to talk about a single scattering angle \\(\theta\\). A value of 0 means that the light is scattered in the forward direction, a value of 180 means that the light is scattered backwards (back-scattered), and a value of 90 would have the light be scattered to the right, left, top, or bottom.

* We can write down a **differential scattering cross section**
$$
\frac{d C_\mathrm{sca}(\theta)}{d \Omega}
$$
for incident unpolarized light to be scattered by an angle \\(\theta\\).

* We can calculate the **mean value** of \\(\cos \theta\\) (scattering angle) by taking the average over all angles
$$
\langle \cos \theta \rangle = \frac{1}{C_\mathrm{sca}} \int_0^\pi \cos \theta \frac{d C_\mathrm{sca}(\theta)}{d \Omega} 2 \pi \sin \theta d \theta
$$

* We can calculate the radiation pressure cross section
$$
C_\mathrm{pr}(\lambda) \equiv C_\mathrm{abs}(\lambda) + (1 - \langle \cos \theta \rangle C_\mathrm{sca}(\lambda)).
$$

* And we also have the **degree of polarization** for light scattered through an angle \\(P(\theta)\\).

Frequently it's convenient to work with a dimensionless cross section that is normalized to some characteristic area of the grain. For a spherical grain, the geometric grain cross section is a good one: \\(\pi a^2\\). Draine adopts the following convention for a non-spherical grain: create a sphere having the same volume \\(V\\) as the grain material (not including voids) and use the effective cross section of that sphere: \\(\pi a_\mathrm{eff}^2\\), where 
$$
a_\mathrm{eff} = \left( \frac{3 V}{4 \pi} \right)^{1/3}.
$$
Then we have 
$$
Q_\mathrm{sca} = \frac{C_\mathrm{sca}}{\pi a_\mathrm{eff}^2}
$$
$$
Q_\mathrm{abs} = \frac{C_\mathrm{abs}}{\pi a_\mathrm{eff}^2}
$$
and
$$
Q_\mathrm{ext} \equiv Q_\mathrm{sca} + Q_\mathrm{abs}
$$
and all of the \\(Q\\) terms are dimensionless.

## Index of refraction

The index of refraction will indicate whether a dust grain is better at absorbing or scattering. The index of refraction is a complex number with real and imaginary parts
$$
\tilde{n} = n_r + i n_i.
$$

* The real part \\(n_r\\) is called the refractive index and determines how much a beam of light is bent (or refracted)
* The imaginary part \\(n_i\\) is called the absorption index, since it determines how strongly the substance absorbs light.

Both \\(n_r\\) and \\(n_i\\) are real and non-negative.

Consider an electromagnetic wave with field strength 
$$
E \propto e^{i(kx - \omega t)}
$$
traveling through a material. The dispersion relationship between wavenumber and frequency will be 
$$
k = \tilde{n} \frac{\omega}{c}.
$$

If the imaginary part of \\(\tilde{n}\\) is greater than zero, then the power of the electromagnetic wave will decrease as it propagates through the material
$$
|E|^2 \propto e^{-2 n_i \omega x/c}
$$
and thus the attenuation coefficient for that light will be 
$$
\kappa = 2 n_i \frac{\omega}{c} = \frac{4 \pi n_i}{\lambda_\mathrm{vac}}.
$$

