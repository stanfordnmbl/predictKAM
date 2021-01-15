# A neural network to predict the knee adduction moment using the positions of anatomical landmarks obtainable from 2D or 3D video
*Authors: Melissa Boswell and Scott Uhlrich*

## Publication
This repository includes the models and dataset for the following paper. Please cite this paper if using this code or dataset:

Boswell MA*, Uhlrich SD*, Kidzinski L, Thomas K, Kolesar JA, Gold GE, Beaupre GS, Delp SL, 2021. A neural network to predict the knee adduction  moment in patients with osteoarthritis using anatomical landmarks obtainable from 2D video analysis. _Osteoarthr. Cartil._, _In Press_. *contributed equally. [Link to download](http://nmbl.stanford.edu/wp-content/uploads/BoswellUhlrich_NN_KAM_PreProofOAC.pdf)

## Task
Given only the position of motion capture marker positions, predict the peak knee adduction moment of each step during walking. Frontal-plane or sagittal-plane marker positions can also be used, to simulate keypoints identified from 2D video.

![Figure 1](https://github.com/stanfordnmbl/predictKAM/blob/master/figures/Figure1.PNG?raw=true)

## Running the code
The model performance using 3D positions can be visualized in the PredictKAM.ipynb jupyter notebook without re-running the code.

### Install Requirements
We used Python v3.6.8 to develop these models. Models were trained on a GPU, but will also train on a CPU rather efficiently.

Install required packages:
```
pip install -r requirements.txt
```
### Download dataset
After cloning the repository, download the dataset from [this link on SimTK](https://simtk.org/projects/predictkam), and put the _inputData.npy_ file in the _Data_ folder of the cloned repo.

### Running the code
The _PredictKAM.ipynb_ jupyter notebook pre-processes the data and allows for you to either load the previously-trained models from the paper and predict on held-out data, or to train the model from scratch using the dataset from the paper.

## The Dataset
The input for this model has a final shape determined by the number of marker positions and leg step by number of time steps, by number of steps.  The output has a final shape of 1 (peak KAM value) by the number of steps.

The data used in the final manuscript was collected from 3D motion capture data during gait from 86 people, giving a total of 112730 steps. The first 66 subjects in the dataset have medial compartment knee osteoarthritis, and the final 20 subjects were young adults without osteoarthritis. 

There are seven matrices in the inputData.py dictionary:
1. markers: the 3D position time series (30 timesteps) from 13 anatomical landmarks (C7, swing ASIS, swing PSIS, stance ASIS, stance PSIS, swing knee (lateral femoral epicondyle), swing ankle (lateral epicondyle), swing calcaneus, swing toe (2nd metatarsal head), stance knee, stance ankle, stance calcaneus, stance toe. The origin of the positions is the midpoint of the PSIS markers, and the mediolateral positions were reflected across the midline, such that all positions on the stance side of the midline are positive. Matrix size: 39 (13 markers x 3 dimensions) x 30 (timesteps) x 112730 steps.
2. KAM: the knee adduction moment curve (Nm) during stance phase for each step.
3. leg: the leg for each step (1 for right or 0 for left) propagated along the time series
4. height: subject height (m) for each step - used for normalization of KAM and marker positions
5. weight: subject weight (kg) for each step - used for normalization of KAM
6. subind: subject number for each step - used to split the dataset by subject
7. lassoDeletedInds3D: The indices (between 0 and 319) of the 3D input vector that were removed by lasso feature reduction.

After the data is loaded into the workspace, the height and weight matrices are used to normalize the marker positions by height and the knee adduction moment by height and weight, so it is expressed as %BW Ht.

## Code functionality
The code performs the following:
1. Flag if you want to train the model on 3D, frontal plane, or sagittal plane data
2. Imports the data
3. Resizes the input and output matrices to the correct format, normalizes data, uses only first half of stance
4. Calculates the peak KAM for each step
5. Divides into Test, Dev, Train sets
6. If 3D model, then pulls out features from the LASSO regression, removes redundant leg inputs for all models.
7. Flatten Input and Output Data
8. Predict KAM using a pre-trained model, evaulate on test set
9. Builds the model (if training from scratch)
10. Trains the model (if training from scratch)
11. Evaluates the performance with r^2, mse, and MAE for a selected split of the dataset
12. Plots the peak KAM predictions

## Resources
More information on data collection and preprocessing in the paper as well as the below paper:
- [Subject-specific toe-in or toe-out gait modifications reduce the larger knee adduction moment peak more than a non-personalized approach](https://www.ncbi.nlm.nih.gov/pubmed/29174534)
