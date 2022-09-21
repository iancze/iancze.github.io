---
title: PSFs, Gridding, and Dirty Images
date: 2022-09-28
publishdate: 2022-08-21
draft: true
---



The instantaneous output of the interferometer corresponds to two delta functions situated at \\(\pm u_0\\). Here we see that the interferometer acts as a *spatial filter* that only responds to the two spatial frequencies \\(\pm u_0\\).

The negative spatial frequency doesn't have a physical meaning but is a mathematical convenience. Because the intensity distribution on the sky is a real quantity, the visibility function itself is symmetric about the origin in a Hermitian sense, meaning it has real even parts and odd imaginary parts.

## Moving on to the general case

### Coordinates

Let's consider a generic situation of a two-element interferometer observing (tracking) a source on the sky with phase center \\(\mathbf{s}_0\\).

{{< figure src="tms-general-coord.png" caption="TMS Fig 3.1" >}}

An element of the source with solid angle \\(d\Omega\\) at some position \\(\mathbf{s} = \mathbf{s}_0 + \mathbf{\sigma}\\) will contribute an element of power \\( \frac{1}{2} A(\sigma) I(\sigma) \Delta \nu d\Omega\\), where \\(A\\) is some (normalized) power pattern of a single antenna. For now, you can consider it to be a smooth function that is effectively constant.

From what we just talked about for the 1D example, the component of the correlator output will be equal to the received power and to the fringe term \\(\cos(2 \pi \nu \tau_g)\\).

Let \\(\mathbf{D}_\lambda\\) be a baseline vector which points from the central antenna to the other one, and specifies the baseline length in multiples of the observing wavelength. Then

$$
\nu \tau_g = \mathbf{D}_\lambda \cdot \mathbf{s} = \mathbf{D} \cdot (\mathbf{s}_0 + \mathbf{\sigma})
$$

and the output from the correlator is
$$
r(D_\lambda, \mathbf{s}_0) = \Delta \nu \int_{4 \pi} A(\mathbf{\sigma}) I(\sigma) \cos [2 \pi D_\lambda \cdot (\mathbf{s}_0 + \mathbf{\sigma})]\\,\mathrm{d}\Omega
$$
which we can split up into
$$
r(D_\lambda, \mathbf{s}_0) =
\Delta \nu \cos(2 \pi D_\lambda \cdot s_0)  \int_{4 \pi}  A(\mathbf{\sigma}) I(\sigma) \cos(2 \pi D_\lambda \cdot \sigma)\\,\mathrm{d}\Omega - \Delta \nu \sin(2 \pi D_\lambda \cdot s_0) \int_{4 \pi}  A(\mathbf{\sigma}) I(\sigma) \sin(2 \pi D_\lambda \cdot \sigma)\\,\mathrm{d}\Omega
$$

Let us define the complex visibility as
$$
\mathcal{V} = |\mathcal{V}|e^{i \phi} = \int_{4 \pi} A_N(\sigma) I(\sigma) e^{-i 2 \pi D_\lambda \cdot \sigma}\\,\mathrm{d}\Omega
$$

and then we can rewrite the response from the correlator as
$$
r(D_\lambda, \mathbf{s}_0) = A_0 \Delta \nu |\mathcal{V}| \cos(2 \pi D_\lambda \cdot \mathbf{s}_0 - \phi).
$$

So now we can describe the response of the correlator in terms of a fringe function (cosine) which depends on the antenna position and projected baseline and a *visibility function* which is independent of the array entirely, it is just this spatial integral over the source brightness (modulo the antenna power pattern, which we said to ignore) that looks suspiciously like a Fourier transform.

### 3D coordinates

Now that we've introduced how the visibility function comes about in a general vector formalism, let's get concrete with respect to coordinates on Earth and in the sky.

{{< figure src="tms-uv-curved.png" caption="Credit: TMS Fig 3.2. Note that the \\(l\\) and \\(m\\) coordinates are technically index the *flat* image plane tangent to \\(\mathbf{s_0}\\), not curved as they are shown here." >}}

The key is to focus in on the \\(u,v\\) plane in the bottom of the figure, which is actually a 3D space and includes the \\(w\\) component, which points towards phase center. The plane is centered on \\(u=0,v=0\\) at the location of one of the antennas.

Recall that the dot product between two vectors is
$$
\mathbf{a} \cdot \mathbf{b} = ||a|| \\; ||b|| \cos \theta
$$

From our previous discussion, we have
$$
\mathbf{D}_\lambda \cdot \mathbf{s}_0 = w
$$
i.e., the \\(u,v\\) plane is oriented orthogonal to the vector pointing towards phase center. The baseline vector itself does not necessarily live in the \\(w = 0\\), plane, it can have a non-zero \\(w\\) component.

And we have
$$
\mathbf{D}_\lambda \cdot \mathbf{s} = \left ( ul + vm + w\sqrt{1 - l^2 - m^2} \right).
$$
Now, hopefully you see why it was convenient to use \\(l, m\\) as direction cosines!

