P. Lara provides an example on training XGBoost models through the station-specific DET model workflow and 
they use identical (meta)model inputs and architectures for each of the 6 independent models.

Thus, by inspecting the workflow involved with training a DET model from scratch, P. Lara also shows users
how to (re)train other models.

In the depths of the `pb_save_DET_model.py` script (around line 152) they set the x_train as the output of a df_train.iloc[:, ] that returns training data feature vectors (and they drop some old features). Thus it appears that each labeled feature vector is a row-vector of this training feature matrix.

Similarly there's a y_train vector that contains elements that are the labels associated with each x_train row.

It appears that DET base model is trained with all available data, rather than the K-folds cross validation used in the rest of the model training architecture

(Re)training any of the other models thus becomes pulling a given set of base models (which are scikit-learn pipelines)
and continuing the model-wise training routine set out in `pb_save_DET_model.py`. It (likely) becomes critical that each model is trained on a consistent Kth holdout validation set?

As a starting point, we need to build up a big pile of x_train feature vectors from waveform data ASAP. All of this pre-processing can be done and then we can split out the labeled feature vectors into:
1) Train
2) Validate
3) test.

Now, we have 10 base_models_ in each