---
title: Regularized Maximum Likelihood (RML) II
date: 2022-10-26
publishdate: 2022-10-21
---

* [Whiteboard recording](https://psu.mediaspace.kaltura.com/media/Astro+589A+Lecture+10+RML+Part+II+Part+I/1_syjygobk)
* [Zoom Slides](https://psu.mediaspace.kaltura.com/media/Astro+589A+Lecture+10+RML+Part+II+Part+II/1_b483lcx9)

## References

* [MPoL introduction](https://mpol-dev.github.io/MPoL/rml_intro.html)
* [Machine Learning: A Probabilistic Perspective](https://catalog.libraries.psu.edu/catalog/19499523) by Murphy, Chapter 10
* [Pattern Recognition and Machine Learning](https://catalog.libraries.psu.edu/catalog/3405468) by Bishop, Chapter 8
* The fourth paper in the 2019 [Event Horizon Telescope Collaboration series](https://ui.adsabs.harvard.edu/abs/2019ApJ...875L...4E/abstract) describing the imaging principles
* [Maximum entropy image restoration in astronomy](https://ui.adsabs.harvard.edu/abs/1986ARA%26A..24..127N/abstract) AR&A by Narayan and Nityananda 1986
* [Multi-GPU maximum entropy image synthesis for radio astronomy](https://ui.adsabs.harvard.edu/abs/2018A%26C....22...16C/abstract) by Cárcamo et al. 2018
* [Regularized Maximum Likelihood Techniques for ALMA Observations](https://ui.adsabs.harvard.edu/abs/2022arXiv220911813Z/abstract) by Zawadzki, Czekala, et al.
* [Fitting Very Flexible Models: Linear Regression With Large Numbers of Parameters](https://ui.adsabs.harvard.edu/abs/2021PASP..133i3001H/abstract) by Hogg and Villar

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


