---
title: Bayesian Inference, Complex Measurement, and Model Fitting
date: 2022-10-05
publishdate: 2022-10-04
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
* Probabilistic Graphical Models (PGMs)
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


### Probabilistic Graphical Models

In my opinion, the most concise introduction to the topic (with a focus on Bayesian inference) is provided by [Pattern Recognition and Machine Learning](https://catalog.libraries.psu.edu/catalog/3405468) by Bishop, Chapter 8.


First, though, let's take a brief moment to talk about probabilistic graphical models or PGMs. They are an extremely useful concept for understanding and reasoning about many types of data analysis problems.

* Basic graphical notation and relationship to writing equations


Chain rule for probabilities. 

Deterministic transformations written as a double line. Could use the example of fitting \\(\ln x\\) rather than \\(x\\).

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

## Probabilistic graphical models

We can wrap up this example by exploring how we might write a PGM for the problem.

TODO:


## Visibility Data 

Now that we have reviewed likelihood functions, let's turn back to radio astronomy and go into further detail *how* the visibility function is sampled. To recall, the visibility domain is the Fourier transform of the image sky brightness \\(\mathcal{V} \leftrightharpoons I\\).

The visibility function is complex-valued, and each measurement of it (denoted by \\(V_i\\)) is made in the presence of noise

$$
V_i = \mathcal{V}(u_i, v_i) + \epsilon.
$$

Here \\(\epsilon\\) represents a noise realization from a [complex normal](https://en.wikipedia.org/wiki/Complex_normal_distribution) (Gaussian) distribution. Thankfully, most interferometric datasets *do not* exhibit significant covariance between the real and imaginary noise components, so we could equivalently say that the real and imaginary components of the noise are separately generated by draws from normal distributions characterized by standard deviation \\(\sigma\\)

$$
\epsilon_\Re \sim \mathcal{N}(0, \sigma)
$$

$$
\epsilon_\Im \sim \mathcal{N}(0, \sigma)
$$ 

and

$$
\epsilon = \epsilon_\Re + i \epsilon_\Im
$$

Radio interferometers will commonly represent the uncertainty on each visibility measurement by a "weight" \\(w_i\\), where

$$
w_i = \frac{1}{\sigma_i^2}
$$

A full interferometric dataset is a collection of visibility measurements, which we represent by

$$
\boldsymbol{V} = \\{V_1, V_2, \ldots V_N\\}_{i=1}^N
$$

each one having a corresponding \\(u_i, v_i\\) coordinate. For reference, a typical ALMA dataset might contain a half-million individual visibility samples, acquired over a range of spatial frequencies.

Because images of the sky are real, therefore the real part of the visibility function must always be even and the imaginary part odd. The visibility function is Hermitian. This means that 

$$
\mathcal{V}(u, v) = \mathcal{V}^{\*}(-u, -v).
$$


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

### Point source sensitivity

* derive same result as point source sensitivity under natural weighting
See Casusus appendix.

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




