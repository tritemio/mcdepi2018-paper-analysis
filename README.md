# MC-DEPI Analysis Notebooks

This repository contains analysis notebooks for the paper:

> *Monte-Carlo Diffusion-Enhanced Photon Inference:*
> *Distance Distributions and Conformational Dynamics in single-molecule FRET*
>
> Antonino Ingargiola, Shimon Weiss, Eitan Lerner (2018) https://doi.org/10.1101/385252

These notebooks also serve as an example on using the [depi](https://github.com/opensmfs/depi) package.

# Workflow Overview

## Preprocessing: burst search and grouping
> *Notebooks:*
> 1. [PIE Analysis and save bursts.ipynb](https://nbviewer.jupyter.org/github/tritemio/mcdepi2018-paper-analysis/blob/master/PIE%20Analysis%20and%20save%20bursts.ipynb)
> 2. [Batch run notebook.ipynb
](https://nbviewer.jupyter.org/github/tritemio/mcdepi2018-paper-analysis/blob/master/Batch%20run%20notebook.ipynb)

Each sample has several smFRET-PIE measurements. The same notebook[1] for burst-search and population selection (D-only, FRET)
is executed in batch on all the data files for a given sample. Bursts for each (sample, population) pair are grouped in a
single data file for further analysis[2].

## Experimental analysis

We perform the experimental data analysis for each sample using bursts aggregated from from multiple measurements in the previous step.
In this step we compute the experimental histograms and plots (ALEX, fluorescence decays, FCS, BVA) to be compared with simulation 
in the following step.

## Single MC-DEPI simulation


# Dependencies

- python >= 3.6
- [depi](https://github.com/opensmfs/depi) >= 0.1+14.g413c350
- [scikit-optimze](https://scikit-optimize.github.io/) >= 0.5.2+39.g000b9d8
