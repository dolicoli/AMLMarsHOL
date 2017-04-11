# AMLMarsHOL

## Mission Brief

You and your crew on Mars are dependents on how well your spacecraft is. Your task is to make sure the spacecraft is in good condition, but of course, there are unpredictable errors and this is identified by lots of logs recorded by the system. With the capabilities from your spacecraft to connect to azure, you have been tasked to identify the error logs by their severity. You will need to deploy a smart prediction system that can identify the logs, and marked them as per the severity. Based on the previous mission, they have given you the previous logs and marked them with ranking of Low (identified by severity of 1,2, and 3) and High (4,5, and 6). 

### Mission Objective

You will need to deploy Azure Machine Learning solution that trained to identify the severity level of upcoming logs. Your system must be able to extract the input and predict + identify the severity level. If the logs identified as high level severity, you will need to save it into Database given by the command center for them to advice on potential solutions. 

## Mission Walktrough

Your Machine learning solution should be able to train the system and categorize the severity of Low or High. 

Mission 1 : You will need to preprocess the input before training them as the logs is not as clean as you expect by using R language and following the format given. 

Mission 2 : You will to train the data and experimenting with different algorithm provided by Azure Machine Learning and find the most effective prediction. 

Challenge : Create a simple site where your crew can input the unknown entry, classify them, and if the severity is high, save them into database. 

Good luck and have fun!
