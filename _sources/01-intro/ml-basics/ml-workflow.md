# Machine Learning workflow

The following represents a typical machine learning workflow.

![ml-workflow](../../_static/ml-workflow.png)


## 1) Data preparation

Retrieve and prepare the data. This step includes several tasks:

- The collection of data from several sources.

- Cleaning all data and making it coerent among the different sources. For instance, converting a variable to the same unit system.

- Transform data into more usefull forms.

- Perform some exploratory data analysis to evaluate the quality of data. Is the data balanced? Are there outliers? 

- Store the data in an useful format for further processing.

Collecting, cleaning and organizing data can represent 80 % of the time invested in a data analysis project, accoring to [Forbes](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/?sh=85080856f637). At the same time it is usually the less enjoyable part of data science.


## 2) Feature engineering

Construct relevant features from the initial data or from computation/simulation. For instance, for a chemical related project this can mean:

- Collecting compound or molecule properties.

- Computing cheminformatic molecular descripotors.

- Perform computational simulations.

Then, one must select which of the available features are of interest and relevant for model development. This is done by employing feature selection methods.


## 3) Model development

Model development is an iterative process that is repeated until a sucessful model is achieved. It involves the follwing tasks:

- Select the machine learning algorithm. 

- Select the desired performance metric.

- Train the model.

- Evaluate the model performance.

- Optimize model hyperparameters.


## 4) Model testing

Check the model for potential underfitting or overfitting.

Test the models agains previously unsean data to assess its predictive performance. 


## 5) Application

Deploy the model in an useful manner for the end user. This can mean the creation of an app that receives the user inputs and returns the target result.


## References

- [Multiscale Computational Approaches toward the Understanding of Materials](https://doi.org/10.1002/adts.202200628)