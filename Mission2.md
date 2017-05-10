# Mission 2

## Mission Objective

You will need to train the data using Feature Hashing and Classification algorithm in order to classify the text into specific category. 
Please be aware  


## Mission 1 DeBrief

On Mission 1, you have clean the data and split it for training and test data. 
In this mission, we will use feature hashing and multiclass model to train our data. 

### Training Data

#### Feature Engineering

The Feature Hashing module can be used to convert variable-length text documents to equal-length numeric feature vectors, using the 32-bit murmurhash v3 hashing method provided by the Vowpal Wabbit library. The objective of using feature hashing is dimensionality reduction; also feature hashing makes the lookup of feature weights faster at classification time, because it uses hash value comparison instead of string comparison.

In a nutshell, we used the Feature Hashing module to convert the plain text of the articles to integers and used the integer values as input features to the model. In order to do this, we need to select column text on the feature hashing.

### Select Column in Dataset

We need to remove text_column by selecting launch column selector. we are doing this because the hashing feature already convert all the texts into computable integers, hence we do not need the original text anymore.  

### Training Data

We will use multiclass decision forest to train the data, as the main objective of this mission is to classify the text to severity category. AzureML provides few classification algorithm that you can choose to execute the objective. 

What you need to do is to drag multiclass decision forest from the left panel and also train model. 

From multiclass you connect it to left input of train model and connect the training data with hashing to right input. 


### Verifying model

Drag and drop Score model from left panel and connect the training model result to left input, and use the test data as another input. 

### Evaluate Model

Drag and drop Evalute model from left panel and connect the result for Score model as input in Evalute model. 

### Run Experiment
 
 You can now run experiment where it will look like this.
 
 ![alt text](https://ys0n2g-sn3302.files.1drv.com/y4mhblmGgpb_t3HpHOsM1Q0AkMuanqlRMz4FQCcycjgDFxjNNbbfmA2dH2rggWUXMyPeynUykbzsTTh1J9F80-LoeX6jiQ3OttiOj7_8gh1uI0vcgn5sbrojeJWiynNoDgWf0xcg9mqwqtD6PEQKkoj9QchTeji171HVZlZkVvWYlc1X38c--KydnC4u867FXU4GnAGB60o6EMM1Njzw8msTQ?width=880&height=879&cropmode=none "Dashboard")
 
 ## Mission 2 Completed
 
 If you want to verify the experiment, the end result should be look like this URL : 
 
 https://gallery.cortanaintelligence.com/Experiment/Log-Classification
