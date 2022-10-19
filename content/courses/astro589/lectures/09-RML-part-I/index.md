---
title: Regularized Maximum Likelihood (RML) I
date: 2022-10-19
publishdate: 2022-10-17
---

* [Recording Part I: whiteboard](https://psu.mediaspace.kaltura.com/media/Astro+589A+Lecture+9A+RML+Part+I/1_3ehfryrc)
* [Zoom Part II: slides](https://psu.mediaspace.kaltura.com/media/Astro+589A+Lecture+9+part+IIA+RML/1_5k3v0oqn)

## References

* [MPoL introduction](https://mpol-dev.github.io/MPoL/rml_intro.html)
* [Machine Learning: A Probabilistic Perspective](https://catalog.libraries.psu.edu/catalog/19499523) by Murphy, Chapter 10
* [Pattern Recognition and Machine Learning](https://catalog.libraries.psu.edu/catalog/3405468) by Bishop, Chapter 8
* The fourth paper in the 2019 [Event Horizon Telescope Collaboration series](https://ui.adsabs.harvard.edu/abs/2019ApJ...875L...4E/abstract) describing the imaging principles
* [Maximum entropy image restoration in astronomy](https://ui.adsabs.harvard.edu/abs/1986ARA%26A..24..127N/abstract) AR&A by Narayan and Nityananda 1986
* [Multi-GPU maximum entropy image synthesis for radio astronomy](https://ui.adsabs.harvard.edu/abs/2018A%26C....22...16C/abstract) by Cárcamo et al. 2018
* [Regularized Maximum Likelihood Techniques for ALMA Observations](https://ui.adsabs.harvard.edu/abs/2022arXiv220911813Z/abstract) by Zawadzki, Czekala, et al.

## Last time

* Discussed \\(u,v\\) coverage and sampling (weights)
* Introduced the "dirty image" as the inverse Fourier transform of the visibility samples
* Introduced the CLEAN image deconvolution procedure

## This time

* Review parametric vs. non-parametric models
* Introduce Regularized Maximum Likelihood (RML) imaging 

## Parametric vs. Non-Parametric Models

Recall our discussion on Bayesian inference and what it means to forward-model a dataset and how to calculate a likelihood function.

Say we have a model that we are fitting to some data. In "Machine Learning: A Probabilistic Perspective," Murphy defines a parametric model as one that has a fixed number of parameters, and a non-parametric one as one where the number of parameters grows with the size of the data.

Something like the line we discussed in our Bayesian Modeling lecture is a parametric model. It has two parameters, a slope and an intercept. If we were observing a source and we wanted to fit a 2D Gaussian to the visibility function, that visibility model would also be a parametric model. Its parameters would be the width of the Gaussian, the position of the source, and the amplitude of the source.

TODO: draw example of Gaussian function and label parameters

If you've ever fit a spline to a bunch of data, then you've used a non-parametric model. A Gaussian process would also a non-parametric model. In these models there in fact are parameters (like the number or type of splines/GP kernels), but these are usually nuisances to the problem. You wouldn't necessarily fit a spline model to determine the exact number of spline position parameters, but you *are* interested in the approximation to \\(f(x)\\) that the spline has enabled you.

TODO: draw points and a spline or GP drawn through them

I would consider a CLEAN model to be a type of non-parametric model too. Through the CLEANing process, you create a model of the source emission from a collection of \\(\delta\\) functions. Each of these \\(\delta\\) functions technically has parameters, but those are mostly nuisance parameters in pursuit of their aggregate representation of the model. In general, non-parametric models have the ability to be more expressive than parametric models, but sometimes at the expense of interpretability.

## RML images as non-parametric models

Now, let me introduce a set of techniques that have been grouped under the banner "Regularized Maximum Likelihood Imaging" or RML Imaging for short. With RML imaging, we're trying to come up with a model that will fit the dataset. But rather than using a parametric model like a protoplanetary disk structure model or a series of Gaussian rings, we're using a non-parametric model of *the image itself*. This could be as simple as parameterizing the image using the intensity values of the pixels themselves, i.e.,

$$
\boldsymbol{\theta} = \{I_1, I_2, \ldots, I_{N^2} \}
$$

assuming we have an \\(N \times N\\) image.

A flexible image model for RML imaging is mostly analogous to using a spline or Gaussian process to fit a series of \\(\boldsymbol{X} = \{x_1, x_2, \ldots\, x_N\}\\) and \\(\boldsymbol{Y} = \{y_1, y_2, \ldots\, y_N\}\\) points---the model will nearly always have enough flexibility to capture the structure that exists in the dataset. The most straightforward formulation of a non-parametric image model is the pixel basis set, but we could also use more sophisticated basis sets like a set of wavelet coefficients, or even more exotic basis sets constructed from trained neural networks. These may have some serious advantages when it comes to the "regularizing" part of "regularized maximum likelihood" imaging. But first, let's talk about the "maximum likelihood" part.

Given some image parameterization (e.g., a pixel basis set of \\(N \times N\\) pixels, with each pixel `cell_size` in width), we would like to find the maximum likelihood image \\(\boldsymbol{\theta}_\mathrm{MLE}\\). Fortunately, because the Fourier transform is a linear operation, we can analytically calculate the maximum solution (the same way we might find the best-fit slope and intercept for the line example). This maximum likelihood solution is called (in the radio astronomy world) the dirty image, and its associated point spread function is called the dirty beam.

In the construction of the dirty image, all unsampled spatial frequencies are set to zero power. This means that the dirty image will only contain spatial frequencies about which we have at least some data. This assumption, however, rarely translates into good image fidelity, especially if there are many unsampled spatial frequencies which carry significant power. It's also important to recognize that dirty image is only *one* out of a set of *many* images that could maximize the likelihood function. From the perspective of the likelihood calculation, we could modify the unsampled spatial frequencies of the dirty image to whatever power we might like, and, because they are *unsampled*, the value of the likelihood calculation won't change, i.e., it will still remain maximal.

When synthesis imaging is described as an "ill-posed inverse problem," this is what is meant. There is a (potentially infinite) range of images that could *exactly* fit the dataset, and without additional information we have no way of discriminating which is best. As you might suspect, this is now where the "regularization" part of "regularized maximum likelihood" imaging comes in.

## Regularization

There are a number of different ways to talk about regularization. If one wants to be Bayesian about it, one would talk about specifying *priors*, i.e., we introduce terms like \\(p(\boldsymbol{\theta})\\) such that we might calculate the maximum a posteriori (MAP) image \\(\boldsymbol{\theta}_\mathrm{MAP}\\) using the posterior probability distribution

$$
p(\boldsymbol{\theta} |\\, \boldsymbol{V}) \propto \mathcal{L}(\boldsymbol{V} |\\, \boldsymbol{\theta}) \\, p(\boldsymbol{\theta}).
$$

For computational reasons related to numerical over/underflow, we would most likely use the logarithm of the posterior probability distribution

$$
\ln p(\boldsymbol{\theta} |\\, \boldsymbol{V}) \propto \ln \mathcal{L}(\boldsymbol{V} |\\, \boldsymbol{\theta}) + \ln p(\boldsymbol{\theta}).
$$

One could accomplish the same goal without necessarily invoking the Bayesian language by simply talking about which parameters \\(\boldsymbol{\theta}\\) optimize some objective function.

We'll adopt the perspective that we have some objective "cost" function that we'd like to *minimize* to obtain the optimal parameters \\(\hat{\boldsymbol{\theta}}\\). The machine learning community calls this a "loss" function \\(L(\boldsymbol{\theta})\\), and so we'll borrow that terminology here. For an unregularized fit, an acceptable loss function is just the negative log likelihood ("nll") term,

$$
L(\boldsymbol{\theta}) = L_\mathrm{nll}(\boldsymbol{\theta}) = - \ln \mathcal{L}(\boldsymbol{V}|\\,\boldsymbol{\theta}) = \frac{1}{2} \chi^2(\boldsymbol{V}|\\,\boldsymbol{\theta})
$$

If we're only interested in \\(\hat{\boldsymbol{\theta}}\\), it doesn't matter whether we include the 1/2 prefactor in front of \\(\chi^2\\), the loss function will still have the same optimum. However, when it comes time to add additional terms to the loss function, these prefactors matter in controlling the relative strength of each term.

When phrased in the terminology of function optimization, additional terms can be described as regularization penalties. To be specific, let's add a term that regularizes the sparsity of an image.

$$
L_\mathrm{sparsity}(\boldsymbol{\theta}) = \sum_i |I_i|
$$

In short, the L1 norm promotes sparse solutions (solutions where many pixel values are zero). The combination of these two terms leads to a new loss function

$$
L(\boldsymbol{\theta}) = L_\mathrm{nll}(\boldsymbol{\theta}) + \lambda_\mathrm{sparsity} L_\mathrm{sparsity}(\boldsymbol{\theta})
$$

Where we control the relative "strength" of the regularization via the scalar prefactor \\(\lambda_\mathrm{sparsity}\\). If \\(\lambda_\mathrm{sparsity} = 0\\), no sparsity regularization is applied. Non-zero values of \\(\lambda_\mathrm{sparsity}\\) will add in regularization that penalizes non-sparse \\(\boldsymbol{\theta}\\) values. How strong this penalization is depends on the strength relative to the other terms in the loss calculation. 

We can equivalently specify this using Bayesian terminology, such that

$$
p(\boldsymbol{\theta} |\\,\boldsymbol{V}) = \mathcal{L}(\boldsymbol{V}|\,\boldsymbol{\theta}) \\, p(\boldsymbol{\theta})
$$

where

$$
p(\boldsymbol{\theta}) = C \exp \left (-\lambda_\mathrm{sparsity} \sum_i | I_i| \right)
$$

and \\(C\\) is a normalization factor. When working with the logarithm of the posterior, this constant term is irrelevant.



The MPoL package for Regularized Maximum Likelihood imaging
-----------------------------------------------------------

*Million Points of Light* or "MPoL" is a Python package that is used to perform regularized maximum likelihood imaging. By that we mean that the package provides the building blocks to create flexible image models and optimize them to fit interferometric datasets. The package is developed completely in the open on [Github](https://github.com/MPoL-dev/MPoL)

We strive to

* create an open, welcoming, and supportive community for new users and contributors (see our `code of conduct <https://github.com/MPoL-dev/MPoL/blob/main/CODE_OF_CONDUCT.md>`__and `developer documentation <developer-documentation.html>`__)
* support well-tested (|Tests badge|) and stable releases (i.e., ``pip install mpol``) that run on all currently-supported Python versions, on Linux, MacOS, and Windows
* maintain up-to-date `API documentation <api.html>`__
* cultivate tutorials covering real-world applications

We also recommend checking out several other excellent packages for RML imaging:
* [SMILI](https://github.com/astrosmili/smili)
* [eht-imaging](https://github.com/achael/eht-imaging)
* [GPUVMEM](https://github.com/miguelcarcamov/gpuvmem)

There are a few things about  MPoL that we believe make it an appealing platform for RML modeling.

* **Built on PyTorch**: Many of MPoL's exciting features stem from the fact that it is built on top of a rich computational library that supports autodifferentiation and construction of complex neural networks. Autodifferentiation libraries like [Theano/Aesara](https://github.com/aesara-devs/aesara), [Tensorflow](https://www.tensorflow.org/), [PyTorch](https://pytorch.org/), and [JAX](https://jax.readthedocs.io/) have revolutionized the way we compute and optimize functions. For now, PyTorch is the library that best satisfies our needs, but we're keeping a close eye on the Python autodifferentiation ecosystem should a more suitable framework arrive. If you are familiar with scientific computing with Python but haven't yet tried any of these frameworks, don't worry, the syntax is easy to pick up and quite similar to working with numpy arrays. 

* **Autodifferentiation**: PyTorch gives MPoL the capacity to autodifferentiate through a model. The *gradient* of the objective function is exceptionally useful for finding the "downhill" direction in a large parameter space (such as the set of image pixels). Traditionally, these gradients would have needed to been calculated analytically (by hand) or via finite-difference methods which can be noisy in high dimensions. By leveraging the autodifferentiation capabilities, this allows us to rapidly formulate and implement complex prior distributions which would otherwise be difficult to differentiate by hand.

* **Optimization**: PyTorch provides a full-featured suite of research-grade [optimizers](https://pytorch.org/docs/stable/optim.html) designed to train deep neural networks. These same optimizers can be employed to quickly find the optimum RML image.

* **GPU acceleration**: PyTorch wraps CUDA libraries, making it seamless to take advantage of (multi-)GPU acceleration to optimize images. No need to use a single line of CUDA.

* **Model composability**: Rather than being a monolithic program for single-click RML imaging, MPoL strives to be a flexible, composable, RML imaging *library* that provides primitives that can be used to easily solve your particular imaging challenge. One way we do this is by mimicking the PyTorch ecosystem and writing the RML imaging workflow using [PyTorch modules](https://pytorch.org/tutorials/beginner/nn_tutorial.html). This makes it easy to mix and match modules to construct arbitrarily complex imaging workflows. We're working on tutorials that describe these ideas in depth, but one example would be the ability to use a single latent space image model to simultaneously fit single dish and interferometric data.

* **A bridge to the machine learning/neural network community**: MPoL will happily calculate RML images for you using "traditional" image priors, lest you are the kind of person that turns your nose up at the words "machine learning" or "neural network." However, if you are the kind of person that sees opportunity in these tools, because MPoL is built on PyTorch, it is straightforward to take advantage of them for RML imaging. For example, if one were to train a variational autoencoder on protoplanetary disk emission morphologies, the latent space + decoder architecture could be easily plugged in to MPoL and serve as an imaging basis set.