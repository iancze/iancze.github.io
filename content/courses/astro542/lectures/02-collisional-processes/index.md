---
title: "Collisional Processes"
date: 2021-08-25
draft: true
---

## References for this lecture

* Draine Ch. 2 (primary)


## Intro

Collisions are one of the main ways in which energy is transferred about the ISM, so in order to understand the physics of the ISM, we need to understand the physics of collisions.

* thermal, electrical, diffusion coefficients
* produce most excitations of atoms and molecules (which then emit photons)
* recombine ions + electrons
* chemical reactions


Goal: understand a basic physical framework for understanding different types of collisions, and how their rates depend on density and temperature.

Types of collisional interactions and how we'll understand them
* long range Coulomb (charges, ions, etc...) \\(\propto 1/r\\): impact approximation
* intermediate range \\(\propto r^{-4}\\) induced dipole between ions and neutral atoms or molecules: scattering by \\(\propto r^{-4}\\) potential
* interactions between electrons and neutrals: experimental data 
* short-range interactions between neutrals: "hard-sphere" estimates

## Collisional rates and rate coefficients

Consider a general two-body collisional process

$$
A + B \rightarrow \mathrm{products}.
$$

We can write the (average) reaction rate per unit volume as 

$$
n_A n_B \langle \sigma v \rangle_{AB}
$$

i.e., the rate is proportional to the number density of each species and the **two-body collisional rate coefficient**, \\( \langle \sigma v \rangle_{AB} \\), which has units of \\(\mathrm{cm}^3 \mathrm{s}^{-1}\\). Once you know the rate coefficient, this is a pretty simple relationship, just multiplying terms together. But let's unpack how we calculate the rate coefficient.

How do we calculate this coefficient?

First, let's consider the cross-section of the reaction

{{< figure src="cross-section.png" link="https://en.wikipedia.org/wiki/Cross_section_(physics)" caption="Scattering Cross Section. Attribution: Wikipedia: Qwerty123uiop">}}

$$
\sigma = \frac{1}{n \lambda}
$$

where \\(n\\) is number density and \\(\lambda\\) is the mean free path. Generally, \\(\sigma \\) can be thought of as the cross-sectional area (hence the name) for the interaction a . I.e., if the cross-section of the spheres are larger, then they're more likely to hit each other (interact). However, if the cross-section is very small, it's unlikely that they'll hit each other (interact).

For the types of interactions we're talking about, the cross-section of the interaction can be dependent on the relative velocity between the particles, \\(\sigma(v)\\).

For a gas, what can we generally say about the distribution of velocities of the particles? For a fluid in thermal equilibrium, the distribution function of the relative speed of encounters between particles is given by a Maxwellian velocity distribution

$$
f_v \\, \mathrm{d}v = 4 \pi \left (\frac{\mu}{2 \pi k_B T }\right)^{3/2} \exp(- \mu v^2/2 k_B T) \\, v^2 \mathrm{d} v
$$

where 
$$
\mu = \frac{m_A m_B}{m_A + m_B}
$$
is the reduced mass of the collisional partners. \\(f_v\\) is a distribution function, so to find the fraction of encounters that have relative speeds \\(v_a < v < v_b\\), we would just need to integrate over the relevant range
$$
\int_{v_a}^{v_b} f_v \\, \mathrm{d}v.
$$

$$
\langle \sigma v \rangle_{AB} = \int_0^\infty \sigma_{AB}(v) v f_v \mathrm{d}v 
$$