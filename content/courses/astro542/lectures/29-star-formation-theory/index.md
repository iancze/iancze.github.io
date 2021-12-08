---
title: "Gravitational Collapse and Star Formation: Theory"
date: 2021-11-15
publishdate: 2021-11-15
---

[Zoom link](https://psu.mediaspace.kaltura.com/media/Astro+542A+Lecture+Nov+15/1_4xd4rf28)

References:
* Draine Ch 41

Gravity is the force responsible for gathering gas into self-gravitating structures, ranging in size from stars to giant molecular cloud complexes.

Star formation involves extreme *compression*: drawing a gas cloud from size \\(10^{18}\\) cm down to the size of a star \\(10^{11}\\) cm.

This lecture deals with the conditions necessary for gravitational collapse to occur (and be consistent w/ observations).
* Gravity must overcome the resistance of pressure (both gas pressure and magnetic pressure)
* Angular momentum must be transferred to nearby material
* Most magnetic field lines initially present in the gas must *not* be swept into the forming protostar

## Gravitational instability

Jeans instability: non-rotating and unmagnetized gas.

Approach: write down the fluid equations expressing conservation of mass and momentum, and the equation for the gravitational potential.

Then, solve for what happens to a perturbation. And define the criterion under which the perturbation grows rather than damps, that defines the **Jeans instability** and a corresponding **Jeans mass**.

$$
\frac{\partial \rho}{\partial t} + \nabla \cdot (\rho \boldsymbol{v}) = 0
$$

$$
\frac{\partial v}{\partial t} + (\boldsymbol{v} \cdot \nabla) \boldsymbol{v} = -\frac{1}{\rho} \nabla p - \nabla \phi
$$

$$
\nabla^2 \phi = 4 \pi G \rho
$$

See Draine Ch 35 for more.

\\(\phi\\) is the scalar potential for the gravitational field.

Suppose we have an equilibrium steady state solution with \\(\rho_0(\boldsymbol{r})\\), \\(\boldsymbol{v}_0(\boldsymbol{r})\\), \\(p_0(\boldsymbol{r})\\), and \\(\phi_0(\boldsymbol{r})\\).

And then, we'll determine the conditions under which the equilibrium solution is susceptible to gravitational collapse by introducing a small perturbation
$$
\boldsymbol{v} = \boldsymbol{v}_0 + \boldsymbol{v}_1
$$

$$
\rho = \rho_0 + \rho_1
$$

etc..

Plugging these into the equations, we get
$$
\frac{\partial \rho_1}{\partial t} + \boldsymbol{v}_0 \cdot \nabla \rho_1 + \boldsymbol{v}_1 \cdot \nabla \rho_0 = - \rho_1 \nabla \cdot \boldsymbol{v}_0 - \rho_0 \nabla \cdot \boldsymbol{v}_1
$$
and
$$
\frac{\partial \boldsymbol{v}_1}{\partial t} + (\boldsymbol{v}_0 \cdot \nabla) \boldsymbol{v}_1 + (\boldsymbol{v}_1 \cdot \nabla) \boldsymbol{v}_0 = \frac{\rho_1}{\rho_0^2} \nabla p_0 - \frac{1}{\rho_0} \nabla p_1 - \nabla \phi_1
$$
and
$$
\nabla^2 \phi_1 = 4 \pi G \rho_1.
$$

Then, if we consider an **isothermal** gas, which has \\(p = \rho c_s^2\\), we reduce the second equation to
$$
\frac{\partial \boldsymbol{v}_1}{\partial t} + (\boldsymbol{v}_0 \cdot \nabla) \boldsymbol{v}_1 + (\boldsymbol{v}_1 \cdot \nabla) \boldsymbol{v}_0 = - c_s^2 \nabla \left ( \frac{\rho_1}{\rho_0} \right) - \nabla \phi_1,
$$
which gives us three equations for three unknowns \\(\rho_1, \boldsymbol{v}_1, \phi_1\\).

Jeans (1928) considered the problem of a uniform, stationary gas, having \\(\nabla \rho_0 = 0\\), \\(\nabla \phi_0 = 0\\), \\(\boldsymbol{v}_0 = 0\\).

We can rearrange this to
$$
\frac{\partial^2 \rho_1}{\partial t^2} = c_s^2 \nabla^2 \rho_1 + (4 \pi G \rho_0) \rho_1
$$

Now, let's consider perturbations in plane-wave form
$$
\rho_1 \propto \exp [i (\boldsymbol{k} \cdot \boldsymbol{r} - \omega t)]
$$
which has the dispersion relation
$$
\omega^2 = k^2 c_s^2 - 4 \pi G \rho_0
$$
if we define 
$$
k_\mathrm{J}^2 \equiv (4 \pi G \rho_0)/c_s^2,
$$
then we have the dispersion relation
$$
\omega^2 = (k^2 - k_\mathrm{J}^2) c_s^2.
$$

So, \\(\omega\\) is real if and only if \\(k \geq k_\mathrm{J}\\), i.e., the wave number is larger than the Jeans wavenumber.

Otherwise, if \\(k < k_\mathrm{J}\\), then \\(\omega\\) becomes imaginary, which corresponds to exponential growth.

So the **Jeans instability** occurs for wavelengths
$$
\lambda > \lambda_\mathrm{J} \equiv \frac{2 \pi}{k_\mathrm{J}} = \left ( \frac{\pi c_s^2}{G \rho_0} \right)^{1/2}.
$$

Basically, perturbations greater than this scale grow.

We can define a corresponding **Jeans mass**
$$
M_\mathrm{J} \equiv \frac{4 \pi}{3} \rho_0 \left ( \frac{\lambda_\mathrm{J}}{2} \right)^3
$$
which, when you plug in typical densities of quiescent dark clouds, you get something around a third of a solar mass.

The exponential "growth time" is
$$
\tau_\mathrm{J} = \frac{1}{k_\mathrm{J} c_s} \approx 10^4\\,\mathrm{yrs}.
$$

**Flaws in the preceeding Jeans analysis**: the assumption that \\(\nabla \phi_0 = 0\\) everywhere is completely unphysical, because it implies that \\(\nabla^2 \phi_0 = 0\\) everywhere, which implies \\(\rho_0 = 0\\) everywhere, i.e., we're in a vacuum.

But, for finite systems, rigorous analyses yield instability criteria that are close to Jeans's.

## Parker instability

We won't have time to talk about it today, but there is an analogous analysis called **Parker Instability** which treats magnetic fields.


## Nonrotating, nonmagnetized isothermal core

Consider a spherical "core" with mass \\(M\\), radius \\(R\\), having external pressure \\(p_0\\) at the surface. The gravitational energy can be written as

$$
E_\mathrm{grav} = - \frac{3}{5} a \frac{GM^2}{R}
$$
where \\(a\\) is a dimensionless factor that is 1 for uniform density and is \\(a > 1\\) if the density is centrally peaked.

If the gas is in equilibrium with \\(v=0\\), then the virial theorem requires that
$$
0 = 3 M c_s^2 - 4 \pi p_0 R^3 - \frac{3}{5}a \frac{G M^2}{R}.
$$

The external pressure \\(p_0\\) must be given by 
$$
p_0 = \frac{1}{4 \pi R^3} \left [ 3 M c_s^2 - \frac{3}{5} a \frac{G M^2}{R} \right].
$$

If the external pressure \\(p_0\\) is small, then equilibrium has
$$
R \approx \frac{a G M}{5 c_s^2}.
$$

For a fixed mass \\(M\\), the external pressure has a maximum allowed value such that the equation holds.

We can turn this around, and instead ask for a given pressure \\(p_0\\), what is the maximum core mass that can be in equilibrium?
$$
M_\mathrm{BE}(p_0) = 0.26 \left ( \frac{T}{10\\,\mathrm{K}} \right)^2 \left (\frac{10^6\\,\mathrm{cm}^{-3}\\,\mathrm{K}}{p_0/k} \right)^{1/2}\\, M_\odot
$$
where \\(M_\mathrm{BE}\\) is the **Bonner-Ebert** mass, i.e., pressure-bounded isothermal sphere. The Bonner-Ebert mass differs from the Jeans mass by only order unity.

Only cores with \\(M > M_\mathrm{BE}\\) are unstable to collapse, which provides a nice explanation to the fact that "typical" stars have masses of \\(\sim 1 M_\odot\\)---only cores larger than this are unstable to collapse and will become stars.

## Angular momentum problem

Self-gravitating cores will generally have non-zero angular momentum. 

But, if the cloud were to contract while preserving constant angular momentum, then the rotational kinetic energy will stop contraction of a cloud at a radius of \\(10^{16}\\,\mathrm{cm}\\), which is about five orders of magnitude larger than the size of a star!

So, it's clear that a collapsing cloud cannot contract to anything approaching the size of a star if angular momentum is conserved.

In order for the core to continue contracting, needs to transfer nearly all of its angular momentum to nearby material. The two mechanisms that can do this are
* gravitational torques: non-axisymmetric density patters in the gas (e.g., spiral arms), then gravitational torques could remove angular momentum from the clump on a time scale of order the dynamical time. But, it's unclear how non-axisymmetric a collapsing clump might be.
* magnetic torques: if the magnetic field is contributing to the support of the system against its self-gravity, then the magnetic braking time can be of order the dynamical time.