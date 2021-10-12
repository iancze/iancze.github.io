---
title: Radiative Trapping
date: 2021-10-06
publishdate: 2021-10-06
---

[Zoom link](https://psu.mediaspace.kaltura.com/media/Astro+542A+Lecture+Oct+6/1_zra8cex4)

References: 

Draine Ch. 19

In many situations, like dense molecular clouds or stellar outflows, we have sufficient gas densities that a photon emitted by 
$$
X_u \rightarrow X_l
$$
will have a high probability of being absorbed by another \\(X_l\\) nearby, and, thus, a low probability of ever escaping from the region and being seen by an observer. This is called **radiative trapping**. The consequences are

1) reducing the emission from \\(X_u \rightarrow X_l\\) emerging from the region
2) changes the level populations of \\(X\\) relative to what would be calculated if the photons escaped

This is (one of the reasons) why radiative transfer can be hard in practice: excitation is coupled across non-local scales. One way to simplify things a tiny bit is to use the **escape probability approximation**.

## Escape probability approximation

Consider some slab of material that has light passing through it. The optical depth through this slab is \\(\tau_\nu\\). What is the probability that the photon makes it through the slab without being absorbed?
$$
\beta = e^{-\tau_\nu}.
$$
I.e., 
* if \\(\tau_\nu \approx 0\\), the photon will pass through with probability of 1 (it will nearly always go through).
* if \\(\tau_\nu \gtrsim 1\\), the photon will pass through with probability of 0 (it will nearly always be absorbed).

Now, let's consider some location \\(\boldsymbol{r}\\) within a cloud, and we're looking in a direction \\(\hat{\boldsymbol{n}}\\). 

The total optical depth (i.e., integrated until we're out of the cloud) in that direction is \\(\tau_\nu(\hat{\boldsymbol{n}}, \boldsymbol{r}) \\).

**Q**: If an atom at this location emitted a photon (in a random direction), what would be the probability that it would escape the cloud?

**A**: We'd need to average over all directions
$$
\bar{\beta}_\nu( \boldsymbol{r}) \equiv \int \frac{d \Omega}{4 \pi} e^{-\tau_nu(\hat{\boldsymbol{n}}, \boldsymbol{r})}.
$$

So far, we've been treating the escape probability at a single frequency. But let's say we wanted to treat the escape probability averaged over the line profile
$$
\langle \beta(\boldsymbol{r}) \rangle = \int \phi_\nu \bar{\beta}_\nu( \boldsymbol{r}) d \nu.
$$

Now, let's re-examine the level population calculations using the escape probability approximation and making a few more assumptions
* excitation level is uniform throughout the cloud
* "on-the-spot": if a radiated photon will be absorbed, it's absorbed close to (at) the point of emission

**Plan**: start with the rate of change of populations for a two-level system, incorporate the escape probabilities, and see what's changed.

$$
\frac{d n_u}{d t} = \left ( n_c k_{lu} + n_\gamma \frac{g_u}{g_l} A_{ul} \right) n_l - \left ( n_c k_{ul} + A_{ul} + n_\gamma A_{ul} \right) n_u
$$

Because the cloud is uniformly excited, we can take the RTE in integral form and rewrite it as 
$$
I_\nu = I_\nu(0) e^{-\tau_\nu} + B_\nu(T_\mathrm{exc})(1 - e^{-\tau_\nu})
$$

and then rewrite it in terms of photon number

$$
n_\gamma(\nu) = n_\gamma^{(0)} e^{-\tau_\nu} + \frac{1 - e^{-\tau_\nu}}{(n_l g_u)/(n_u g_l) - 1}
$$
where

$$
n_\gamma^{(0)} \equiv \frac{c^2}{2 h \nu^3} I_\nu(0).
$$

we can introduce \\(\beta_\nu\\), do the average over direction and line profile, and obtain

$$
\langle n_\gamma \rangle = \langle \bar{\beta} \rangle n_\gamma^{(0)} + \frac{1 - \langle \bar{\beta} \rangle}{(n_l g_u/n_u g_l) - 1}.
$$

Then we can rewrite the rate of change of level populations to be
$$
\frac{d n_u}{d t} = n_c k_{lu} n_l  + n_c k_{ul} n_u - \langle \bar{\beta}\rangle A_{ul} n_u + n_l \frac{g_u}{g_l} \langle \bar{\beta}\rangle A_{ul} n_\gamma^{(0)} \left (1 - \frac{n_u g_l}{n_l g_u} \right),
$$
which is called the **escape probability approximation**.

What have we done? We started with an equation that included photoexcitation and stimulated emission, including the effects of photons emitted by the cloud material, which is valid in the optically thick regime as well as optically thin. This was tricky to solve in the optically thick regime, because the terms were **coupled across large distances**.

We have used the escape probability approximation and written down an equation that the level populations must satisfy, taking into account all of the radiative processes (absorption, spontaneous emission, and stimulated emission), and photons both originating externally and emitted with the cloud, assuming we know the value of the escape probability \\(\langle \bar{\beta} \rangle\\).

This modified equation is nice because it functionally looks like an equation we'd write down with
* no internally generated radiation field present
* cloud was optically thin to external radiation field \\(I_\nu(0)\\)
* the Einstein A coefficient replaced by an effective value \\(\langle \hat{\beta}\rangle A_{ul}\\).

Because this is functionally "optically thin," we've decoupled the terms across distances and can use it to calculate level populations in a much more straightforward manner.

Can also be applied to the case of scattering.

[See also notes from NED/Caltech](https://ned.ipac.caltech.edu/level5/March02/Netzer/Netzer2_4.html).

## Applications

### Homogeneous static spherical cloud

The angle-averaged escape probability depends on the geometry and velocity structure of the region, e.g., escape probability will be largest at cloud boundary and lowest in cloud center. There are ways to define a mass-averaged escape probability over the cloud volume (see Draine 19.2).

### CO J=1-0

Frequently used as a tracer of molecular gas. Can follow through from an assumption of Gaussian velocity distribution, and known quantities about partition functions. Given these, we usually find that at least the J=1-0 level of CO is expected to be thermalized in molecular clouds.

## Local Velocity Gradient (LVG) approximation

Also called Sobolev approximation.

If there is a velocity gradient such that point-to-point differences across the flow field are large compared to the width of the velocity distribution at a given point, then you can get more emission out of the cloud (it has more frequency bandwidth on which to emit).

## Escape probability for turbulent clouds and the "X-Factor" between \\(\mathrm{H}_2\\) mass and CO

Observers of the cold ISM face a perennial problem in that most of the hydrogen mass in these regions is in molecular form \\(\mathrm{H}_2\\), but that molecule doesn't have a dipole moment, and therefore does not readily emit, so it's "invisible."

It's commonly assumed that CO 1-0 transition can be used as a tracer of molecular cloud mass. 

Assumptions underpinning this include virialized clouds (setting turbulent velocity dispersions). Depends on the density and temperature of the cloud.