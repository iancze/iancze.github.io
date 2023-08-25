---
title: Regularized Maximum Likelihood (RML) 
date: 2023-09-27
publishdate: 2023-08-25
---


## References for today

* [MPoL introduction](https://mpol-dev.github.io/MPoL/rml_intro.html)
* [Data Analysis: a Bayesian Tutorial](https://catalog.libraries.psu.edu/catalog/19551280) by Sivia and Skilling
* [Data Analysis Recipes: Fitting a Model to Data](https://ui.adsabs.harvard.edu/abs/2010arXiv1008.4686H/abstract), by Hogg et al.
* [Data analysis recipes: Probability calculus for inference](https://ui.adsabs.harvard.edu/abs/2012arXiv1205.4446H/abstract) by Hogg
* [Machine Learning: A Probabilistic Perspective](https://catalog.libraries.psu.edu/catalog/19499523) by Murphy, Chapter 10
* [Pattern Recognition and Machine Learning](https://catalog.libraries.psu.edu/catalog/3405468) by Bishop, Chapter 8
* [Probabilistic Graphical Models](https://catalog.libraries.psu.edu/catalog/32525864) by Sucar, especially Chapters 7, 8

## Last time

* general interferometer with north-south and east-west baselines
* arrays with multiple antennas and Earth aperture synthesis
* point spread functions (PSFs) and their relationship to array configuration

## Today's lecture

Now that we've covered how interferometers work to observe a source and the Fourier transform theory behind that, we're going to focus on the data products (called "visibilities") and Bayesian inference techniques for analyzing the data in its natural space. In subsequent lectures we will talk about how we might use the visibilities and Fourier inversion techniques to synthesize images of the source, but this lecture occupies an important intermediate (and foundational) step, where we are treating the visibilities as the "raw" data product and thinking about how we bring our analysis techniques into that space.

The topics we will cover include:

* Bayesian inference
* Forward-modeling with a *generative* model
* Complex-valued noise and measurement (weights)
* Statistical weight and relationship to point source uncertainty
* Forward-modeling visibility data
* Missing spatial frequencies (model constraints)

## Probability calculus and Bayesian Inference

* A good reference for this section is [Data analysis recipes: Probability calculus for inference](https://ui.adsabs.harvard.edu/abs/2012arXiv1205.4446H/abstract) by Hogg

Generally, we write probability distributions like \\(p(a)\\). The probability distribution is a function describing the probability of the variable \\(a\\) having some value.

Probability functions are normalized.
$$
\int_{-\infty}^\infty p(a)\\,\mathrm{d}a = 1
$$

Say that \\(a\\) represents the height of an individual from the US population, measured in meters.

TODO: draw a bell curve

Probability functions **have units**. In this case, \\(p(a)\\) has units of \\(a^{-1}\\), or \\(\mathrm{m}^{-1}\\).

If selected an individual from the US population and we wanted to know the probability that their height was between 1.7 and 1.9 meters, we could do an integral over this range

$$
\int_{1.7\\,\mathrm{m}}^{1.9\\,\mathrm{m}} p(a)\\,\mathrm{d}a
$$

### Conditional probabilities

It's very common that we will talk about multiple parameters at the same time. For example, we can continue our example and let \\(b\\) be the age of an individual drawn from the US population. We can talk about the probability of an individual drawn from the US population having a certain height *given* that we know they are 20 years old

$$
p(a | b = 20\\,\mathrm{yr}).
$$

What are the units of this probability distribution? It's actually the same as before, it's \\(a^{-1}\\), or \\(\mathrm{m}^{-1}\\), because this probability distribution must obey the same normalization

$$
\int_{-\infty}^\infty p(a | b = 20\\,\mathrm{yr})) \\,\mathrm{d}a = 1
$$

You can't do the integral
$$
\int p(a | b ) \\,\mathrm{d}b
$$
because this integrand now has units of \\(a^{-1}b\\), which is nonsensical.

### Factorizing probabilities

Now let's consider the probability distribution
$$
p(a,b)
$$
this is a two-dimensional probability distribution. It has units of \\((ab)^{-1}\\), or in our previous example \\(\mathrm{m}^{-1}\\, \mathrm{yr}^{-1}\\). You can read this as the probability of an individual having \\(a\\) value of height *and* \\(b\\) value of age, i.e., this is a *joint* probability distribution.

The same normalization rules apply, only now these need to be done over two dimensions.

You can take *any* joint probability distribution and factor it into conditional distributions. So, we could write \\(p(a,b)\\) in two different ways
$$
p(a,b) = p(a) p(b | a)
$$
or
$$
p(a,b) = p(a|b) p(b).
$$
So, in words, we can say that the probability of \\(a\\) *and* \\(b\\) (the left hand side) is equal to the probability of \\(a\\) *times* the probability of \\(b\\) given \\(a\\).

As I said, this factorization can apply to any joint probability distribution.

Side note that *if*
$$
p(b | a) = p(b)
$$
we would say that \\(a\\) and \\(b\\) are independent variables, and so we would write
$$
p(a, b) = p(a) p(b),
$$
but this isn't true in the general case, only if the variables are independent.

We can put the two factorization equations together to arrive at another relationship
$$
p(a | b) = \frac{p(b|a) p(a)}{p(b)}
$$
which is called Bayes's theorem.

### Marginalization

One amazing thing you can do with probability distributions is *marginalization*. Say that I told you the joint distribution \\(p(a, b)\\). As we talked about, we said this distribution had units of \\((ab)^{-1}\\) or \\(\mathrm{m}^{-1}\\, \mathrm{yr}^{-1}\\). But let's say you only cared about \\(p(a)\\), the distribution of heights. We can marginalize away the variable we don't want by integration
$$
p(a) = \int p(a, b)\\,\mathrm{d}a.
$$

As we'll see in a moment, this has huge implications when it comes time to do inference.

## Likelihood functions

Now, let's revisit Bayes's rule and rewrite it like
$$
p(\mathrm{hypothesis} | \mathrm{data}) \propto p(\mathrm{data} | \mathrm{hypothesis}) \times p(\mathrm{hypothesis}).
$$
We're omitting a constant of proportionality, commonly called the Bayesian evidence.

The term on the left hand side is called a posterior distribution and is really a wonderful thing to report at the end of your analysis. Say you collected many years of measurements on the positions (orbits) of Jupiter, Saturn, and their moons, and then used those measurements to infer the mass of Saturn (as Laplace famously did). The posterior you would be most interested in would be a 1D distribution of the probability of the mass of Saturn and would represent your degree of belief that Saturn truly had that particular mass. This would be conditional on all of the measurements you made.

The term on the very right hand side is a prior probability distribution and expresses your belief about the mass of Saturn in the absence of data. For example, we might rightly say that the mass needed to be greater than zero, and less than the mass of the Sun. A simple prior would then ascribe equal probabilities to all values in between (or perhaps equal probabilities to the *logarithm* of the mass of Saturn).

The remaining term, \\(p(\mathrm{data} | \mathrm{hypothesis}) \\) is called a likelihood function, and it is really where the rubber meets the road in most statistical analyses. Simply put, the likelihood function is how probable the observed data is for a given setting of the hypothesis. So, what is the probability of obtaining the observed positional measurements of Jupiter, Saturn, and their moons *if* the mass of Saturn were \\(5\times 10^{26}\\,\mathrm{kg}\\), for example.

A quick note that likelihood functions show up in frequentist analysis all the time, too. However, the interpretations of probability are different.

### Fitting a line

Let's dive into a quick example with some real data to make these concepts clearer.

Typically, when astronomers fit a model to some dataset, such as a line \\(y = m x + b\\) to a collection of \\(\boldsymbol{X} = \{x_1, x_2, \ldots\, x_N\}\\) and \\(\boldsymbol{Y} = \{y_1, y_2, \ldots\, y_N\}\\) points, we require a likelihood function. Simply put, the likelihood function specifies the probability of the data, given a model, and encapsulates our assumptions about the data and noise generating processes.

TODO: draw a bunch of points for putting a line through.

For most real-world datasets, we don't measure the "true" \\(y\\) value of the line (i.e., \\(mx + b\\)), but rather make a measurement which has been partially corrupted by some "noise." In that case, we say that each \\(y_i\\) data point is actually generated by

$$
y_i = m x_i + b + \epsilon
$$

where \\(\epsilon\\) is a noise realization from a standard [normal distribution](https://en.wikipedia.org/wiki/Normal_distribution) with standard deviation \\(\sigma\\), i.e.,

$$
\epsilon \sim \mathcal{N}(0, \sigma).
$$

This information about the data and noise generating process means that we can write down a likelihood function to calculate the probability that we observe the data that we do, given a set of model parameters. The likelihood function is \\(p(\boldsymbol{Y} |\boldsymbol{\theta})\\). Sometimes it is written as \\(\mathcal{L}(\boldsymbol{Y} |\boldsymbol{\theta})\\), and frequently, when employed in computation, we'll use the logarithm of the likelihood function, or "log-likelihood," \\(\ln \mathcal{L}\\) to avoid numerical under/overflow issues.

Let's call \\(\boldsymbol{\theta} = \{m, b\}\\) and \\(M(x_i |\, \boldsymbol{\theta}) = m x_i + b\\). This is a very simple example here, but we would still call \\(M\\) a forward or *generative* model. By that, we mean our model is sophisticated enough that we can use it (and some noise model) to fully replicate the dataset, or alternative sets of data indistinguishable from the measured data.

The probability of observing each datum is a Gaussian (normal distribution) centered on the model value, evaluated at the \\(y_i\\) value. So, the full likelihood function for this line problem is just the multiplication of all of these probability distributions

$$
\mathcal{L}(\boldsymbol{Y} |\boldsymbol{\theta}) = \prod_i^N \frac{1}{\sqrt{2 \pi} \sigma} \exp \left [ - \frac{(y_i - M(x_i |\boldsymbol{\theta}))^2}{2 \sigma^2}\right ].
$$

The logarithm of the likelihood function is

$$
\ln \mathcal{L}(\boldsymbol{Y} |\,\boldsymbol{\theta}) = -N \ln(\sqrt{2 \pi} \sigma) - \frac{1}{2} \sum_i^N \frac{(y_i - M(x_i |\boldsymbol{\theta}))^2}{\sigma^2}.
$$

You may recognize the right hand term looks similar to the \\(\chi^2\\) metric,

$$
\chi^2(\boldsymbol{Y} |\boldsymbol{\theta}) = \sum_i^N \frac{(y_i - M(x_i |\boldsymbol{\theta}))^2}{\sigma^2}
$$

Assuming that the uncertainty (\\(\sigma\\)) on each data point is known (and remains constant), the first term in the log likelihood expression remains constant, and we have

$$
\ln \mathcal{L}(\boldsymbol{Y} |\boldsymbol{\theta}) = - \frac{1}{2} \chi^2 (\boldsymbol{Y} |\boldsymbol{\theta}) + C
$$

where \\(C\\) is a constant with respect to the model parameters. It is common to use shorthand to say that "the likelihood function is \\(\chi^2\\)" to indicate situations where the data uncertainties are Gaussian. Very often, we (or others) are interested in the parameter values \\(\boldsymbol{\theta}_\mathrm{MLE}\\) which maximize the likelihood function. Unsurprisingly, these parameters are called the *maximum likelihood estimate* (or MLE), and usually they represent something like a "best-fit" model.

When it comes time to do parameter inference, however, it's important to keep in mind

1) the simplifying assumptions we made about the noise uncertainties being constant with respect to the model parameters. If we were to "fit for the noise" in a hierarchical model, for example, we would need to use the full form of the log-likelihood function, including the \\(-N \ln \left (\sqrt{2 \pi} \sigma \right)\\) term.
2) that in order to maximize the likelihood function we want to *minimize* the \\(\chi^2\\) function.
3) that constants of proportionality (e.g., the 1/2 in front of the \\(\chi^2\\)) can matter when combining likelihood functions with prior distributions for Bayesian parameter inference.

To be specific, \\(\chi^2\\) is not the end of the story when we'd like to perform Bayesian parameter inference. To do so, we need the posterior probability distribution of the model parameters given the dataset, \\(p(\boldsymbol{\theta}|\,\boldsymbol{Y})\\).

## Visibility Data

Now that we have reviewed likelihood functions, let's turn back to radio astronomy and go into further detail *how* the visibility function is sampled. Recall that the visibility domain is the Fourier transform of the image sky brightness \\(\mathcal{V} \leftrightharpoons I\\).

The visibility function is complex-valued, and each measurement of it (denoted by \\(V_i\\)) is made in the presence of noise

$$
V_i = \mathcal{V}(u_i, v_i) + \epsilon.
$$

Here \\(\epsilon\\) represents a noise realization from a [complex normal](https://en.wikipedia.org/wiki/Complex_normal_distribution) (Gaussian) distribution. Thankfully, most interferometric datasets *do not* exhibit significant covariance between the real and imaginary noise components *and* the distributions of the values are similar, so we could equivalently say that the real and imaginary components of the noise are separately generated by draws from normal distributions characterized by standard deviation \\(\sigma\\)

$$
\epsilon_\Re \sim \mathcal{N}(0, \sigma)
$$

$$
\epsilon_\Im \sim \mathcal{N}(0, \sigma)
$$

where \\(\sigma\\) is a real-valued quantity. If the units of the visibility function are Janskys, then the units of \\(\sigma\\) are also Janskys.

The full complex noise-draw is given by
$$
\epsilon = \epsilon_\Re + i \epsilon_\Im.
$$

Radio interferometers will commonly represent the uncertainty on each visibility measurement by a "weight" \\(w_i\\), where

$$
w_i = \frac{1}{\sigma_i^2}.
$$

Like \\(\sigma\\), the weight itself is a real quantity, in this case having units of \\(1/\mathrm{Jy}^2\\).

A full interferometric dataset is a collection of visibility measurements, which we represent by

$$
\boldsymbol{V} = \\{V_1, V_2, \ldots V_N\\}_{i=1}^N
$$

each one having a corresponding \\(u_i, v_i\\) coordinate. For reference, a typical ALMA dataset might contain a half-million individual visibility samples, acquired over a range of spatial frequencies.

### Likelihood functions for Fourier data

Now that we've introduced likelihood functions in general and the specifics of Fourier data, let's talk about likelihood functions for inference with Fourier data. As before, our statement about the data generating process

$$
V_i = \mathcal{V}(u_i, v_i) + \epsilon
$$

leads us to the formulation of the likelihood function.

First, let's assume we have some model that we'd like to fit to our dataset. To be a forward model, it should be able to predict the value of the visibility function for any spatial frequency, i.e., we need to be able to calculate

$$
\mathcal{V}(u, v) = M_\mathcal{V}(u, v |, \boldsymbol{\theta}).
$$

Following the discussion about how the complex noise realization \\(\epsilon\\) is generated, this leads to a log likelihood function

$$
\ln \mathcal{L}(\boldsymbol{V}|\,\boldsymbol{\theta}) = - \frac{1}{2} \chi^2(\boldsymbol{V}|\,\boldsymbol{\theta}) + C
$$

Because the data and model are complex-valued, \\(\chi^2\\) is evaluated as

$$
\chi^2(\boldsymbol{V}|\,\boldsymbol{\theta}) = \sum_i^N \frac{|V_i - M_\mathcal{V}(u_i, v_i |\,\boldsymbol{\theta})|^2}{\sigma_i^2}
$$

where \\(| |\\) denotes the modulus squared. Equivalently, the calculation can be broken up into sums over the real and imaginary components of the visibility data and model

$$
\chi^2(\boldsymbol{V}|\,\boldsymbol{\theta}) = \sum_i^N \frac{(V_{\Re,i} - M_\mathcal{V,\Re}(u_i, v_i |\,\boldsymbol{\theta}))^2}{\sigma_i^2} + \sum_i^N \frac{(V_{\Im,i} - M_\mathcal{V,\Im}(u_i, v_i |\,\boldsymbol{\theta}))^2}{\sigma_i^2}.
$$

Because images of the sky are real, therefore the real part of the visibility function must always be even and the imaginary part odd. The visibility function is Hermitian. This means that

$$
\mathcal{V}(u, v) = \mathcal{V}^{\*}(-u, -v).
$$

So, if you make a measurement of \\(\mathcal{V}(u, v)\\), this means you have also made the same measurement of \\(\mathcal{V}^{\*}(-u, -v)\\). If you are doing forward-modeling of the visibilities as we just described, you only need to use one of the Hermitian pairs, otherwise you will double count your measurements (this only turns out to be a scale factor in the likelihood for most analysis, so it's technically OK). If you are gridding the visibilities to then image them, however, you will certainly want to include the Hermitian pairs. Otherwise, your image will not turn out to be real!

### Point source sensitivity

We'll use our forward modeling formalism to fit for the flux of a point source.

Se also [Casussus and Carcamo](https://ui.adsabs.harvard.edu/abs/2022MNRAS.513.5790C/abstract) 2022, appendix A4.

## More complex visibility models

It's difficult to reason about all but the simplest models directly in the Fourier plane, so usually models are constructed in the image plane \\(M_I(l,m |,\boldsymbol{\theta})\\) and then Fourier transformed (either analytically, or via the FFT) to construct visibility models

$$
M_\mathcal{V}(u, v |, \boldsymbol{\theta}) \leftrightharpoons M_I(l,m |,\boldsymbol{\theta})
$$

For marginally resolved sources, it's common to fit simple models like a 2D Gaussian. We can write down an image plane model and then calculate its Fourier transform analytically.

But, these could be more complicated models. For example, these models could be channel maps of carbon monoxide emission from a rotating protoplanetary disk (as in [Czekala et al. 2015](https://ui.adsabs.harvard.edu/abs/2015ApJ...806..154C/abstract), where \\(\boldsymbol{\theta}\\) contains parameters setting the structure of the disk), or rings of continuum emission from a protoplanetary disk (as in [Guzmán et al. 2018](https://ui.adsabs.harvard.edu/abs/2018ApJ...869L..48G/abstract), where \\(\boldsymbol{\theta}\\) contains parameters setting the sizes and locations of the rings).

With the likelihood function specified, we can add prior probability distributions \\(p(\boldsymbol{\theta})\\), and calculate and explore the posterior probability distribution of the model parameters using algorithms like Markov Chain Monte Carlo. In this type of Bayesian inference, we're usually using forward models constructed with a small to medium number of parameters (e.g., 10 - 30), like in the protoplanetary disk examples of [Czekala et al. 2015](https://ui.adsabs.harvard.edu/abs/2015ApJ...806..154C/abstract) or [Guzmán et al. 2018](https://ui.adsabs.harvard.edu/abs/2018ApJ...869L..48G/abstract).

All of these type of models would be called *parametric* models, because we can represent the model using a finite set of parameters. E.g., for the Gaussian model, we have width in the major and minor axes, rotation angle, and 2D position. So these parameters fully represent the model. One thing you need to be concerned with inference using parametric models is whether you have the right model! If your source is actually a ring instead of a Gaussian, the posterior distribution of your parameters can be rendered meaningless.

Forward modeling with (simple) parametric models can be very useful for understanding

## Discussion about model mis-specification and unsampled visibilities (model constraints)

Could use u / v undersampling to constrain width or size, say.


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



## Last time

* Recap of (parametric) forward modeling in a Bayesian context
* Recap of the CLEAN procedural image deconvolution algorithm
* Introduction of RML process as a non-parametric model
* Discussion of regularization, in the context of priors
* Discussion of loss function space (defined by probability distribution) vs. the optimization engineering that helps you navigate it

## Today

* Overarching question---how do you assess whether something is good? Forays into Machine Learning
* Deeper dive into future RML applications and opportunities

## Model comparison

Last time we talked about the difference between parametric and non-parametric models, i.e., the difference between fitting a line with slope and intercept vs. fitting a spline or Gaussian process. And we made the simple distinction that a parametric model has a fixed number of parameters, whereas a non-parametric model generally has parameters that grow with your number of data points. The truth is that in several contexts these exist as part of a continuum.

Today we're going to take a journey along this continuum and examine some of the failure modes that can come about. The discussion will first be general and applicable to *many* problems, but then we'll zero in on the case of interferometric imaging (both CLEAN and RML) specifically.

Let's think of a polynomial basis. If you recall back to one of our first lectures, where I asked you to draw a function through a set of discrete samples. Let's narrow in on the specific case of a polynomial basis set of degree \\(N\\), where \\(N\\) is the number of terms. I'm going to write it like this

$$
y = a_0 + a_1 x + a_2 x^2 + a_3 x^3 + \ldots.
$$
but similar arguments apply to Legendre polynomials or Chebyshev polynomials, etc, and in practice it's better to use on of those for your actual fitting problem.

But lets say we have 10 data points. We can fit a 0th order polynomial, 1st order, etc.., using say a \\(\chi^2\\) likelihood function. And then we can get to a polynomial of degree ten. Who has heard the common critique "your model has more parameters than data, it's so flexible". This situation is where the common advice against using a model with more parameters than your data comes from, and when people utter this critique, I think this is the situation that they are referring to.

What are the criticisms of this model? On one hand, it has fit the data *perfectly*. It's done what we've asked. On the other hand, it doesn't seem to do what we want. If we were to get new data, our model probably wouldn't be that useful.

What can we do? Well, the common wisdom would have us stick to models with fewer parameters. But, this is being a little shortsighted. Instead, we can add a *regularization* that discourages the fit from taking on large amplitudes in many of the terms. One type of regularization is "ridge regression," also called Tikhonov or L2 regularization. It adds an extra term to the fit metric that says

$$
\lambda \sum_{i=0}^{N-1}|a_i|^2
$$

then we find that the amplitudes of those higher order terms (which might not be necessary) will be diminished. If you recall from very early in the semester, where we talked about the concept of band-limited signals, this regularization is related. In the limit we let \\(N\rightarrow \infty\\), we arrive at a Gaussian process, and the autocovariance of the function (the Gaussian process kernel) is related to the power spectrum of the signal. If we say the signal is band-limited, then that puts a *cap* on the number of higher order terms that we can actually use.

This math is fully equivalent to the RML imaging problem we introduced last week, and it also raises the same problem: how do we set the regularizer strength? What is the best choice?

## Cross validation

* Useful thoughts from <https://biometry.github.io/APES/LectureNotes/2017-Resampling/CrossValidationLecture.html>

The idea is to test the *predictive power* of your model. In this case, the model would be your setup of your. In the RML case, the model would be the settings of your image pixelization,

If we have *the right model*, we will generalize perfectly to new data. The problem is that our training data are always limited and will usually always have some noise.

The problem of non-independence of your random hold-outs. When the data are small, it is possible to overfit your cross validation. This is a hard place to be in, especially when getting new data is expensive.
