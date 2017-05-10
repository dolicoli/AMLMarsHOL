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

![alt text](https://88qylg-sn3302.files.1drv.com/y4mIquqSMKsPXCVRyGsZgNdTlwXet_VCYv_G-fr6edecylq5N46bfsqB2V47XS5ea3ZUL06HJ4W_W7LdRAQjuR4HXfTxPnSHF4AMH1Q-kxE2kmU-L1IZ82cKwVqitrgCYXcXfTF_V8Ltmlm5OzBzN62CfRBhRWRI7T5a-BNZPM0ktrs9MCcgAvQeY5lluL0KnbkKXdIskdn-LcnUcPw3rb7tg?width=395&height=907&cropmode=none "Dashboard")

### Verifying model

Drag and drop Score model from left panel and connect the training model result to left input, and use the test data as another input. 

### Evaluate Model

Drag and drop Evalute model from left panel and connect the result for Score model as input in Evalute model. Evaluating the model will give you the accuracy level of the traning model. You will see the value is around 90%. Don't panic. The reason why it is almost perfect is because i am using console program to generate the training data. In real life, it will be varied depend on your test data. You will see the percentage of how each of the text is being categorized to their respective severity level. 

![alt text] (https://88qxlg-sn3302.files.1drv.com/y4me0smi1xuiIbgcWtmGVmm1ill1lnprG4DbqBVlCO1mgH6ssAgpbnzzMXQnm191VQ5FXEGw6thLeXiwadcW9of3tG-etXizZKDaWHObKBIeER9AwxCVQAD0e0CLIj9rlUhFEztGMEDa5N3HbuXRQiF7uWl2QuNd_IIhlvPSKEreDN6cPVo3oWI4h7ukuWwSU9PE1vrcrKDScBkxSV52ZKhXQ?width=991&height=929&cropmode=none "Dashboard")

### Run Experiment
 
 You can now run experiment where it will look like this.
 
 ![alt text](https://9sqqlg-sn3302.files.1drv.com/y4mGIypk26m4rKg7i3yj4udkZTbGwwl0KDU-69DrbK4-e1YSe9EvwXvJQsbXrQvD5t208WyXzyaLFw9NztjMc3i1RI11-1NV_ehlRCu0nww7hauGQyWZ2bznQfnOHQvNNHx_k3LYm2FZVzqDO4qiyH3plRgm7aUePCo2J_9VuNl8A1Kmx_s_yhaN55MBtuJZsPGtI5qXa-3wifNd3IEpiMQow?width=985&height=904&cropmode=none "Dashboard")
 
 ## Mission 2 Completed
 
 If you want to verify the experiment, the end result should be look like this URL : 
 
 https://gallery.cortanaintelligence.com/Experiment/Log-Classification
