---
layout: mathjax
title: Homework 3
---

## Homework 5 (100 pts)

_This problem set is due Monday (10/5/2020) at 11:59pm. Please turn in your
work by uploading to Canvas. If you have questions, please post them on the
course forum, rather than emailing the course staff. This will allow other
students with the same question to see the response and any ensuing discussion.
The goal of this problem set is to review classification by performing
“discrete decoding” of some neural data. You should submit both commented code
and a write-up for this homework assignment._


### Description of data set

The data (in file `ReachData.npz` on Canvas) is in a struct array “r” of size 1127.
You can see the variables in a `.npz` file by querying the `.files` class element.

```
import numpy as np
ReachData = np.load('ReachData.npz')

print(ReachData.files)

### to find out the fields of a Python structure/dictionary, the command "dir" is useful
print(dir(ReachData))
```

Each element in the array contains data on one trial. Time stamps for
each spike in trial i and neuron j are given in r(i).unit(j).spikeTimes (in ms
relative to trial start). Other relevant members of the structure for this
homework assignment are: r(i).timeTouchHeld (the time the reach target appears
and movement planning nominally begins), r(i).timeGoCue (the time the animal is
cued to move and planning nominally ends), r(i).timeTargetAcquire (the time
movement ends). 

Here's some code to convert the "r" structure to more normal Python/numpy format:
```
r = ReachData['r']

spikeTimes = [] # This will be a list across trials of a list across units of np arrays of spike times 
timeTouchHeld = []
timeGoCue = []
timeTargetAcquire = []
for trial in r:
    spikeTimes.append([u.spikeTimes if type(u.spikeTimes) is np.ndarray \
                           else np.array([u.spikeTimes]) for u in trial.unit])
    timeTouchHeld.append(trial.timeTouchHeld)
    timeGoCue.append(trial.timeGoCue)
    TimetimeTargetAcquireMovementEnds.append(trial.timeTargetAcquire)
```

A big part of all three parts of this assignment will be
writing code that creates _spike count vectors_. These will be vectors of size
$$Nx1$$, where $$N$$ is the total number of neurons (i.e., 190). All of the
classification will be on these vectors, and you should combine across neurons
_by taking the product of probabilities_ (not by doing anything that would
collapse across neurons or mix their information). There are 8 different reach
targets (locations in the vector targets); the target index for each trial is
in the vector `cfr`.

### Questions

1. _Plan data only vs movement data (30 pts)_

   Define two windows of spikes, the “plan window” encompassing spikes which were emitted between
   target onset and the go cue, and the “movement window” encompassing spikes which were emitted
   between the go cue and the time movement ends (note that both types of window are of different
   length for different trials). Here is example code to find the array of spikes in the plan window:
   ```
planSpikes = []
for trialIdx, trialSpikes in enumerate(spikeTimes):
    planSpikes.append([np.sum((st > timeTouchHeld[trialIdx]) & (st < timeGoCue[trialIdx])) for st in trialSpikes])
planSpikes = np.array(planSpikes)
   ```
   Randomly choose 50 trials per target to be a set of training data
   (400 trials total). The remaining trials will be used for testing (“test data”). We can
   describe the number of spikes that a neuron produces in each window (vector of neural counts as
   $$\mathbf{n}_{plan}$$ and $$\mathbf{n}_{move}$$, where $$\mathbf{n} \in \mathbb{R}^N$$,
   where $$N$$ is the number of neurons) using a wide variety of random process
   models. Use the training data to estimate the parameters of a target- direction-dependent
   vector Poisson process, i.e., $$\Pr(\mathbf{n} \mid \mathrm{target}_k) \sim
   \mathrm{Poisson}(\boldsymbol{\lambda}_k) = \prod_{n=1}^N \mathrm{Poisson}(\lambda_{k,n})$$.
   In other words, calculate a Poisson model for _each neuron_ and class - the rate for a class
   will be a vector the size of the number of neurons.  For each of remaining test trials
   estimate the maximum likelihood reach target (do it in log space!). What is the overall
   decoding accuracy (i.e., what fraction of trials are decoded correctly) using only the data
   in the plan window? Using only data in the movement window?  Combining plan and movement
   data?



2. _Amount of plan data (40 pts)_

     **a.** The plan periods in the data set are either 755 or 1005 ms. Consider decoding using less
     than the full plan period. Generate new models for plan periods of increasing size (50 ms to
     750 ms in 50 ms increments, where all plan windows begin at the target onset time). Using
     maximum likelihood estimation, decode the reach target for the test data using these different
     sized windows of neural data. (Both train and test with these window sizes!) Plot the
     decoding accuracy as a function of the size of the decoding window. Briefly describe what
     you see. (20 pts)

     **b.** Now, instead of using an increasing window size, use a constant 200 ms window, but slide
     the window start time from target onset (“0”) to 550 ms after target onset (use 50 ms
     steps). Generate new models for each window location and decode the reach target for the
     test data. Plot the decoding accuracy as a function of the temporal location of the
     decoding window. Briefly describe what you see. (20 pts)

3. _Number of neurons (30 pts)_

     Using your model from (1), you decoded maximum likelihood targets using all 190 neurons. Now,
     perform a “neuron dropping analysis”. Starting with your full model, randomly eliminate neurons
     and evaluate decoding accuracy in the reduced data set. Eliminate between 20 and 180 neurons
     (by 20s – so decode using between 10 and 190 neurons in steps of 20) – average each point
     (number of neurons) by randomly choosing neurons to be dropped 20 times. Plot decoding
     accuracy as a function of the number of neurons available to the decoder. Briefly describe
     what you see?
