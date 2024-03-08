# SFF_TRANSFORMERS




## Data

GOES data was captured using the sunpy_5 library, using the sunkit_instruments.goes_xrs function. Explosion data was captured from class A1. In this database, data from May 1, 2010 to February 31, 2023 were used.
The SHARP data were obtained through a URL provided by JSOC, which allowed the download of the hmi.sharp_720s database, allowing us to choose the time period and parameters. The SHARP data were selected from the list of solar flares obtained by GOES.
This script was created from the script by Jacob Conrad Trinidad (https://stacks.stanford.edu/file/druid:yv269xg0995/Predicting%20Solar%20Flares%20using%20SVMs.html), realizado a partir do trabalho de Bobra e Couvidat (2015) 

(https://doi.org/10.1088/0004-637X/798/2/135). 

## Model

This repository contains the source code and research data "article_date", de <author_name>. 

All models used (MLP, SVM, LSTM and Transformers) were created using the [TensorFlow](https://www.tensorflow.org) library, together with [Keras](https://keras.io). Additionally, the [Sklearn](https://scikit-learn.org/stable/) library was used for other features related to the models, such as data division, normalization and evaluation metrics. For SMOTE balancing, the [Imbalanced Learn](https://imbalanced-learn.org/) library was used.



## Balancing

All models can be trained with unbalanced or balanced data, using undersampling, oversampling, and SMOTE techniques.

## Division of sets

The division between the training, validation and test sets can be done randomly or chronologically, considering the recording date of each event.

## Normalization

In all models, data normalization occurred using the StandardScaler library, which sets the mean to 0 and standard deviation to 1.

## Hyperparameters

|Model| Description|
|------------|------------|
|MLP         | 16 dense layers ReLu activation <br> dropout 50% sigmoid activation|
|LSTM        |8 units LSTM layer <br> 2 dense layers linear activation |
|SVM         |dense layer <br>Fourier function scale 10 |
|Transformers| 4 encoders <br> 4 attention heads size 256 <br> dropout 25% <br> normalization layer for each encoder <br> one-dimensional convolution connect layer with softmax activation|

Use

|Variables|Description|
|---------|-----------|
|results_file  | file path that will store the results of the model run|
|data_file     | patch of the .csv file with the data|
|separator_file| character that separates the columns of the CSV file (; , ) |
|set_balancing | Indicates the model, balancing, division of sets and weights<br><br> set_balancing = [model, balancing, data_division, weight]<br><br> model = mlp, svm, lstm, transformers<br> balancing = original, undersampling, oversampling, smote<br> data_division = data, random<br> weight = True, False |
|list_col_delete| List of fields that must be excluded from the original set.<br><br>lista_col_delete  = ['DATE','DATE_S','DATE_B','DATE__OBS',<br>'DATE-OBS','T_OBS','T_REC_formatado','Class_2',<br>'class_flare','Ano']<br><br>Exclusion of parameters according to the study by Bobra and Couvidat (2015)<br><br>10_parameters = ['DATE','DATE_S','DATE_B','DATE__OBS',<br>'DATE-OBS','T_OBS','T_REC_formatado','Class_2',<br>'class_flare','Ano','MEANGAM','MEANGBT',<br>'MEANGBZ','MEANSHR','MEANGBH','MEANJZH',<br>'MEANJZD','MEANALP']<br><br>all_parameters_sharp =['DATE','DATE_S','DATE_B','DATE__OBS',<br>'DATE-OBS','T_OBS','T_REC_formatado',<br>'Class_2','class_flare','Ano','TOTUSJH',<br>'TOTPOT','TOTUSJZ','ABSNJZH','SAVNCPP',<br>'USFLUX','AREA_ACR','MEANPOT','R_VALUE',<br>'SHRGT45','MEANGAM','MEANGBT','MEANGBZ',<br>'MEANSHR','MEANGBH','MEANJZH','MEANJZD','MEANALP'] |
|date_chronological |Date ranges of training, validation, and test sets. <br><br>Entire database:<br><br>date_chronological = [train_start_date, train_end_date, <br>validate_start_date, validate_end_date, test_start_date, <br> test_end_date] <br><br>Partial database - high and low of the solar cycle <br><br> date_chronological = [train_start_date_1,<br> train_end_date_1, train_start_date_2, <br>train_end_date_2, validate_start_date, validate_end_date,<br> test_start_date, test_end_dade]|

## Example







 



