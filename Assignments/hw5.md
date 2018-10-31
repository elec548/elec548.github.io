---
layout: mathjax
title: Homework 5
---

## Homework 5 (100 pts)

_This problem set is due Tuesday (11/13/2018) at 11:59pm. Please turn in your
work by uploading to Canvas. f you have questions, please post them on the
course forum, rather than emailing the course staff. This will allow other
students with the same question to see the response and any ensuing discussion.
The goal of this problem set is to review clustering through the process of
spike sorting action potentials data. You should submit both commented code and
a write-up for this homework assignment._


### Description of data sets
For Problem 1 of this assignment, you will again use the reach data from HW 3
(in file `ReachData.npz` on Canvas).  For Problem 2, you will use the datasets
from HW4, [SpikeData05.npz](SpikeData05.npz) and
[SpikeData12.npz](SpikeData12.npz), which contain numpy arrays `SpikeWaveforms`
and ~~`RawData`~~. (The raw data was too large to upload. It is available on
request.) The  corresponding Matlab files are
[SpikeData05.mat](SpikeData05.mat) and [SpikeData12.mat](SpikeData12.mat).

1. _Visualization of high-dimensional neural activity using PCA (30 pts)_
  For this problem, you will again use the reach data from HW 5 (in file
  `ReachData.npz` on Canvas). Begin by extracting the "plan" data for all trials
  and all targets. Use a constant length window 200 ms long starting 50 ms after
  the timeTouchHeld. You should end up with a matrix of $$N \times K$$, where
  $$N$$ is the number of neurons and $$K$$ is the total number of trials.

    **a.** Calculate the principal components of the neural activity (there is a
    PCA decomposition within sklearn, but you can also just use the
    `numpy.linalg.eig` function). Plot the square-rooted eigenvalue spectrum.
    If you had to identify an elbow in the eigenvalue spectrum, how many
    dominant eigenvalues would there be? What percentage of the overall
    variance is captured by the top 3 principal components?

    **b.** For the purposes of visualization, we’ll consider the PC space defined by
    the top M = 3 eigenvectors. Project the data into the three-dimensional PC
    space. Plot the projected points in a 3D plot
    ([example here](http://matplotlib.org/mpl_toolkits/mplot3d/tutorial.html)) and
    color each dot appropriately according to reaching target. Rotate the axes
    ([example here](http://matplotlib.org/examples/mplot3d/rotate_axes3d_demo.html))
    to show a view in which the clusters are well-separated.

    **c.** Define a matrix $$U_M \in \mathbb{R}^{D \times M}$$ containing the
    top three eigenvectors (i.e., PC directions), where $$U_M (d, m)$$ indicates
    the contribution of the $$d$$th neuron to the $$m$$th principal component.
    Show the values in $$U_M$$, e.g., by calling `imshow(UM )’. (Note: Also
    call `colorbar’ to show the scale.) Are there are any obvious groupings
    among the neurons in each column of $$U_M$$?

2. _Reducing the dimensionality of spike waveforms (40 pts)_
  For this problem, you will again use the spiking data from HW 4.

    **a.** Begin with the data from the numpy file
    [SpikeData05.npz](SpikeData05.npz). Use the first 5000 samples. Calculate
    the principal components of the spike waveforms on the first tetrode channel.
    Plot the square-rooted eigenvalue spectrum. If you had to identify an elbow
    in the eigenvalue spectrum, how many dominant eigenvalues would there be?
    What percentage of the overall variance is captured by the top 3 principal
    components?

    **b.** Plot the mean and the first 2 principal components. What do they
    look like?

    **c.** Now repeat the process for the first 5000 samples in
    [SpikeData12.npz](SpikeData12.npz), again just using the first tetrode
    channel. Are there any major differences?

    **d.** Now, consider all the channels in SpikeData12 as a single, 160x1
    vector. Calculate the principal components of this larger vector. Plot
    the square-rooted eigenvalues - how many dominant eigenvalues do you
    think there are?

    **e.** Plot the mean and the first 2 principal components of the tetrode
    principal components. How do the values for the first channel differ
    from the values in the single channel analysis from (c)? What is the
    difference in the fraction of the variance in the first tetrode channel
    captured by the 4 channel PCA vs. the single channel PCA? What about
    the first three principal components? _Hint: Only consider
    the first tetrode channel. Calculate the total variance in the samples,
    then project into the space of the first two principal components (either
    single channel or full data). Back-project these into waveform space
    by multiplying by the principal component vectors and adding the results.
    Add the mean to get back to the original. Now calculate the variance (only
    for the first tetrode channel!) of these effectively lower dimensional
    waveforms, and take the ratio to the baseline.)_

3. _Comparing PCA, P-PCA, and FA (30 pts)_
    To answer these questions, please begin by downloading and running the
    Jupyter notebook for the problem
    [Homework5Problem3.ipynb](https://elec548.github.io/Assignments/Homework5Problem3.ipynb).
    For PCA and P-PCA, the notebook makes a plot showing:
      - Each data point $$x_n$$ as a black dot in its native 2-D space
      - The mean, $$\mu$$  as a green point
      - The PC space found by either PCA or P-PCA (which is one dimensional)
      plotted as a line segment
      - The projection of each data point into the PC space as red points
      - A red line connecting the original and lower dimensional data

    **a.** The lower dimensional space found by PCA and P-PCA are the same.
    So why are the lines in P-PCA not orthogonal as they are in the PCA
    case?

    **b.** Using the code for P-PCA as a guide, implement the EM algorithm
    for Factor Analysis and make a plot showing the same information. Note that
    the lower dimensional space found by FA will pass through the mean but
    will not be the same as in PCA/P-PCA. Why is it different?
