# Predicting the Knee Adduction Moment
*Authors: Melissa Boswell and Scott Uhlrich*

## This Repository
Predict the peak knee adduction moment during walking using neural network with motion capture marker position inputs.
This repository holds the code and text for generating the manuscript on OpenSim Moco, a software toolkit for solving optimal control problems with OpenSim musculoskeletal models.

The accompanying manuscript was first released as a bioRxiv preprint: *link*.

The code folder contains Python scripts to train and run a neural network to predict the peak KAM.

The paper folder contains a pdf of the paper.

The data folder contains the data used by the code. Only data for people without osteoarthritis is currently available.

## Requirements
We used Python v3.6.8 to develop this code.  This code also uses the keras package.
```
pip install -r requirements.txt
```
## Task
Given only 3D motion capture marker positions, predict the peak knee adduction moment of each step during walking.

## The Dataset
The input for this model has a final shape determined by the number of marker positions and leg step by number of time steps, by number of steps.  The output has a final shape of 1 (peak KAM value) by the number of steps.

The data used in the final manuscript was collected from 3D motion capture data during gait from 86 people, giving a total of 142469 steps. The data provided in this repository is a subset of the data used in the final manuscript. It includes # of people without osteoarthritis, for a total of # steps.

There are six matrices of data the model needs as input:
1. markerPositions.mat: the 3D position time series (30 steps) from 13 anatomical landmarks (Toe (R/L), Heel (R/L), Ankle(R/L), Knee (R/L), Front Pelvis (R/L), Back Pelvis (R/L), Neck) per step
2. KAM.mat: the knee adduction moment curve timeseries perstep
3. leg.mat: the leg step as a binary (1 for right or 0 for left) propagated along the time series
4. height.mat: a height constant propagated along the time series 
5. weight.mat: a weight constant propagated along the time series 
6. subind.mat: subject number per step - used to split up the data by person

After the data is loaded into the workspace, the height and weight matrices are used to normalize the marker positions by height and the knee adduction moment by weight.

## Guidelines for Use
The code performs the following:
1. Flag if you want to train the model on 3D, frontal plane, or sagittal plane data
2. Imports the data
3. Resizes the input and output matrices to the correct format, normalizes data, uses only first half of stance
4. Calculates the peak KAM per step
5. Divides into Test, Dev, Train sets
6. If 3D model, then pulls out features from the LASSO regression (imports lassoInd.csv created outside of code)
7. Flatten Input and Output Data
8. Builds the model
9. Runs the model
10. Evaluates the performance with r^2 and MSE values
11. Plots the peak KAM predictions for the test set

Depending on the data being used, you may want to adjust and tune the model hyperparameters.

## Resources
More information on data collection and preprocessing:
- [Subject-specific toe-in or toe-out gait modifications reduce the larger knee adduction moment peak more than a non-personalized approach](https://www.ncbi.nlm.nih.gov/pubmed/29174534)
