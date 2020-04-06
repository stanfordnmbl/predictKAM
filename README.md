# Predicting the Knee Adduction Moment
*Authors: Melissa Boswell and Scott Uhlrich*

## This Repository
Predict the peak knee adduction moment during walking using neural network with motion capture marker position inputs.
This repository holds the code and text for generating the manuscript on OpenSim Moco, a software toolkit for solving optimal control problems with OpenSim musculoskeletal models.

The accompanying manuscript was first released as a bioRxiv preprint: .

The code folder contains Python scripts to train and run a neural network to predict the peak KAM.

The paper folder contains a pdf of the paper.

The data folder contains the data used by the code. Only data for people without osteoarthritis is currently available.

## Requirements
We used Python v3.6.8 to develop this code.  This code also uses the keras package.
```
pip install -r requirements.txt
```
## Task
Given only 3D motion capture marker positions, predict the knee adduction moment.
## The Dataset
The input for this model has a final shape determined by the number of marker positions, velocities, acclerations, and leg step by number of time steps, by number of steps.  The output has a final shape of 1 (peak KAM value) by the number of steps.

The data used to generate this model was collected from 3D motion capture data during gait from 98 people, giving a total of 128416 steps.  

The input is the 3D position time series (30 steps) from 13 anatomical landmarks (Toe (R/L), Heel (R/L), Ankle(R/L), Knee (R/L), Front Pelvis (R/L), Back Pelvis (R/L), Neck) per step along with scalar anthropometric data. There are three additional columns: the leg step as a binary (1 for right or 0 for left), a height constant, and a weight constant, all propagated along the time series, giving the data a shape of (42, 8, 128416). After the data is loaded into the workspace, the height and weight columns are removed from the matrix and used to normalize the marker positions by height and knee adduction moment by weight.  Velocities and accelerations are calculated from the positions along the time series and added to the matrix, which gives a final input shape of (118, 8, 128416). The knee adduction moment data file is one peak value per step for a shape of (1,128416). The subject index data file is the subject number per step for a shape of (1,128416) and is used to split up the data by person.

## Guidelines for Use
The code performs the following:
1. Imports the data
3. Resizes the input and output matrices to the correct format
4. Divides into Test, Dev, Train sets
5. Flatten Input and Output Data (Fully connected only)
6. Bulids the model
7. Runs the model
8. Evaluates the performance with r^2 and RMSE values

Depending on the data being used, you may want to adjust and tune the model hyperparameters.

## Resources
More information on data collection and preprocessing:
- [Subject-specific toe-in or toe-out gait modifications reduce the larger knee adduction moment peak more than a non-personalized approach](https://www.ncbi.nlm.nih.gov/pubmed/29174534)
