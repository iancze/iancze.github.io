---
title: Explanation of UV Coverage
date: 2021-06-21
publishdate: 2021-06-21
---

Here are some figures demonstrating the effects of UV coverage that made for various talks. I have made them available for download in the hope that they may be useful for your presentations as well, but I kindly ask that you properly attribute them (to Ian Czekala). You may download the files by right-clicking on the "Download link" and choosing "save as."


<h4>The effect of varying UV coverage</h4>
[video width="2100" height="780" mp4="https://sites.psu.edu/iczekala/files/2020/09/alma_noiseless.mp4"][/video]
<a href="https://sites.psu.edu/iczekala/files/2020/09/alma_noiseless.mp4">Download link</a>
This video shows an example of a mock sky brightness distribution (the ALMA logo, left panel) whose UV plane (central panel) is sampled by a typical 1-hr integration with a typical ALMA array configuration. The recovered dirty image (right panel) is the inverse Fourier transform of the visibility measurements, with all unsampled spatial frequencies set to 0 power. The video then iterates among different 75% subselections of baselines and reconstructs a new dirty image. There is no noise in this simulation (all visibility samples are measured exactly), so the changes in image quality are solely due to the changes in and assumptions about the sampled and unsampled spatial frequencies.

<h4>The effect of noise (same UV coverage throughout).</h4>
[video width="2100" height="780" mp4="https://sites.psu.edu/iczekala/files/2020/09/alma_noise.mp4"][/video]
<a href="https://sites.psu.edu/iczekala/files/2020/09/alma_noise.mp4">Download link</a>
This video is similar to the above, only the array configuration stays the same through all frames. The difference is that noise is added to corrupt the visibility measurements before making the dirty image. The simulation starts out with no noise and gradually increases the noise added to each visibility until it ends at a signal to noise level approximately 20 times lower than would be typical for such an ALMA observation of a protoplanetary disk source. 