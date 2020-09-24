# Data Science Salary Estimator: Project Overview
*Disclaimer: This project is **strongly** based off of Ken Jee's tutorial. Much of the code used is his with minor tweaks by me to better incorporate the data I pulled.*

- Created a data science salary estimation tool (MAE ~$25K) which could help data scientists negotiate their income when they are offered a job.
- Scraped 1000 job descriptions from glassdoor using Python and Selenium.
- Engineered features from the job description text.
- Optimized Linear, Lasso, and Random Forest regressors using GridSearchCV to find the best model to productionize.
- Productionized the model by building a client facing API using Flask.

## Project Tutorial Resource
Again, this project is based on the tutorial by Ken Jee. The link to his video and GitHub are below. \
**YouTube Tutorial:** https://www.youtube.com/playlist?list=PL2zq7klxX5ASFejJj80ob9ZAnBHdz5O1t \
**GitHub Repo:** https://github.com/PlayingNumbers/ds_salary_proj

## Code and Resources Used
**Python Version:** 3.7\
**Packages:** pandas, numpy, sklearn, matplotlib, seaborn, selenium, flask, json, pickle\
**Web Requirements:** `pip install -r requirements.txt` \
**Glassdoor Scraper:** https://github.com/arapfaik/scraping-glassdoor-selenium \
**Flask Article:** https://towardsdatascience.com/productionize-a-machine-learning-model-with-flask-and-heroku-8201260503d2 \

## Web Scraping
By using the glassdoor scraper linked above (and making my own modifications) I scraped 1000 job postings from glassdoor.com. The following data fields were collected:
- Job Title
- Salary Estimate
- Job Description
- Rating
- Company
- Location
- Company Headquarters *note: (returned all -1 so ignored)*
- Company Size
- Company Founded Date
- Type of Ownership
- Industry
- Sector
- Revenue
- Competitors *note: (returned all -1 so ignored)*

## Data Cleaning
I cleaned the data after scraping in order to use it in my models. Most of the parsing followed the tutorial with a few tweaks. Listed below are the changes that were made to the data:
- Parsed numeric data out of salary
- Made indicator column for if salary was quoted hourly
- Removed rows located in England 
- Parsed rating out of company text
- Made a new column for company state
- Transformed founded date into age of company
- Made columns for if different skills were listed in the job description:
  - Python
  - R
  - Excel
  - AWS
  - Spark
  - SQL
  - Tableau
- Created columns for simplified job title, seniority, and description length

## EDA
Explored data distributions, values counts, and various pivots. Below are some examples of my findings. \
![alt text](https://github.com/nkrajew/ds_salary_proj/blob/master/images/pivot.PNG "Pivot Table")
![alt text](https://github.com/nkrajew/ds_salary_proj/blob/master/images/corr_2.PNG "Correlations")
![alt text](https://github.com/nkrajew/ds_salary_proj/blob/master/images/state_jobs_resize.PNG "Correlations")

## Model Building
#### Data Manipulation 
1. Transformed categorical variables into dummy variables. 
2. Split the data into train and test sets (test size = 20%).

#### Evaluation Metric
Following along with Ken Jee, I chose Mean Absolute Error to evaluate the models. The decision was made because MAE is easy to interpret. Furthermore, outliers are not that extreme for this type of prediciton model.

#### Models
Three different models were built:\
**Multiple Linear Regression** – Baseline model (built using both statsmodels and sklearn).
**Lasso Regression** – The sparse data from the many categorical variables suggest that a normalized regression (like lasso) would be effective.
**Random Forest** – Due to the sparsity in the data, Random Forest would be another good model to use. 

## Model Performance
The Random Forest model outperformed the other approaches, however, the difference in results was not as drastic for me as it was for Ken in his tutorial:

Random Forest : MAE = 24.98
Lasso Regression: MAE = 26.22
Linear Regression: MAE = 26.83

## Productionization
I built a Flask API endpoint that was hosted on a local webserver by following along with Ken Jee's and the TDS tutorial in the reference section above. The API endpoint takes in a request with a list of values from a job listing and returns an estimated salary.
