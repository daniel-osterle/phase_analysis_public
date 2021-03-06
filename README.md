# Confocal Microscopy Images Analysis

* [Project Description](#project-description)
* [Detailed Script Description](#detailed-script-description)
  * [Public Scripts Description](#public-scripts-description)
  * [Private Scripts Description](#private-scripts-description)

## Project Description

## Detailed Script Description


To run the analysis several scripts are required which are described in detail below. 
First, the scripts that are publicly available in this repository are described and then the private scripts. 
Please contact me at daniel.oesterle@outlook.com to request the private scripts.

## Public Scripts Description

### process_results.R
Execute this script from the command line with `./process_results.R` or run `./process_results.R --help` to see all available options that can be supplied from the command line. Without options the script will parse the directory from which this script is executed for a set of files that are required to postprocess the raw image files.
Most importantly, the script will look for the following files:
* A file of ".nd" format that contains basic information about the set-up of the (automated) microscopy screen
* A plate definition ("pdef") file of ".csv" format that contains information about the contents of each well on a 384-well plate
* Several files of ".txt" format that are the result of prior cell identification and segmentation and contain the raw imaging data, including cell size, position and fluorescence intensities for the measured wavelengths of a specific screen as described in [Heidenreich et al.,(2020)](https://rdcu.be/cE9xO) PMID: 32661377

Wile the files of ".nd" and ".txt" format result from the microscopy screen, the plate definition file has to be supplied by the user.

Further, this script then uses these input files and supplies them to the postprocess_result_txt() function to postprocess the raw data by mainly filtering unnecessary data with custom scripts from the [Emmanuel Levy Lab](https://www.weizmann.ac.il/CSB/ELevy/home) as described below and in this [preprint](https://doi.org/10.1101/260695).

Finally, the global environment is saved to "data_ready_to_analyze.RData". This file is sourced in "phase_analysis_main.R" for extracting the data of interest and plotting.


### phase_analysis_main.R

This script loads "phase_analysis_main.R" that contains all required functions and postprocessed data. Next, a new data frame is created by extracting columns of interest which include median measured fluorescence intensity, cellular dimensions, amount and dimensions of observed foci as well as identifying labels.
Finally, pdf files containing various plots are created by using functions from the "phase_plot_functions.R" module.

### phase_plot_functions.R

This module contains several functions to create plots with the ggplot2 R package. 

In the uploaded version of "phase_analysis_main.R" the function plot.overlap.gfp() is used which creates two x-y-scatterplots overlaid on top of each other. The bottom scatter plot uses all cells in which maximum GFP fluorescence intensity is below 100 AUF (arbitrary fluorescence units), whereas the top scatter plot uses cells where maximum GFP fluorescence intensity is above 1000 AUF. Biologically, this corresponds to low and high expression of a protein (here: a kinase) tagged with GFP, respectively. Both plots illustrate the median fluorescence intensity of protein scaffolds that are tagged with either BFP (y-axis) or RFP (x-axis).


## Private Scripts Description

### postprocess_result_txt.R

Function to perform the postprocessing using custom scripts from the [Emmanuel Levy Lab](https://www.weizmann.ac.il/CSB/ELevy/home). In brief, these scripts filter background fluorescence, outliers and cells outside of a specified size range from the raw data. A detailed description can be found in this [preprint](https://doi.org/10.1101/260695).

### Multiwell.R

This module contains several function that are used to process multiwell plates such as 96 or 384 well plates and to ensure correct mapping of screening order to plate layout.

### general.R

This module contains several auxiliary functions related to color scales, matrix manipulation and memory checks.

### MicroscopeToolBox.R


