# SFF_TRANSFORMERS




## Data

GOES data was captured using the sunpy_5 library, using the sunkit_instruments.goes_xrs function. Explosion data was captured from class A1. In this database, data from May 1, 2010 to February 31, 2023 were used.
The SHARP data were obtained through a URL provided by JSOC, which allowed the download of the hmi.sharp_720s database, allowing us to choose the time period and parameters. The SHARP data were selected from the list of solar flares obtained by GOES.
This script was created from the script by Jacob Conrad Trinidad (https://stacks.stanford.edu/file/druid:yv269xg0995/Predicting%20Solar%20Flares%20using%20SVMs.html), carried out from the work of Bobra and Couvidat (2015) (https://doi.org/10.1088/0004-637X/798/2/135). 

The final base is available on Zenodo (https://zenodo.org/records/10800828)

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
|separator| character that separates the columns of the CSV file (; , ) |
|url_data     | patch of the .csv file with the data|
|list_col_delete| List of fields that must be excluded from the original set.<br><br>lista_col_delete  = ['DATE','DATE_S','DATE_B','DATE__OBS',<br>'DATE-OBS','T_OBS','T_REC_formatado','Class_2',<br>'class_flare','Ano']<br><br>Exclusion of parameters according to the study by Bobra and Couvidat (2015)<br><br>10_parameters = ['DATE','DATE_S','DATE_B','DATE__OBS',<br>'DATE-OBS','T_OBS','T_REC_formatado','Class_2',<br>'class_flare','Ano','MEANGAM','MEANGBT',<br>'MEANGBZ','MEANSHR','MEANGBH','MEANJZH',<br>'MEANJZD','MEANALP']
|date_chronological | Date ranges of training, validation, and test sets. <br><br>Entire database:<br><br>No division by date (random)<br><br>date_chronological_empty = []<br><br>Division by date<br><br>date_chronological = [train_start_date, train_end_date, <br>validate_start_date, validate_end_date, test_start_date, <br> test_end_date] <br><br> Partial database - high and low of the solar cycle. Preceded by the phase of the solar cycle (A1, A2, A3, A4) <br><br> date_chronological_A1 = [train_start_date_1,<br> train_end_date_1, train_start_date_2, <br>train_end_date_2, validate_start_date, validate_end_date,<br> test_start_date, test_end_dade]|
|set_dataset_base|Indicates in the results file the base used. Default value "all_abcmx". |
|set_window|Indicates the prediction window in the results file. Standard value, based on the base used: 24h |
|set_epoch|Indicates the number of epochs used in training. |
|set_batch|Indicates the Batch size. |
|set_model_name|Array with the models that will be trained. All models or just some of them can be used. <br><br>Default value: ['mlp', 'svm', 'lstm', 'transformers']. |
|set_dataset_base|Array with the divisions of the dataset. One or all of them can be used. <br><br>Default value: ['all', 'A1', 'A2', 'A3', 'A4'] |
|set_balancing| Array with the balances used in training. One or all of them can be used. The model will be run once for each one. <br><br>Default Value: ['weight', 'undersampling', 'oversampling', 'smote']|










## Example

``` python
#path dataset
separator = ','
url_data = 'data/sharp_abcmx-nf-all_6h_2010-01-01_2024-04-30.csv'

#Coluns Delete in CSV file
#list_col_delete = ['DATE','DATE_S','DATE_B','DATE__OBS','DATE-OBS','T_OBS','OBS_VR', 'QUALITY', 'Class_Flare']

#Attributes' redution 
list_col_delete = ['DATE','DATE_S','DATE_B','DATE__OBS','DATE-OBS','T_OBS','T_REC_formatado','Class_2','class_flare','Ano','MEANGAM','MEANGBT','MEANGBZ','MEANSHR','MEANGBH','MEANJZH', 'MEANJZD','MEANALP'] 

date_chronological_empty = []
#date_chronological = ["2010-01-01 00:00:00", "2014-10-26 23:59:00", "2014-10-27 00:00:00", "2015-10-30 23:59:00", "2015-10-31 00:00:00", "2023-12-31 23:59:00"]

#date A1 - 2010-2011
date_chronological_A1 = ["2010-05-03 00:00:00", "2010-10-31 23:59:59", "2011-06-01 00:00:00", "2011-12-31 23:59:59", "2010-11-01 00:00:00", "2011-01-31 23:59:59", "2011-02-21 00:00:00", "2011-05-31 23:59:59"]

#date A2 - 2012 - 2013
date_chronological_A2 = ["2012-01-01 00:00:00", "2012-08-31 23:59:59", "2013-07-01 00:00:00", "2014-08-31 23:59:59", "2012-09-01 00:00:00", "2013-01-31 23:59:59", "2013-02-01 00:00:00", "2013-06-30 23:59:59"]

#date A3  - 2014-2016
date_chronological_A3 = ["2014-09-01 00:00:00", "2015-03-31 23:59:59", "2015-10-01 00:00:00", "2016-06-30 23:59:59", "2015-04-01 00:00:00", "2015-06-30 23:59:59", "2015-07-01 00:00:00", "2015-09-30 23:59:59"]

#date A4 - 2016-2018
date_chronological_A4 = ["2016-07-01 00:00:00", "2017-02-28 23:59:59", "2017-09-01 00:00:00", "2018-07-31 23:59:59", "2017-03-01 00:00:00", "2017-05-31 23:59:59", "2017-06-01 00:00:00", "2017-08-31 23:59:59"]

#set parameters before training
set_dataset_base = "all-abcmx"
set_window = "24h"
set_epoch  = 10
set_batch = 64
set_model_name = ['transformers'] # ['mlp', 'svm','lstm','transformers']
set_dataset_base = ['A1'] #['all', 'A1', 'A2', 'A3', 'A4']
set_balancing = ['weight', 'oversampling'] # ['weight', 'undersampling', 'oversampling', 'smote']


```


## Data Collection

In the data directory we have some files that are important for capturing data. If you want to make a new collection, it is important that they are deleted so that the script creates updated versions of them:

### all_harps_with_noaa_ars.txt

Lists the active regions mapped, showing their number corresponding to HARPNUM, which is used in the SHARP database and their NOAA_ARS number, used by GOES. This file is constantly updated and the script captures the latest available version. His official address is:

http://jsoc.stanford.edu/doc/data/hmi/harpnum_to_noaa/all_harps_with_noaa_ars.txt

### Set Variables

**start_date** <br>
**end_date**

Datetime start and end to capture date

_Ex:_

start_date = '2010-05-01'
end_date = '2023-03-31'’

### Alter forecast window 

date_event = event['peak_time'].to_datetime() - dt.timedelta(hours=24)







 



