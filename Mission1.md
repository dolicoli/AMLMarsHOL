# Mission 1

## Mission Objective

You will need to preprocess the input before training them as the logs is not as clean as you expect by using R language / Module and following the format given. 

Open the file using any text editor, and you can see that on logMessage column, 
there are additional info that not needed which are separated with || delimiter. Our mission is to clean the logMessage column and only have the logMessage to be trained. 

## Getting Started

Go to studio.azureml.net. You can create free account of you don't have one. 

The first thing you need to do is to upload the Dataset contained in this github as your Saved Dataset. 
Click New from the bottom left -> From Local File -> 

Select the 'ShipLog.csv' File on the file upload modal. 
The type should be Generic CSV File with a header. 

After Uploading the data, we can start using the studio. 

The overall workflow for this project is explained below : 

![alt text](https://88qrlg-sn3302.files.1drv.com/y4myorUNM2d55gFcl_1LO5BxBin1yDQMjW6DGx5dQmNQwbcorR6YJnqlR-oMG_GPHjNqE89hUcjq2E8zPRSwZzLZ5edNN0PYCDoz74BVlQU0Taz4pIfC7lg2s1PHN_WpgqINw-aVvpzHSIPPoSdfWL3wx3QoS5lQgWqcEyYsA5HvGJl4LY7z-9BpGkJSoAUnyCl-dHThG1FLYNnUgTiv_tc1g?width=1041&height=177&cropmode=none "Dashboard")

### Adding new Experiments

You can create new expriment by clicking New -> Experiment tab -> Blank Experiment

You will be then redirected to the dashboard where there are 3 panel shown. 
The left one will be the experiment items list. It contains all the modules that you need to run your experiment, including inserting data input and output, or even web service module.

The center of the dashboard will be where you drag and drop items from the left panel, connect each items from one to another, and view additional settings/vizualization. 

### Inserting dataset

You can insert the dataset by selecting Saved Datasets from item pabel, select My Datasets, and click on the uploaded dataset.
Drag and drop the item into the dashboard, or simply double click it. 

### Data Preparation

#### Execute R Script

As the mission objective said, we will need to clean the data by splitting the column "logMessage". You will notice that the said column has || delimiter. We will use R script to achieve this by drag and dropping the "Execute R Script" item from left panel. 

Connect the dataset with the R script by dragging a line from left output of the dataset to input port of the R script.

Replace the script inside with this :

```
# Map 1-based optional input ports to variables
df <- maml.mapInputPort(1) # class: data.frame

splitdata <- strsplit(as.character(df$logMessage),'||',fixed=TRUE) 

dfoutput <- data.frame(df$id, df$severity, do.call(rbind, splitdata))

maml.mapOutputPort("dfoutput");
```

df will take the data frame from input port 1. 
We then create a variable splitdata where it will perform a string split by selecting column logMessage with delimiter of || and make sure that it only match that delimiter.

We then perform the function by also combining the data with the id, severity level, and of course, the splitdata function.  

It will then become an output of data.frame. 

#### Mapping new column

After the R script executed, you will then need to exclude the unusable column, which is X1. 
Drag and drop "Select Column In Dataset" module and connect the R output to the input of this module. 
On the right panel you click on the Launch Column selector, and click "Exclude" and select X1 column to exclude. 
This will create a new output that has all the column except X1 column. 

#### Edit Metadata

You may want to drag the "Edit Metadata" and drop it into the dashboard to rename the column name. Choose column selector from the right panel and 
select to include df.severity, df.id, and X2. 

On new column names field, 
put id, category, and text.


#### Clean Stop Words
The next thing we need to is to clean the text to make sure that all the stop words are removed. If you notice the data that we have, there are numbers on the text as well. We need to exclude it from the training data using this text preprocessing. We will use a public available stop words from Microsoft as an input that connected to a text preprocessing package. Use Import Data module, select Data Source to get it from "Web URL via HTTP"
and url from : http://azuremlsamples.azureml.net/templatedata/Text - Sentiment Stopwords.tsv
and change the data format to TSV.


#### Uploading Preprocessing Text Package. 
Inside the preprocess folder, there are .zip package that contained an .R file which will do the preprocessing for you. 
Click New -> Dataset -> Upload File. Choose Zip File on "Select a type for the new dataset" and put text.preprocessing.zip as name.

#### Execute R script
Drag and drop the text.preprocessing.zip from "My Dataset" to the dashboard.After that combine the input data (input data 1 from the train data, input data 2 from the stop words, and input 3 from the preprocess text). Like the image below :

![alt text](https://789rig-sn3302.files.1drv.com/y4mZh9izpgdc--6lhIHrt54w5QbtRUwR8wfbotV8ls2XH6EupaZRSHquxyVSqhP7MQ4j9aK93VeUP_wluhCbHyLMQl8opvpFsuExBFTydK3OjqY9ztKd6Tzq7AteyUEHr5HqZSW5IICwK4UIJDSJHybn2kmzoorJaN_Y112lx7mK4PZxmYdyeZap1_z3Bdx_Z5t_szQoIhllYWCIwsh_qoxPQ?width=917&height=319&cropmode=none "Dashboard")


Execute the script below : 
```
# # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # 
# Please determine the required text preprocessing steps using the following flag 
replace_special_chars <- TRUE
remove_duplicate_chars <- TRUE
replace_numbers <- TRUE
convert_to_lower_case <- TRUE
remove_default_stopWords <- FALSE
remove_given_stopWords <- TRUE
stem_words <- TRUE
# # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # 

# Map 1-based optional input ports to variables
dataset1 <- maml.mapInputPort(1) # class: data.frame
# get the label and text columns from the input data set
text_column <- dataset1[["text"]]
label_column <- dataset1[["category"]]

stopword_list <- NULL
result <- tryCatch({
   dataset2 <- maml.mapInputPort(2) # class: data.frame
   # get the stopword list from the second input data set
   stopword_list <- dataset2[[1]]
}, warning = function(war) {
   # warning handler 
   print(paste("WARNING: ", war))
}, error = function(err) {
   # error handler
   print(paste("ERROR: ", err))
   stopword_list <- NULL
}, finally = {})
 
# Load the R script from the Zip port in ./src/
source("src/text.preprocessing.R");
                            
text_column <- preprocessText(text_column, 
                         replace_special_chars,
                         remove_duplicate_chars,
                         replace_numbers,
                         convert_to_lower_case,
                         remove_default_stopWords,
                         remove_given_stopWords,
                         stem_words, 
                         stopword_list)                   

data.set <- data.frame(
                label_column,
                text_column,
                stringsAsFactors = FALSE 
                )    

# Select data.frame to be sent to the output Dataset port
maml.mapOutputPort("data.set")
```

#### Split Data

Use Split Data features to split data into 2. Use "Rows" as a splitting mode and 0.7 as a fraction to split. 
We will use 70% of the dataset as training data, and 30% as test data. 


By end of this mission, your dashboard should look like this


![alt text](https://88qslg-sn3302.files.1drv.com/y4mS8Tp2W1X1c1RwWdYzoP89nj1Et2HsQHodfir5_EbqYSbpSVq6xd5KfCpsSr9bRX6xZuekgWAgJN6dv03h58pHUlWmOqMjGsBw2lS-wTy1kR-PH4SwWfpfVTwg4R8smyS8qoaalc7eaQmdObVrnDXEybQlCwDiAVm22tSgeVaF3I4tkLy59UTP74N7cAPZuWmLH18cODnXvh_Ht06rnEdww?width=1343&height=834&cropmode=none "Dashboard")