---
layout: mathjax
title: Homework 6
---

## Homework 6 (100 pts)

_This problem set is due 11/29/2018. Please turn in your
work by uploading to Canvas. If you have questions, please post them on the
course forum, rather than emailing the course staff. This will allow other
students with the same question to see the response and any ensuing discussion.
The goal of this problem set is to review Kalman filtering (continuous time latent
variables). You should submit both commented code and a write-up for this
homework assignment._


The goal of this assignment is to carry out continuous neural decoding, first
just using Bayes rule and then by using a _particle filter_ to account for a
prior model of animal movement.

1. _Review of generative models (30 pts)_
  Write down simple pseudocode for the generation of simulated data for the
  following problems.

    **Ex.** Classification of data into $$K$$ classes using labeled training data
    where the data are conditionally Gaussian.

        1. Generate a random number, k from 1, ..., K as the class
        identity. (Use the class prior probabilities to weight the choices.)

        2. Sample a random Gaussian from a Gaussian distribution with mean
        mu_k and covariance Sigma_k.

    **a.** Clustering data into $$K$$ clusters using a mixture of Gaussians model.
    (_Hint: This might not be different from the example!_)

    **b.** Dimensionality reduction from $$D$$-dimensional data to $$M$$-dimensional
    data, using probabilistic PCA.

    **c.** A linear dynamical system with Gaussian initial state, innovations,
    and observation noise (i.e., the generative model for a Kalman filter).


2. _Convexity conditions for Poisson firing rate functions (30 pts)_
  Consider a set of $$k = 1\ldots N$$ rat hippocampal neurons in which the number
  of spikes observed  follows a Poisson distribution, $$n_k \sim \text{Poisson}\!(e^{-g(x)})$$,
  where $$g(x)$$ is some function of the rat's position $$x$$. Given an observation
  of spike counts,  $$\mathbf{n} = [n_1, n_2, \ldots]^\text{T}$$, what is the log
  likelihood function of the rats  position $$x$$? What are the first and second
  derivatives of the log likelihood with respect to $$x$$? 

  Now, consider two situations. In the first situation, let's assume that $$g(x)$$ is
  the same for each neuron, and that $$N$$ (the number of neurons) is large. What 
  conditions on $$g(x)$$ would imply that the log likelihood was concave (i.e.,
  the second derivative was negative everywhere)? Hint: make use of the weak law
  of large numbers, for independent identically distributed $$x_n$$, 
  $$\frac{\sum_{n=1}^N x_n}{N} \approx \mu$$, where $$\mu$$ is the mean of the
  $$x_n$$ (and remember what the mean of a Poisson process is!).

  In the second situation, answer this question: what conditions on g(x) would
  ensure that $$e^{-g(x)}$$ is both convex and log-concave (i.e., $$log(e^{-g(x)})$$
  is concave)? These constraints come from 
  ["Maximum likelihood estimation of cascade point-process neural encoding models"](https://www.tandfonline.com/doi/abs/10.1088/0954-898X_15_4_002).

3. _Single step of dynamical system estimation (40 pts)_
  Consider a set of 10 neurons whose firing can be described as function of the form
  $$
  \begin{equation*}
  n_k \sim \text{Poisson}\!\left[r_{max,k}  \exp\left(\frac{-(x - \mu_k)^2}{2\sigma_k^2}\right)\Delta_t\right].
  \end{equation*}
  $$
  You will find the parameters for these neurons in the file
  [hw6problem3.npz](hw6problem3.npz) ([hw6problem3.mat](hw6problem3.mat)).
  The vectors `MaxRates` (in spikes per second), `FieldCenters`, and `FieldWidths`
  correspond to the values of $$r_{max}$$, $$\mu$$, and $$\sigma$$ for the
  neurons, respectively. Spike counts for 250 milleseconds of neural activity is
  found in the vector `NeuralObservation`.

    **a.** Plot the log likelihood of the rat's position, $$x$$, over the range $$[0,
    100]$$ (ignore the term(s) in the likelihood function that is common to all
    positions). What is the maximum likelihood position of the rat?

    **b.** Now assume that you have prior information about the rat's position
    characterized by a normal distribution, with mean 30 and standard deviation 5.
    On the same graph as for (a), plot the log likelihood of the rat's position
    using just your prior distribution and using both the prior distribution and
    the observations of neural activity. What is the maximum _a posteriori_
    position of the rat?
