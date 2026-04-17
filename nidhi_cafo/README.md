Concentrated Animal Feeding Operations (CAFOs) play a significant role in shaping 
environmental risk and zoonotic disease emergence, including outbreaks of highly
pathogenic avian influenza (HPAI) H5N1. As the need to understand and model the 
emergence of HPAI H5N1 intensifies, it is necessary to consider the spatial 
distribution of CAFOs, given the increase in cross-species viral transmission. 
Currently, no comprehensive dataset of CAFO locations exists for the United States. 
The goal of this project is to use these predictions to generate a spatially 
continuous dataset of estimated CAFO density across the contiguous US, enabling 
future efforts to model HPAI H5N1 and assess the environmental and public health 
risks associated with CAFOs. Due to incomplete and inconsistent reporting of CAFO
locations across states, this project aims to develop a semi-supervised spatial 
machine learning framework to estimate CAFO counts at a 10 km x 10km grid 
resolution, with the goal of improving coverage in underreported states. The final 
dataset provides CAFO counts at a 10 km grid resolution, along with the associated
agricultural, socioeconomic, spatial, and environmental predictors.

The dataset and feature data to run the model are hosted at:
https://drive.google.com/drive/folders/18m86MOTIcd_90mrV3b1JSxFwAH_ZHnIW?usp=sharing
If the data is not publicy accessible, please email nidhi.ram[at]columbia.edu. 
The files in this folder include: us_grid_ (shapefile of the full grid of features), 
combined_cafos (shapefile of all known CAFO locations), CAFO_Density (shapefile 
of EPA-published county CAFO counts), and state_totals_2024.csv (state CAFO counts).

The full workflow to build this model was implemented in XGBoost_model.Rmd.
It includes training dataset construction using negative downsampling, XGBoost 
machine learning modeling, prediction generation for the entire study region,
post-calibration using a polynomial regression model, and model evaluation.

Data: Make sure to have the data folder downloaded to successfully run the code.

Negative Downsampling: The training dataset was constructed using negative downsampling. 
Because of the over-representation of zero-count cells in the labeled data, a 
random subset equal to 50% of the number of positive cells (>0 CAFOs) was sampled 
to create a balanced set of negative observations. The resulting training dataset 
thus consisted of Positive cells (CAFO count > 0) and downsampled Negative cells 
(CAFO count = 0).

Model: We then employed a semi-supervised machine learning approach to build a spatial 
XGBoost model to predict CAFO density in the United States. Model training was 
implemented using the caret framework in R. Predictions were generated in the 
log space and subsequently transformed back to the original count scale for analysis. 

Prediction: After model training, predictions were generated for all unlabeled 
grid cells located in states without comprehensive CAFO reporting.

Post-Calibration: After generating initial predictions from the model, we employed 
a post-calibration technique. We used a polynomial regression model fitted between 
raw model predictions and observed counts in the training dataset. This process 
aimed to reduce the systematic under or over estimation of CAFO counts and 
improve ranking.

Model Evaluation: We applied spatial cross-validation to assess the performance
of the model. We evaluated predictions using RMSE, MAE, R2, and Pearson correlation.
We also analyzed residuals and spatial error patterns to understand the underlying
error structure of the model.

Note: The files to spatially downscale all the feature data to the grid are included
in the supplementary_code folder, but the data itself are not included. The links
to all data used in this project can be found in the paper. 
