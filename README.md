# MC-DEPI Analysis Notebooks

This repository contains analysis notebooks for the paper:

> *Monte-Carlo Diffusion-Enhanced Photon Inference:*
> *Distance Distributions and Conformational Dynamics in single-molecule FRET*
>
> Antonino Ingargiola, Shimon Weiss, Eitan Lerner (2018) https://doi.org/10.1101/385252

These notebooks also serve as an example on using the [depi](https://github.com/opensmfs/depi) package.

To run these notebooks online click here [![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/tritemio/mcdepi2018-paper-analysis/master)

# Data
Raw data files for the 7d and 17 dsDNA samples are available on Figshare:

- [Ingargiola, A; Weiss, S; Lerner, E (2018): Photon-HDF5 files. figshare. Fileset.](https://doi.org/10.6084/m9.figshare.6931271)

# Workflow Overview

## Preprocessing: burst search and grouping
> *Notebooks:*
> 1. [PIE Analysis and save bursts.ipynb](https://nbviewer.jupyter.org/github/tritemio/mcdepi2018-paper-analysis/blob/master/PIE%20Analysis%20and%20save%20bursts.ipynb)
> 2. [Batch run notebook.ipynb
](https://nbviewer.jupyter.org/github/tritemio/mcdepi2018-paper-analysis/blob/master/Batch%20run%20notebook.ipynb)
> 3. [Group Results by Name.ipynb ](https://nbviewer.jupyter.org/github/tritemio/mcdepi2018-paper-analysis/blob/master/Group%20Results%20by%20Name.ipynb)
> 4. [Export TCSPC decays from ns-ALEX Photon-HDF5.ipynb](https://nbviewer.jupyter.org/github/tritemio/mcdepi2018-paper-analysis/blob/master/Export%20TCSPC%20decays%20from%20ns-ALEX%20Photon-HDF5.ipynb)

A notebook[1] for burst-search and population selection (D-only, FRET)
is executed in batch (using [2]) on all the Photon-HDF5 data files for a given sample.
Next, burst photon data (timestamps, D/A labels, TCSPC nanotimes) for each (sample, population) pair
are grouped in a single data file[4] for further analysis.

Notebook 4 exports fluorescence decays histograms from a smFRET-PIE data file in Photon-HDF5 format.
This notebook is used to export IRF decays used to simulate realistic TCSPC nanotimes in the following steps.

## Experimental data analysis and simulation fitting
> *Notebooks:*
>
> 5. [Burst Analysis-DEPI-exp.ipynb](https://nbviewer.jupyter.org/github/tritemio/mcdepi2018-paper-analysis/blob/master/Burst%20Analysis-DEPI-exp.ipynb)
> 6. [Burst Analysis-DEPI-sim2-E_nanotime.ipynb](https://nbviewer.jupyter.org/github/tritemio/mcdepi2018-paper-analysis/blob/master/Burst%20Analysis-DEPI-sim2-E_nanotime.ipynb) (7d sample)
> 7. [Burst Analysis-DEPI-sim2-E_nanotime-17d-bg-irf.ipynb](https://nbviewer.jupyter.org/github/tritemio/mcdepi2018-paper-analysis/blob/master/Burst%20Analysis-DEPI-sim2-E_nanotime-17d-bg-irf.ipynb) (17d sample)

We perform the experimental and simulated data analysis for each sample in similar fashion.
For the experimental analysis[5] we use bursts aggregated from from multiple measurements in the previous step.
The simulated data is generated by an MC-DEPI simulation with user-defined distance distribution and self-diffusion
parameters[6, 7]. Simulations have the same number of photons as the experimental burst photon-data, with "recoloring"
(i.e. reassigned D and A labels) and with with simulated TCSPC nanotimes. The simulation takes into
account multiple conformational states with arbitrary transition matrix, a distance distribution model for each state,
a D-A diffusion relaxation time for each state, acceptor photo-blinking, correction factors (gamma, donor leakage,
acceptor direct excitation from donor laser), background counts.
In the paper we compare FRET histograms and D and A fluorescence decays between experiments and simulations.
Other analysis carried out in the notebooks are FCS, BVA.

The notebooks [8] and [9] repeat the simulations 100 times using the same input parameters, but different seeds
for the random number generation. In this way, we assess the dispersion of the simulation due to the sole
Monte Carlo noise. The dispersion is assessed both graphically and computing the standard deviation of the
loss function.

The notebooks, [6] and [7], first perform a single user-define simulation, then they compute the loss function
and finally the run an optimization procedure to find the best parameters fitting the experimental data.
The standard deviation of the loss function is important for the convergence of the optimization algorithm.

# Additional Notebooks
> - [Continous-Time Markov Chain.ipynb](https://nbviewer.jupyter.org/github/tritemio/mcdepi2018-paper-analysis/blob/master/Continous-Time%20Markov%20Chain.ipynb)

This notebooks demonstrates an implementation of Continuos-Time Markov Chain matrix formalism using numpy.

# Dependencies

- Python >= 3.6
- [FRETBursts](https://fretbursts.readthedocs.io) >= 0.7
- [depi](https://github.com/opensmfs/depi) >= 0.1+14.g413c350
- [scikit-optimze](https://scikit-optimize.github.io/) >= 0.5.2+39.g000b9d8
- [randomgen](https://bashtage.github.io/randomgen/) ==1.14.4 (next-generation RNG, soon to be included in numpy)

# Installation

Follow the instructions below to create a reproducible conda environment to run the notebooks in this repository
on a local computer (Windows, macOS or Linux).

> **Note:** you can also run the notebooks on the cloud (no installation) by clicking here [![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/tritemio/mcdepi2018-paper-analysis/master)

Install the free [Anaconda 3](https://www.anaconda.com/) python distribution and follow these steps.

1) In a terminal, type each single line (assuring that there is not error after each line):

```
conda activate depi_env
conda install fretbursts ipython
pip install randomgen
pip install git+https://github.com/scikit-optimize/scikit-optimize/ --upgrade
pip install pycorrelate
conda install depi
```

The previous command installs the `depi` python package and all dependencies in an conda environment called `depi_env`.

Type `ipython` and try runnning:

```python
import depi
```

You should get no error. Type `quit` to exit ipython.

2) We need to create a "kernel" which allows using that environment from the notebook.
   In the same terminal as before (after exiting ipython) type:

```
python -m ipykernel install --name depi_env --display-name "MC-DEPI (Python 3.6)"
```

3) Download the notebooks used for the 2018 MC-DEPI paper from github:

https://github.com/tritemio/mcdepi2018-paper-analysis/archive/master.zip

In the folder where you put these notebooks, make a subfolder `data/results`
(one inside the other) and extract there the archive you download from:

https://ndownloader.figshare.com/files/12753497?private_link=4080f1df435c07e7bd21

(this is the burst data for the dsDNA smFRET-PIE measurements used in the paper).

4) Finally, in a new terminal, launch the notebook with `jupyter notebook`. Create a notebook. Make sure you choose the MC-DEPI kernel.

5) From the notebook tab, navigate to the depi notebooks and open:

     Burst Analysis-DEPI-sim2-E_nanotime.ipynb

Select the MC-DEPI kernel and run (at least) the first part until the single-condition DEPI simulation. No error should occur.
Running the full fit may require several hours of computations depending on initial conditions, model, number of iterations and
processing power.