We also have
$$
d \Omega = \frac{\mathrm{d}l\\; \mathrm{d} m}{\sqrt{1 - l^2 - m^2}}.
$$
I.e., as we move further and further from the phase center, \\(\mathrm{d}\Omega\\) grows larger, because it is actually on the image tangent plane.

{{< figure src="2D-uv-plane.png" caption="Credit: TMS Fig 2.7" >}}

We also have the third direction cosine, w.r.t. the \\(w\\) axis, which is
$$
n = \sqrt{1 - l^2 - m^2}
$$

With these relationships in hand, we can rewrite the visibility function as
$$
\mathcal{V}(u, v, w) = \int_{-\infty}^\infty \int_{-\infty}^\infty A_N(l,m) I(l,m) \exp \left \\{ -i 2 \pi \left [ ul + vm + w \left ( \sqrt{1 - l^2 - m^2} - 1 \right )\right ]   \right \\} \\;\frac{\mathrm{d}l\\;\mathrm{d}m}{\sqrt{1 - l^2 - m^2}}.
$$

The factor in the exponential comes about from the measurement of angular position with respect to phase center, as we saw in the "general coordinates" example.

*If* all of the measurements could be made with the antennas in a plane normal to the \\(w\\) direction such that \\(w=0\\), then we would turn this equation into an exact 2D transform. But this isn't usually the case and we need to make approximations.

So long as we are in the small-field regime and \\(l\\) and \\(m\\) are small enough such that the term
$$
\left ( \sqrt{1 - l^2 - m^2} - 1 \right ) w \simeq - \frac{1}{2}(l^2 + m^2)w
$$
can be neglected, then we have
$$
\mathcal{V}(u, v, w) = \int_{-\infty}^\infty \int_{-\infty}^\infty \frac{A_N(l,m) I(l,m)}{\sqrt{1 - l^2 - m^2}} \exp \left \\{ -i 2 \pi \left [ ul + vm \right ]   \right \\} \\;\mathrm{d}l\\;\mathrm{d}m.
$$

OK! So we've arrived at the result that I told you about at the beginning of class, that the visibility function is the Fourier transform of the sky brightness (modified by the primary beam of each antenna, which we can mostly ignore for this discussion as a constant). The approximation we made for the \\(w\\) term places a limit on the maximum size of the field that we can image (at once). There are approaches designed to overcome this scenario, but at least in the context of this course we will restrict our discussion to those that don't require it. This is generally the case for all images made with VLA or ALMA for a single pointing (i.e., imaging the full primary beam).

---

What are the units of \\(\mathcal{V}\\) itself? We can get at this by looking at the units of \\(I_\nu(l,m)\\) and how we carried out the Fourier transform integral.

If we parameterized our image using \\(\mathrm{Jy} / \mathrm{arcsec}^2\\) and we integrated over \\( \mathrm{d}l\\, \mathrm{d}m\\) (both assuming they had units of arcsec), then \\(\mathcal{V}\\) must have units of Jy. I.e., you can think of it sort of like the flux being observed at that angular scale. The visibility function is complex-valued, so if you want to discuss "power" at some angular scale then you should consider \\(|\mathcal{V}|^2\\).

### Multiple antennas

With multiple pairs of antennas, we can build up an improved point source response, because the fringe patterns of the individual two-element interferometers add up.

{{< figure src="building_a_beam.png" link="https://www.cv.nrao.edu/~sransom/web/Ch3.html#S7" caption="How the fringe patterns of the individual two-element interferometers in an array build up to create a beam. Each shaded circle represents an antenna spaced out along a 1D line. Credit: Essential Radio Astronomy.">}}

In the limit of many antennas, the beam usually looks like something centralized, approximately Gaussian, but usually containing many sidelobes (hence, why radio astronomers call this a "dirty beam").

The resolution of the beam is on the order of \\(\lambda/b\\), where \\(b\\) is the projected baseline of the longest antenna pair. Note that this resolution strongly depends on the relative number of baselines at long/short separations, since what you're really talking about is sensitivity at certain spatial scales. We'll return to this in the next lecture. It won't suffice to just have a single long-baseline pair of antennas if most other antenna pairs are at much shorter baselines.

---

## Wrapping up

A "slightly extended source" is something that is larger than the dirty beam but smaller than the primary beam of each telescope. Instantaneous field of view of an interferometer is the same as the primary beam of each telescope, treated as a single dish (see previous section). Each single dish antenna is still seeing the same thing as before, it's just that we have a correlator backend that's doing things with the signals, allowing us to create a *synthesized beam* that is considerably smaller than the size of the primary beam. For example, at 220 GHz (band 6), ALMA has a primary beam of about 20 arcseconds in diameter. However, it's common to make synthesized beams on the size of 0.1 arcseconds or smaller.




---

* Complex noise and measurement
For more on this, see the [MPoL notes on likelihood functions](https://mpol-dev.github.io/MPoL/rml_intro.html).
* Images are real, therefore the real part of the visibility function must always be even and the imaginary part odd. The visibility function is Hermitian.
* Structure of "natural" images, what the amp, phase, etc.. looks like for real scenes
