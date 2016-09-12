---
layout: default
title: Homework 2
---

## Homework 2 (120 pts)

_This problem set is due Tuesday (9/20/2016) at 5pm. Please turn in your work by uploading to
Canvas. For extra credit, create a Jupyter Notebook containing your answers and your python
code for question 4, and push this to a github repository (the commit time will be taken as the
submission time). If you have questions, please post them on the course forum, rather than
emailing the course staff. This will allow other students with the same question to see the
response and any ensuing discussion. The goal of this problem set is to review the fundamentals
of electrophysiology as well as providing some “warmup” Python work._


1. _Membrane Potentials and the Nernst Equation (20 pts)_

   As discussed in class, a neuron’s resting potential is determined by the relative intra- and
   extra-cellular ion concentrations and the corresponding conductivities of the respective ion
   channels. 

     **a.** Use the Nernst equation to fill in the equilibrium potentials for potassium, sodium,
     and chloride in the table below. (3 pts) 

     Ion | Extracellular Concentration | Intracellular Concentration | Permeability | Equilibrium Potential
     --- | --- | --- | --- | ---
     K+ |   20 mM | 400 mM | 1 | 
     Na+ | 440 mM | 50 mM | 0.04 | 
     Cl- | 560 mM | 52 mM | 0.45 | 
     Mn++ |    2 mM | 0.01 mM | 0.01 |  | 

     **b.** Taking into account the potassium, sodium, and chloride concentrations (ignoring
     manganese), calculate the resting potential of a neuron with the characteristics in the
     table above. (2 pts)

     **c.** How could you adjust the potassium level to raise the resting potential? Why does this
     work? (3 pts)

     **d.** Calculate the new extracellular potassium concentration that would be needed to bring
     the resting membrane potential to -65mV. To counteract the decrease in potassium
     concentration, the cell could insert or remove ion channels into the membrane thus
     changing the permeability ratio. How might the cell recover the original resting membrane
     potential and calculate the new permeability ratio?  (6 pts)

     **e.** Cells have mechanisms for active transport of sodium, potassium and chloride ions. What
     would happen to the information in the table above (ignoring Mn++) if the chloride
     transporter protein was turned off and only passive forces affected chloride ions? What
     about the resting membrane potential? (6 pts) 


2. _GHK equation with divalent ions (35 pts)._

   The ion channels which are active at the cell’s resting potential (“resting channels”) are in
   real-life fairly selective and not particularly permeable to ions other than sodium and
   potassium, and in particular not to divalent ions. Let’s imagine that that is not the case, and
   that the resting membrane is also permeable to manganese. Further, let’s assume intra- and
   extracellular concentrations as in the table above.

   **a.** Use the Nernst equation to specify the equilibrium potential for Mn++. (2 pts)
 
   **b.** Derive the GHK equation for 3 monovalent ions and a divalent ion. (20 pts)
 
   **c.** Consider an unusual cell with the membrane permeabilities and internal and external
   ionic concentrations in the table. What is the resting membrane potential of this cell? (13
   pts)


3. _Simulating a neuron (65 pts)_ 

   For the following problems, you will make use of Python code that simulates a neuron. You
   can find the hh() function in a [Jupyter
   notebook](https://github.com/elec548/JupyterNotebooks/blob/master/HodgkinHuxleyNotebook.ipynb)
   in the class github repository. You should use a time step, Δt  = 0.01 ms (except for **3b**).

   **a.** Consider 1 s of responses of the HH neuron. Plot the responses to a 0.2 uA/mm2
   current step at time 0.2 s that lasts for 0.2 s. How many spikes are generated by this
   input? (4 pts)

   **b.** Consider the input in **4a** with Δt  = 0. 1 ms, Δt  = 0.05 ms, and Δt  = 0.001 ms. How
   are the results similar/different? (4 pts)

   **c.** Evaluate 0.2 s current injections at time 0.2 s. What is the “threshold voltage” for
   this neuron? (You can either look for a “knee” in the voltage curve or use test currents of
   increasing size to probe when an AP is triggered.) Tweak the parameters to decrease and
   increase the threshold voltage by ~5 mV. What did you change, and by how much? (8 pts)

   **d.** Using the same time/length of current injection (0.2 s at 0.2 s), consider
   depolarizations of 0 to 0.50 uA/mm2 (in 0.025 uA/mm2 steps). Count and plot the number of
   spikes generated for each level. This is an “input-output” curve. How could you change the
   slope of this curve? (8 pts)

   **e.** Now modify the HH neuron to become a CST neuron (TCN Ch. 6 and/or included paper for
   parameters). Plot the input-output curve of the CST neuron as in **4d**. How is it different?
   (10 pts)

   **f.** Plot and compare the response of an HH neuron and a CST neuron to an injected current
   profile of 0 for 0.2 s, −0.050 uA/mm2 for 0.2 s, and then 0.15 uA/mm2 for 0.1 s (i.e., an
   input current that goes low then high). How are they similar? How are they different? (6
   pts)

   **g.** (Random input.) Now you will consider input current profiles for the CST neuron (your
   choice!) that are random. Construct an input current profile that is the sum of a 0.4 s step
   depolarization (starting at 0.1 s) and a random input vector. For the random input vector,
   smooth the MATLAB Gaussian random number generator (randn) with a 100 point moving average
   filter. What are the expected value and variance of the numbers generated by randn? What is
   the expected value of one point of your smoothed random number sequence? What is the
   expected variance? Construct 25 repeats for each combination of the following parameters:
   depolarization steps – {0 uA/mm2, 0.1 uA/mm2, 0.2 uA/mm2}, smoothed standard deviation
   (achieved by scaling randn) – {0 uA/mm2, 0.1 uA/mm2, 0.2 uA/mm2}. Plot the mean number of
   generated spikes as a function of step depolarization and noise standard deviation (i.e., 2
   plots)? How does the variance in the number of spikes per trial change with these different
   parameters? (25 pts)
