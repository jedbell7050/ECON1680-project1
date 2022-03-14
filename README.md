# Project 1 - Machine Learning
This is the template for the first project for ECON1680: Machine Learning, Text Analysis, and Economics. This project is meant to be an economic application of machine learning. Over the course of the project, you should fill in the following sections of the Readme.md file below with your research question and motivation, your data sources, your methods, your results, and instructions for replciation. 

## Research Question  
How strong of a regressor is housing availability for both cost of living and income in metro areas? And at what number of units (both rental properties and personally owned houses) and cost of rent does it justify the development of new housing?

## Data  
American Community Survey:  
  Dataset of housing and economic statistics from 24 selected cities  
  Atlanta, Austin, Boston, Charlotte, Chicago, Denver, Detroit, Houston, Indianapolis, Jacksonville, Kansas City, Las Vegas, Los Angeles, Memphis, Miami, Minneapolis, New York, Philadelphia, Phoenix, Salt Lake City, San Francisco, Seattle, St. Louis, Washington DC
  
FRED:  
  Dataset of Median Real Wages Adjusted for Cost of Living by city and date  
  Dataset of building permits for 1 unit housing developments and the total number of building permits per city and date  
  Dataset of Case-Shiller Index by city and date  
  Dataset of Housing Supply in the US by date  
  
## Method
As a preliminary test, I plotted an OLS regression of real median wages vs each of the four feature variables (have only currently done this for the permit ratio and vacancies, I’m having trouble getting the data for the other variables into the correct format. I will see a librarian about this). 
	As I hypothesized, there was a negative correlation between the permit ratio and real median wages. It had a small R-squared of just 0.09, which got even smaller when I took out San Francisco and New York, but it did have a statistically significant coefficient. (Assuming the other variables have the correlation I expect, I would include all of them in my initial regression model)
	Somewhat surprisingly, vacancies had a negative coefficient that was also statistically significant, but only with a p-value of 0.037. This could be an example of reverse causality where the lower real median wages are actually causing housing units to be vacant since fewer people can afford them. I will address how to help deal with this later with an instrumental variable.
	The next step is to run the two-stage least squares regression with the control and instrumental variables (Regional Purchasing Parity) and each of the feature variables. This establishes a baseline model to compare my other models. I will then run a Lasso regression to see if any of the four feature variables are redundant and should be kicked out. 
	To compare the two models, I randomly split the data into stratified training and testing sets, train each model on the training data, and then compare each model’s average in-sample and out-of-sample mean squared errors between the actual and predicted real median wages across five cross fold validations. Whichever model produces a lower average mean squared error is the better model.
	At this point, if my model still has all significant coefficients but a low R-squared value, I will try adding a quadratic term to the regression and running it again using the same method of evaluation to the original two-stage least squares. 


## Results

## Replication Instructions
