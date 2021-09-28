---
title: Collisional Excitation
date: 2021-10-01
publishdate: 2021-09-25
---

* Draine Ch. 17. also Ch. 3

Why is collisional excitation important?
* puts atoms, ions, and molecules into excited states from which they can decay radiatively, leading to cooling of the gas
* puts species into excited states that are used as diagnostics of physical conditions in the gas, such as density, temperature, or radiation field

Collisional rate coefficients between initial and final state
$$
k_{if} \equiv \langle \sigma v \rangle_{i \rightarrow f}
$$
having units \\(\mathrm{cm}^3\\,\mathrm{s}^{-1}\\).

And radiative transition probability 
$$
A_{if} \equiv A_{i \rightarrow f}
$$
having units \\(\mathrm{s}^{-1}\\).

## Two-Level atom 

Consider only the ground state and the first excited state of an atom. Usually not a bad assumption, especially for getting down the basics.

Only processes acting are
* collisional excitation
* collisional de-excitation
* radiative decay

### Assume no background radiation present

Let the number densities of the two levels of the atom be given by \\(n_0\\) and \\(n_1\\), and the number density of the collisional partner (e.g., electrons) be given by \\(n_c\\). Then,

The rate of change of the excited state is
$$
\frac{d n_1}{d t} = n_c n_0 k_{01} - n_c n_1 k_{10} - n_1 A_{10}.
$$
i.e., it's equal to how quickly level 1 is populated via collisions, minus how quickly level 1 is depopulated via collisions, minus how quickly level 1 is depopulated via spontaneous emission. Steady-state solution is \\(dn_1/dt = 0\\).

The law of mass action and the principle of detailed balance allow us to relate the collisional rate coefficients by 
$$
\frac{k_{01}}{k_{10}} = \frac{g_1}{g_0} e^{-E_{10}/k T_\mathrm{gas}}.
$$
In the limit that \\(n_c \rightarrow \infty\\) (i.e., collisions thermalize the gas), the steady state solution has us bring about the relationship
$$
\frac{n_1}{n_0} = \frac{g_1}{g_0} e^{-E_{10}/k T_\mathrm{gas}}.
$$

### Allow background radiation to be present

Consider background radiation with dimensionless photon occupation number
$$
n_\gamma = \frac{c^3}{8 \pi h \nu^3} u_\nu.
$$

The rate of change of the excited state is
$$
\frac{d n_1}{d t} = n_0 \left [ n_c k_{01} + n_\gamma \frac{g_1}{g_0} A_{10} \right] - n_1 \left [ n_c k_{10} + (1 + n_\gamma) A_{10} \right]
$$
where we've written the absorption coefficient \\(B_{01}\\) in terms of \\(A_{10}\\) and canceled out some prefactors. We've also included stimulated emission in the rightmost term.

Let's rearrange for the steady-state solution
$$
\frac{n_1}{n_0} = \frac{n_c k_{01} + n_\gamma(g_1/g_0) A_{10}}{n_c k_{10} + (1 + n_\gamma) A_{10}}
$$
and examine for a few cases.

* in the limit that \\(n_\gamma \rightarrow 0\\), we have the previous result
* in the limit \\(n_c \rightarrow 0\\), and the radiation field is described by a blackbody field with \\(T_\mathrm{rad}\\), then the level populations are given by Boltzmann w/ that temperature (gas temperature could be different, though, it's just that the number density of the collisional partners isn't high enough to thermalize anything to the gas temperature).
* if the temperature of the radiation field happens to match that of the gas \\(T_\mathrm{gas}\\), then the system will be brought into equilibrium independent of the collisional density

## Critical density 

**the density at which collisional deexcitation equals radiative deexcitation, including stimulated emission**

For a collisional partner \\(c\\), and the excited state, we define this as 
$$
n_{\mathrm{crit}, u}(c) \equiv \frac{\sum_{l<u} [1 + (n_\gamma)_{ul}] A_{ul}}{\sum_{u <l} k_{ul}(c)}
$$
i.e., we're considering multi-level atoms and summing over all states \\(l\\) lower than the state under consideration (\\(u\\)).

For a two-level atom, this is 
$$
n_\mathrm{crit} = \frac{(1 + n_\gamma) A_{ul}}{k_{ul}}
$$

This definition is appropriate only when the gas is optically thin such that the radiated photons escape. If it's not, then we get "radiative trapping," which we'll discuss in a future lecture.

* In the **high density regime**, \\(n \gg n_\mathrm{crit}\\), the rate of collisional deexcitation is greater than radiative deexcitation, and the excitation temperature is driven towards the kinetic temperature. 

* In the **low density regime** \\(n < n_\mathrm{crit}\\), the rate of collisional deexcitation is less than radiative deexcitation, and the excitation temperature is driven to the radiation temperature. 

We can expand this discussion a bit by following the example from [Richard Pogge](http://www.astronomy.ohio-state.edu/~pogge/Ast871/Notes/Molecules.pdf) and discuss the specific example of a medium with two-level atoms (ratio of states given by \\(T_\mathrm{exc}\\), kinetic temperature given by \\(T\\)) with some *blackbody* radiation field with radiation temperature \\(T_\mathrm{rad}\\).

In the high density regime, we'll have the excitation temperature driven towards the kinetic temperature \\(T_\mathrm{exc} \rightarrow T\\). If \\(T > T_\mathrm{rad}\\) (common in ISM), we'll see an emission line.

In the low density regime, we'll have the excitation temperature driven towards the radiation temperature \\(T_\mathrm{exc} \rightarrow T_\mathrm{rad}\\), in which case we won't see a line.

This is why it is sometimes said that a medium needs to be above the critical density for a line to "turn on." Different species have different critical densities, and so line strengths can be used as density diagnostics.