---
title: Introduction to Radio Astronomy and ALMA
date: 2021-09-20
draft: true
---

Resources:

* [Essential Radio Astronomy](https://www.cv.nrao.edu/~sransom/web/xxx.html) by James Condon and Scott Ransom (HTML book)
* [Interferometry and Synthesis in Radio Astronomy, 3rd Edition](https://ui.adsabs.harvard.edu/abs/2017isra.book.....T/abstract) by Thompson, Moran, and Swenson. Electronically available from PSU library.
* [Tools of Radio Astronomy](https://ui.adsabs.harvard.edu/abs/2013tra..book.....W/abstract) by Wilson and Hüttemeister. Also electronically available from PSU library.
* [NRAO Summer School Lectures and Slides](http://www.cvent.com/events/virtual-17th-synthesis-imaging-workshop/agenda-0d59eb6cd1474978bce811194b2ff961.aspx)


*Disclaimer*: Radio astronomy (including radio interferometry) is a *vast* subject area worthy of several full length courses. I encourage you to check out the resources above to survey the many topical areas that we won't be able to cover in this lecture.

*Goal*: provide enough of an introduction to radio astronomy, radio interferometry, and specifically ALMA, such that you see some of the connections possible with our mock TAC and scientific justification later in the course.

## Atmospheric Windows

{{< figure src="atmospheric_windows.jpg" link="https://www.cv.nrao.edu/~sransom/web/Ch1.html#S1" caption="Atmospheric windows for astronomy. Credit: ESA/Hubble (F. Granato) and Essential Radio Astronomy.">}}

By comparison to optical windows, the radio region of the spectrum is downright transparent. Only towards very high radio/microwave frequencies (wavelengths about 1 mm), does the atmospheric transmission start to decline. 

The same 
$$
\theta \approx \frac{\lambda}{D}
$$
applies for radio antennas, as well. 

Take a \\(\lambda = 1\\;\mathrm{cm}\\) observation, for example. Compared to an optical \\(\lambda = 500\\;\mathrm{nm}\\) telescope the same size, the resolution will be a factor of 
$$
\frac{1\\;\mathrm{cm}}{500\\;\mathrm{nm}} = 20,000
$$
worse. Yikes!

Radio astronomers are constantly trying to find ways to increase angular (spatial) resolution. 
* One way is to build bigger telescopes, such as the Green Bank Telescope (100m diameter)
* Another way is to work at higher frequencies (shorter wavelengths), e.g. sub-mm radio astronomy
* A final way is to use *interferometry*



Outline:
* Atmospheric windows, including ALMA transmission (from Essential Radio 1)
* Definition of a "beam"
* Interferometers (ER Ch 3.7), including FT examples from David. Show GIF of subsampled reconstructions.
* Note how imaging requires inverse FT, then CLEAN or RML techniques
* Science capabilities of ALMA, following ALMA primer and other materials
