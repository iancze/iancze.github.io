---
title: "Spontaneous Emission, Stimulated Emission, and Absorption"
date: 2021-09-01
draft: true
---

Draine Ch. 6

General rules for absorption and emission of radiation by absorbers with *quantized* energy levels. Can be atoms, ions, molecules, dust grains, or anything that has (quantized) energy levels.


## Absorption of photons

Some absorber is in a lower level, there is radiation present that has 
$$
h \nu = E_u - E_l
$$
the absorber can absorb a photon and undergo an upward transition
$$
X_l + h \nu \rightarrow X_u.
$$

### Rate of this reaction

There is some \\(n(X_l)\\). The rate per volume at which absorbers absorb photons (\\(l \rightarrow u\\)) is proportional to the density of photons of the appropriate energy and the number density of absorbers

$$
\underset{\mathrm{populate\\;level}\\;u}{\left ( \frac{\mathrm{d}n_u}{\mathrm{d}t} \right )_{l \rightarrow u}} = \underset{\mathrm{depopulate\\;level}\\;l}{- \left ( \frac{\mathrm{d}n_l}{\mathrm{d}t} \right)} 
$$

$$
= n_l B_{lu} u_\nu
$$

\\(u_\nu\\) is the radiation energy density per unit frequency.

\\(B_{lu}\\) is a proportionality constant called the Einstein B coefficient for the transition \\(l \rightarrow u\\).

## Emission of photons

### Spontaneous emission
$$
X_u \rightarrow X_l + h \nu
$$

Random process (independent of radiation field), and occurs with a probability per unit time \\(A_{ul}\\) called the Einstein A coefficient.

### Stimulated emission 
$$
X_u + h \nu \rightarrow X_l + 2 h \nu
$$

Occurs if photons of *identical*
* frequency
* polarization
* direction
are present in a radiation field.

### Rate of emission
From state \\(u \rightarrow l\\)

$$
\left ( \frac{\mathrm{d}n_l}{\mathrm{d}t} \right) = - \left ( \frac{\mathrm{d}n_u}{\mathrm{d}t}\right) = n_u (A_{ul} + B_{ul} u_\nu)
$$

\\(B_{ul}\\) is the Einstein B coefficient for the downward transition.

## Einstein coefficients are not independent

$$
B_{ul} = \frac{c^3}{8 \pi h \nu^3} A_{ul}
$$

$$
B_{lu} = \frac{g_u}{g_l} B_{ul} = \frac{g_u}{g_l} \frac{c^3}{8 \pi h \nu^3} A_{ul}
$$

Strength of stimulated emission (\\(B_{ul}\\)) and absorption (\\(B_{lu}\\)) are both determined by \\(A_{ul}\\) (spontaneous emission) and the ratio of the degeneracies \\(\frac{g_u}{g_l}\\).

## Radiation fields

(Review from Rybicki and Lightman, Ch 1)

\\(I_\nu \\) is the *specific intensity* of radiation, you can think of it as the energy carried along by an infinitesimal "bundle" of rays.

It has dimensions 
$$
\mathrm{ergs}\\;\mathrm{s}^{-1}\\;\mathrm{cm}^{-2}\\;\mathrm{ster}^{-1}\\;\mathrm{Hz}^{-1}
$$

\\(B_\nu\\) is exactly the same type of variable, we just use it instead of \\(I_\nu\\) when we're referring to blackbody radiation.

\\(I_\nu \\) can be a little mind-bending to think about... it can be a function of 
* 3D space
* direction
* frequency

Intensity itself is *not* a vector quantity but it *is* a function of direction. Draine and Rybicki and Lightman write the angular direction vector as \\(\Omega\\) and the solid angle surrounding that vector as \\(\mathrm{d}\Omega\\).

