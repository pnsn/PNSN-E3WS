# PNSN-E3WS  
This repository is a fork of the E3WS repostory published by Pablo Lara (see below)  

The purpose of this repository is to test application of elements of E3WS on data from the Pacific Northwest Seismic Network (PNSN) with particular focus on charactrizing events in the US Pacific Northwest (PNW) and Cascadia Subduction Zone (CSZ) that bound the current magnitude alerting threshold for PNSN operations (M >= 2.95) and fall below the minimum magnitude used for the published version of E3WS.  

This initial fork is being used as a course project for ESS 469/569 at the University of Washington (Fall 2023) and if it shows promise may have continued development beyond the course.

:fork editor: Nathan T. Stevens  
:fork co-editors: Benz Poobua, Jake Ward  
:email: ntsteven (at) uw.edu  
:org: Pacific Northwest Seismic Network  

:license: This forked repository adopts the CC4.0-BY license.   
:attribution: For referencing the E3WS model architecture and code base, please reference Pablo Lara and co-authors as stated below (Lara et al., 2023). If using PNSN specific adaptations of the E3WS code-base in this repository, please cite Lara et al. (2023) and the URL for this repository.  

## Added Features  
### environments   

We provide a YAML version of the environment installation instructions provided by Pablo Lara in the main E3WS repository. Install the environment with:  
 `conda env create -f environments/environment_basic.yml`  

We also provide a version of the basic environment that adds the `apple` channel, which may be necessary for Apple Silicon compliant builds.  
 `conda env create -f environments/environment_apple.yml`  

Finally, we provide a YAML file that provides additional modules for development, which has all the necessary dependencies for interactive development/plotting:
 `conda env create -f environments/environment_def.yml`  

___________________________________________________________________________

## Initial README.md at time of fork-ing (17. Nov. 2023)

# E3WS: Earthquake Early Warning Starting From 3 s of Records on a Single Station With Machine Learning

[![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/pablolarasismo.svg?style=social&label=Follow%20%40pablolarasismo)](https://twitter.com/pablolarasismo)   
Email: pablo.elara@ieee.org

-----------------------------------------

Welcome to the E3WS. Do you have a station? Start monitoring earthquakes!

If you use this software, you should cite the code as follow:   

**E3WS, Pablo Lara et al. 2023, ** [![DOI](https://zenodo.org/badge/637827897.svg)](https://zenodo.org/badge/latestdoi/637827897)

And the paper:   

**Pablo Lara, Quentin Bletery, Jean-Paul Ampuero, Adolfo Inza, Hernando Tavera. Earthquake Early Warning Starting From 3 s of Records on a Single Station With Machine Learning. Journal of Geophysical Research: Solid Earth.**

**E3WS article: https://doi.org/10.1029/2023JB026575**

## Installation
i) git clone https://github.com/PabloELara/E3WS.git

ii) Create E3WS environment
- Install miniconda3 (https://docs.conda.io/en/latest/miniconda.html) or miniforge3 for Raspberry Pi 4 (https://github.com/conda-forge/miniforge).
- Create the environment: $conda create -n E3WS python=3.7
- Activate environment: $conda activate E3WS

iii) Install E3WS dependences ('pip' command is your friend):
- `python = 3.7 - 3.9`
- `xgboost = 1.6.1`
- `scipy = 1.8.1`
- `python-speech-features = 0.6`
- `scikit-learn = 1.1.1`
- `PyGeodesy = 23.3.23`
- `obspy = 1.3.0`


## E3WS models

E3WS consists of 3 stages: detection, P-phase picking and source characterization.

For P-phase picking and source characterization (magnitude and location) the models are already defined. You can find them in the models folder. For example MAG7tp3*.joblib, it means the magnitude model uses 7 seconds before P-phase and 3 seconds after.

For detection, you must create your own model with intrinsic noises of the station to be installed. Relax, it is not difficult.

Inside the 'DET/build_DET/' folder:
1. Have 10 days (or more) of continuous data to extract the noise (we must reach 900000 samples) and add it to the 'data/' folder.
2. Eliminate the seismic records in these 10 days. I made an automatic program to remove it ('gen_catag.py'). I strongly recommend removing earthquakes from the trace manually, following the format of the '.csv' of the 'picked/' folder.
3. Generate the feature vector with the program 'pb_FV_noise.py', it will create a csv file in the 'atr_noise/' folder.
4. Download the earthquake feature vector folder 'atr_eq/' at https://mega.nz/folder/E2UTXIQZ#rH_k9nNrUIgU3D04rzPzzQ
5. Finally, we have our noise ('atr_noise') and earthquake ('atr_eq') attributes. Now we have to train our detector model! Adapt the file pb_save_DET_model.py, and it will create our model in 'saved_models/'.

## Run E3WS
Time to monitor earthquakes.
An example of how E3WS works is in the 'real_time/' folder. E3WS detected the M5.6 earthquake of January 7, 2022 in Lima, Peru. The first E3WS estimate was M5.3 based on the first 3 seconds of P-wave. Continuous updates converged to M5.6 and are placed in the results/ folder.

-----------------------------------------

![map](real_time/E3WS_Lima.png)
