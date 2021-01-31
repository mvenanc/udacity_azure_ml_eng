![](images/initial.png?raw=true)


# ESRB Ratings + Predictive Modeling


# Introduction
In this Project we tend to develope a Machine Learning model for Predicting these ratings. To choose the best possible model, two different approaches. The first one was choosing an machine learning algorithm and then tried to find the best possible parameters to optimze its performance. In the scond approach, it was applied an Automated Machine Learning tecnique. All the project was build on top of Azure Machine Learning envrionment. 


Before going deep into the implementation, lets introduce what ESRB ratings is.


## What is ESRB and what are the various Categories?
ESRB stands for Entertainment Software Rating Board. It is an American self-regulatory organization for assigning content ratings to consumer Video games. It was established in 1994 in response to criticism of contoversial video games like mortal Kombat "Fatality, hmm...". ESRB has the following labels:

>> **RP** - Ratings Pending (1994-present) This symbol is used in promotional materials for games which have not yet been assigned a final rating by the ESRB.


>> **EC** - Early Childhood (1994-2018) Games with this rating contain content which is aimed towards a preschool audience. They do not contain content that parents would find objectionable to this audience.No longer used as of 2018 due to few titles using this, and all titles with this rating are replaced with the E rating.


>> **E** - Everyone (1994-present) Games with this rating contain content which the ESRB believes is "generally suitable for all ages".They can contain content such as infrequent use of "mild"/cartoon violence and mild language.This rating was known as Kids to Adults (K-A) until 1998, when it was renamed "Everyone".


>> **E10+** - Everyone 10+ (2005-present) Games with this rating contain content which the ESRB believes is generally suitable for those aged 10 years and older. They can contain content with an impact higher than the "Everyone" rating can accommodate, but still not as high as to warrant a "Teen" rating, such as a larger amount of violence, mild language, crude humor, or suggestive content.


>> **T** - Teen (1994-present) Games with this rating contain content which the ESRB believes is generally suitable for those aged 13 years and older; they can contain content such as moderate amounts of violence (including small amounts of blood), mild to moderate use of language or suggestive themes, sexual content, partial nudity and crude humor.


>> **M17+** -  Mature 17+ (1994-present) Games with this rating contain content which the ESRB believes is generally suitable for those aged 17 years and older; they can contain content with an impact higher than the "Teen" rating can accommodate, such as intense and/or realistic portrayals of violence (including blood, gore, mutilation, and depictions of death), strong sexual themes and content, nudity, and more frequent use of strong language.


>> **A** - Adults (1994-present) Games with this rating contain content which the ESRB believes is only suitable for those aged 18 years and older; they contain content with an impact higher than the "Mature" rating can accommodate, such as graphic sexual themes and content, extreme portrayals of violence, or unsimulated gambling with real currency. The majority of AO-rated titles are pornographic adult video games; the ESRB has seldom issued the AO rating solely for violence.



## Architecture

The architecture of the project is based on the Azure ML MLOps architecture. There are steps for data preparation, training, validation, deploy and validation the model.

![](images/entire_pipeline.png?raw=true)


## Dataset 

[![label](https://www.kaggle.com/static/images/site-logo.png)](https://www.kaggle.com/imohtn/video-games-rating-by-esrb)


## Data Analysis 

## Correlation

![](images/correlation.png?raw=true)


features blood_gore and blood has correlations of 0.5 and 0.41 respectively, more of these more sever will be the rating.
feature voilence has a correlation 0.47 and hence follows the above statement.

## Data distribution

It was applied an PCA of 2 components in order to vizualize the data in 2D.

![](images/data_boundaries.png?raw=true)

It is clear that there exists a decission boundary and then a Machine Learning approach can be applied to predict the rating.


## Screenshots

## Scikitlearn - Logist Regression 

First, it was applied an classical approach using a logist regression applied for multiclass, that is , all the reatings. To perform the task, it was used Azure ML sdk to submit a training script which accepts two parameters, C and max_iter. 

Besides that, Hyperdrive optmization was applied. Hyperdrive is a tool from Azure ML which is applied for hyperparameters optimization.

Below we can see the output of the run details. The parameters possibile values was and unfiorm distribution for **C** from 1.0 to 2.0, and for **max_iter**, it was chosen 100, 3000 and 5000. As stop criteria, it was used a BanditPolicy. And our goal was to maximize the Accuracy metric. 

![](images/step2_automl_run_details.jpg?raw=true)

The image below shows the best model found and its parameters.

![](images/step2_hyperparameters_best_model.jpg?raw=true)



## Automated ML

For the second approach, and Automated Machine Learning tecnique was applied. Azure Machine Learning presents this feature to make it easier to find good models for different kinds of problems, for instance, Time Series, Classification or Regression problems. This kind of tecnique, runs several different Machine Learning algorithms, each one can run in differents clusters to speed up the process of training and optimizing hyperparameters. With this AutoML, several diferent algorithms can be tests in a very short period of time. This task could be very time expensive if a Data Scientist choose to test several algorithms by yourself. 

Below, it is prsented the run details of the AutoML.

![](images/step2_automl_run_details.jpg?raw=true)

We can noticed that several different approaches was applied and not only Logistic Regression. For each algorithm, different parameters could be chosen. But all the complexity of this process was encapsulated in the process of AutoML.

The best model was ranked in the first place, which was an strategy of Ensemble of models. That is, a final model was created based in a composition of other models. 

Below, we can see details of the best model like confusion matriz and feature importance graph. 

![](images/step2_automl_bestmodel_runid.jpg?raw=true)


![](images/step2_automl_bestmodel_graphs.jpg?raw=true)


## Best model deployment

The best model found was the model which was trained with Automated Machine Learning, using Ensemble of models.

This model was deployed using Azure ML SDK to access the features of Azure ML environment. To deploy, and Azure Container Instance was chosed as the host for our socring file. 

Below we can see the details of the deployment and an endpoint inference test. 

![](images/step2_model_deployed_details.jpg?raw=true)


Endpoint inference test.

![](images/step2_test_infer.jpg?raw=true)


Another feature that we have when a model is deployd as an endpoint on Azure is to monitor the health and anothere metrics of the endpoint. This is possible turning on Application Insights. 

![](images/step2_enable_app_insights.jpg?raw=true)


## Screencast vídeo (Clik on the image - The video is hosted on youtube)

[![Watch the video](https://img.youtube.com/vi/oNAXpyJLlCo/maxresdefault.jpg)](https://youtu.be/oNAXpyJLlCo)



## Project future improvements

As future improvements, There is a chance to improve the model quality by investing time to investigate all the features and run some descriptive statistics analysis in order to find out how important are all the features. Try to select the best ones, as well create another feature. 

Also, a more detailed Automated ML can be run testing many more scenarios and also Deep Learning algorithms in attempt to find the best model.

New models can be compared. Different parameters can be tested using the hyperdrive and than, finding the best model and best parameters that generated the best possible model.

