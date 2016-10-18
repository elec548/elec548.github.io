---
layout: mathjax
title: Homework 4
---

## Homework 4 (120 pts)

_This problem set is due Friday (10/21/2016) at 5pm. Please turn in your work by uploading to
Canvas. f you have questions, please post them on the course forum, rather than
emailing the course staff. This will allow other students with the same question to see the
response and any ensuing discussion. The goal of this problem set is to review the basics of
point processes in general and the Poisson process in particular._


1. _Point Process Model Validation (30 pts)_

   We can describe the number of spikes that a neuron produces in a one second
   window using a wide variety of random process models. Let’s refer to the
   parameters of a given model as $$\theta$$, and a set of neural counts as
   $$\mathbf{n}$$. We can evaluate the quality of the model by asking for which
   model/parameters the likelihood of the data, $$P(\theta \mid \mathbf{n})$$,
   is maximized. In this problem you will try to determine the right model for
   the spikes from a synthetic neuron.


     **a.** _Model Comparison (10 pts)_ Look at the data in the numpy data file
     [hw4problem1A.npy](hw4problem1A.npy). This file contains two variables, `SpikeTimes` and `SpikeCounts`.
     `SpikeTimes` is a list of spike times recorded in one experimental trial (each trial is 1
     s for simplicity) from a neuron.  For each trial, for convenience, the number of spikes
     is given in the corresponding element of `SpikeCount`. You can load the data into python
     using `numpy.load()`:

   ```python
   import numpy as np
   [SpikeTimes, SpikeCount] = np.load('hw4problem1A.npy')
   ```
     
     Consider two possible models for the number of spikes per trial: 
   
   | Model | Parameters |
   |:-------|:------|
   | Gaussian | Mean = 10.2, Variance = 9.5  |
   | Poisson  | Rate = 9.8  |
   |--------------------|
     
     Using the data provided evaluate which set of parameters best describes the data. (_Hint:
     You should calculate the likelihood of each model. In other words, calculate the
     probability of, e.g., data from 1000 trials of the simulated data given one model or
     the other?_) 
     
     **b.** Now split the data into training and validation sets, use the training sets
     to train a Gaussian model and a Poisson model, and use the validation set to assess which
     model fitst the data best. (_Hint: You should use the maximum likelihood estimate for the
     parameters of the models, which will be the sample mean and variance of the training data
     for the Gaussian and the sample mean for the Poisson._)

     **c.** _Point Processes (10 pts)_ Consider the actual spike times from **(a)**
     (given in the SpikeTimes list). If you were to model this neuron with a Poisson process,
     would you use a constant or a time-varying rate over the 1 second window?
     (Justify your answer with one or more plots.)

     **d.** _Fano Factor / ISI Distribution (10 pts)_ Consider the spike times returned by a
     different neuron whos data are given in [hw4problem1C.npy](hw4problem1C.npy)
     If you were to model this neuron with a Poisson process, would you use a constant or a
     time- varying rate? Do the variance and mean of the count distribution have the
     relationship you would expect for a Poisson process? What physiological process might you
     observe in the spike times that would explain this? (_Hint: You’ll want to actually look
     at the spike times. Generating a peri-stimulus time histogram (PSTH) showing the average
     number of spikes in small bins of time will help explain your observations._)


2. _Describing Real Data (60 pts)_

   In this problem you are going to characterize the [neural activity recorded
   on a multielectrode array in visual cortex](#datasource), found in
   [hw4problem2.npy](hw4problem2.npy).  Spike times (in microseconds) for 10 neurons are given
   in `spiketimes`, a 10 element numpy array, where each element is a numpy vector of
   spiketimes.  The time-varying stimulus is described in the `stimulus` numpy array, where the
   first column is timestamps (which follow regular 5 ms steps) and the second column is the
   direction of motion (in degrees) of a moving bar. Stimulus directions are randomized, each
   direction is maintained for 4 s, and directions are repeated 8 times. You can load the data
   into python using `numpy.load()`:

   ```python
   import numpy as np
   [stimulus, spiketimes] = np.load('hw4problem2.npy')
   ```

     **a.** (10 pts) Neural activity in visual cortex was recorded while the cat
     was viewing drifting bars (like in the Hubel and Weisel video) that
     cycled through different angles. The visual stimulus was constant for
     4s, and then changed (the angle of the stimulus in 5 ms bins is given
     in the stimulus vector). Plot the “tuning curve” for the activity from the 10th
     neuron, show how the mean number of spikes varies as a function of the
     direction of the moving bar. (The x-axis should be stimulus direction and the
     y-axis should be “average spikes per second”.) Does this neuron have one
     “preferred direction”?  Why might the directional tuning be shaped the way it
     is?  

     **b.** (10 pts) Calculate the tuning curves for all the neurons. Plot a histogram
     of the “preferred direction” (i.e., direction of stimulus which produces
     maximum average spiking) of the neurons. Plot a histogram of the “maximum
     firing rate” (i.e., the average firing rate for the preferred direction).

     **c.** (10 pts) Select the neuron with the largest maximum firing rate.
     For each stimulus direction, calculate the mean spiking rate and the variance
     from this mean in the number of spikes observed in one stimulus presentation.
     Calculate the Fano factor for each stimulus direction and compare to what is
     expected for a Poisson process. Repeat for the neuron with the largest average
     (i.e., across all directions) firing rate.  

     **d.** (10 pts) For the neurons in part (c), create ISI distributions for
     spikes during stimuli in the preferred direction. Compare this to the ISI
     distribution expected under a Poisson approximation.

     **e.** (20 pts) For the neurons in part (c), use the time-rescaling
     theorem to renormalize the ISIs for all stimulus directions (i.e., assuming a
     piece-wise constant inhomogeneous Poisson process). Plot the resulting ISI
     distribution. Conduct a goodness-of-fit test on the spikes. Is the piece-wise
     Poisson model a good one?
 

### <a name="datasource"></a> Data Source Note:
This data is courtesy of Tim Blanche, UC Berkeley. He captured single and multiunit recordings
in the primary visual cortex of cats during viewing of simple and complex stimuli.  It has been
shared for public use on the CRCNS.org website as data set “pvc3”, and we are using the data
from the “drifting bar” section.