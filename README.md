# Project 1 - Machine Learning
This is the template for the first project for ECON1680: Machine Learning, Text Analysis, and Economics. This project is meant to be an economic application of machine learning. Over the course of the project, you should fill in the following sections of the Readme.md file below with your research question and motivation, your data sources, your methods, your results, and instructions for replciation. 

## Research Question  
How strong of a regressor is housing availability, as measured by numbered of housing units per perosn, for both cost of living and income in metro areas?

## Data  
American Community Survey:  
 Dataset of total population in each metro area from 2010-2019
 Dataset of total housing units, broken up by occupied and vacant in each metro area from 2010-2019
  
FRED:  
  Dataset of Median Real Wages Adjusted for Cost of Living by metro area and date  
  Dataset of building permits for 1 unit housing developments and the total number of building permits by metro area and date   
  Dataset of Housing Price Index by metro area and date  
  Dataset of Regional Purchasing Parity by metro area and date  
  
## Method
I started by plotting a correlation matrix (Image 1) of all the variables in the model. This was primarily a test of my instrumental variables to ensure that they were correlated with their respective regressors, but not the response variable. They all were, although the Prev_HPI instrument had a slightly high correlation of 0.6 with real wages, so I was wary of HPI as useful regressor.
	Next, I continued with a simple OLS regression without the instrumental variables. The regressors all proved significant at the 95% confidence level, but there were strong multicollinearity problems, so I tried a shrinkage approach. 
	Next, I ran a Ridge regression with the same variables and ran into the same problem. In fact, the coefficients for each regression were nearly identical. In order to really get rid of the multicollinearity problems, the next approach I tried was Principal Component Analysis.
	Originally, I ran an OLS regression with two principal components and the year dummy variables, but this still gave multicollinearity issues. When I included the year in the computation of the principal components, that actually got rid of the multicollinearity issues. This however resulted in a relatively low R-squared of just 0.61, which meant I was still losing too much variance through PCA. When I tried using three principal components, the third one had a very large p-value that was greater than 0.5, so it wasn’t significant in the regression. 
	To help deal with this, I introduced the instrumental variables and ran a two-stage least squares regression. This ultimately gave me very good results with an R-squared of 0.972, meaning I was successfully catching most of the variance in the model, and all the regressors were significant at the 95% confidence level. However, the regression didn’t work with the year dummy variables and gave several NaN results in the output, so to get around this I rescaled the year to go from 0 to 10. Because of this rescale, I would not use this model to predict real wages in the future, but for the purposes of showing causality, I think it’s more important to make sure the instrumental variables are included than the year fixed effects.
	Still, the coefficient of HPI confused me here, especially since I was suspicious of it before from the correlation matrix. Based on my starting assumptions, a higher HPI would indicate a scarcer housing market and therefore lower real wages. I think this suggests Prev_HPI is too correlated with real wages to catch reverse causality and I should exclude both it and HPI entirely. This gave me my final results.



## Results
From the final regression, of the four feature variables I was interested in studying, I excluded HPI, while Percent of Single Unit Permits (Permit_Ratio), Vacancy Rate (Vac_Rate), and Population per Housing Unit (Pop_per_unit) all had negative effects.  
	The coefficients of Permit_Ratio and Pop_per_unit are both negative, which indicates that as the percentage of permits for single unit housing, and the number of people per housing unit increase, real wages decrease. These are both statistically significant regressors, and behave in the way that I expected.
	Vac_Rate, surprisingly, had a statistically significant negative coefficient as well. I expected a high vacancy rate to suggest that the housing demand was low and therefore wages would be higher. However, this would seem to suggest instead that a high vacancy rate is an effect of low wages in a metro area. 


## Replication Instructions
