## Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree. In this project, we build and optimize an Azure ML pipeline using the Python SDK, HyperDrive and a provided Scikit-learn model. This model is then compared to an Azure AutoML run.

## Useful Resources
•	ScriptRunConfig Class

•	Configure and submit training runs

•	HyperDriveConfig Class

•	How to tune hyperparamters

## Summary
The dataset contains information about loan applicants, including martial status, education, loan status etc. The objective is to predict whether or not the client will subscribe a term deposit (binary, yes/no), so the target column is "y.""

### The best performing model generated by a sample HyperDriveConfig was with Accuracy: 0.9098 C: 10 max-iter: 200

## Scikit-learn Pipeline
The Scikit-learn pipeline helps in developing and optimizing a machine learning model using Azure ML. Below is the explanation of the data scource, Pipeline architecture, hyperparameter tuning and classification algorithm.

Data source: Tabular Data is created from delimited files loaded from a given path. Post which it is cleaned and split into testing and validation sets.

Compute Cluster : A compute cluster is defined using Azure ML and provides the necessary computational resources for training the machine learning model.

Environment Setup for Estimator: An environment is defined using a Conda environment specification (conda_dependencies.yml). This environment ensures the availability of libraries dependencies required for the training script. Using this environment and the training script, Estimator is configured

Hyperparameter tuning: RandomParameterSampling is used to explore different combinations of hyperparameters (--C and --max_iter). Explicit choices is given to reduce computational time and large number of runs.

HyperDrive Configuration: HyperDriveConfig ties together the estimator, hyperparameter tuning, metrics, Accuracy, optimization goal, and early stopping policy (policy). It specifies the maximum number of total runs and concurrent runs allowed during hyperparameter tuning.

Then multiple runs are executed with different hyperparameter combinations. The best performing model is selected based on the Accuracy metrics. The corresponding hyperparameters (--C and --max_iter) used in this best run are also retrieved.

## Benefits of the Parameter Sampler:

RandomParameterSampling randomly selects hyperparameter values from specified distributions. It also allows the user to provide explicit choice if the user wants to get a first idea if everything is tied together correctly and working as expected. Later more rigorous sampling can be done. Random sampling also takes less time thant grid search.

## Benefits of the Early Stopping Policy
BanditPolicy was chosen as the early stopping policy for continously monitoring the performance of individual training runs and terminates poorly performing runs early. This prevents wasting resources on less competitive results.

## AutoML
AutoML generated a pipeline that used various feature engineering and scaling algorithms for e.g. StandardScalerWrapper, MaxAbsScaler, SparseNormalizer and TuncatedSVDWrapper. These feature engineering algos were combined with Various Classification algorithms by the AutoML Pipeline for e.g. XGBoostClassifier, RandomForest, SGD, LightGBM, LogisticRegression etc.

## Pipeline comparison

### The AutoML model achieved an accuracy of 0.91657 using the algorithm VotingEnsemble, whereas the hyperparameter-tuned model from the HyperDrive achieved an accuracy 0.9098634294385433.

The reason for better performance of AutoML is that it explores multiple machine learning models and configurations automatically, including the preprocessing steps. However, HyperDrive only optimizes the parameters of a single pre-defined classification algorithm (LOgistic regression in this case) and chooses randomnly (or only out of given explicit choices in this case) the pre-defined varying parameters (regulairsation strength and max iterations in this case ) of the classification algotihm.

AutoML's exhaustive search across multiple models and hyperparameter configurations including feature engineering have led to a better-tuned model compared to the limited combinations tested in the HyperDrive experiment.

AutoML also gives explanations of the chosen models as below: 

![image](https://github.com/saxenam06/Azure_MP_OptML/assets/83720464/6f297674-af1c-4b3a-8f65-f7f9d75cdb40)

![image](https://github.com/saxenam06/Azure_MP_OptML/assets/83720464/c8fa4478-3d0e-4f89-9862-20fbaf9d743e)

![image](https://github.com/saxenam06/Azure_MP_OptML/assets/83720464/0544a3a2-d5f5-4f40-a0df-e710431329ab)

## Future work
Possible areas for future work could be: Implement ensemble methods such as stacking or blending to combine predictions from multiple models, including those from AutoML and the manually tuned model.
Improve Interpretability using feature importance and model explanations.

## Proof of cluster clean up
![image](https://github.com/saxenam06/Azure_MP_OptML/assets/83720464/5121defb-8c4a-4f99-b71a-a0fe00e96f81)
