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

#### Split Data

Use Split Data features to split data into 2. Use "Rows" as a splitting mode and 0.7 as a fraction to split. 
We will use 70% of the dataset as training data, and 30% as test data. 


By end of this mission, your dashboard should look like this


![alt text](https://ys0g2g-sn3302.files.1drv.com/y4m6K8b7y7jIaXrNeRMoDfvlARaeXH2k94HT2OXRty_9Tipk1hmLVznGVZrbuCBXg3gxGAQu4aZ53tY5OBDxo1ojQ-Nd_qI5Vvh6Tx4EJolVxhuoejqqQfW1B4Edu7ZEIjBmUf_rfBshaM97eI_NePiS09eE3wRoYRBYE9c_8ok9nc3Cuk_U4qtli5fFLPatqJzJaXMPgVmqvZV4ktOudvB0w?width=704&height=608&cropmode=none "Dashboard")