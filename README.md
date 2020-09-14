#### Introduction

For the second project of the [Metis Data Science Bootcamp](https://www.thisismetis.com/), I web scraped San Francisco [Craigslist car postings](https://sfbay.craigslist.org/d/cars-trucks/search/cta) and built a linear regression model. My model is used to predict how price is affected by a car's attributes such as age, odometer reading, and post location.

#### Individual Contributor

* Jacky Lu

#### Project Motivation

My target audience are consumers interested in buying or selling a car on Craigslist. Additionally, the model I built can be useful for car dealerships or market research companies interested in exploring the relationship between key car attributes and price.

The main question I want to address is:

- What are the main factors that contribute to price on Craigslist?

#### Project Submission Directory

Craigslist Cars Web Scraping Jupyter Notebook

* Contains functions and a web scraping workflow needed to scrape Craigslist car postings and save the information into a DataFrame
* Code is organized as follows:
  * 1) Imports
  * 2) Scraping a Craigslist Car Post
  * 3) Scraping Craigslist Results Page
  * 4) Building my DataFrame
  * 5) Saving the Data

Craigslist Cars Data Cleaning Jupyter Notebook

* Notebook to clean up data by adding, removing, and filling in columns
* Exploratory Data Analysis is also performed to see the relationship between different features and price
* Code is organized as follows:
  * 1) Imports
  * 2) Data Cleaning
  * 3) Exploratory Data Analysis

Craigslist Cars Regression Jupyter Notebook

- Notebook to build, validate/test, and interpret my regression model
- I optimize my model using LASSO to select the most impactful features for my model. I also cross validate and test my model. Finally I interpret my results.
- Code is organized as follows:
  - 1) Imports
  - 2) Checking Assumptions
  - 3) Exploratory Regression
  - 4) Cross-Validation and Testing
  - 5) Results

#### Data Science Analysis

1. Data Collection

   [Craigslist SF Bay Area car postings](https://sfbay.craigslist.org/d/cars-trucks/search/cta) were scraped on July 10, 11, and 12, 2020. In total, I scraped 7,443 car posts and obtained values for the following features:

   * Condition, Cylinders, Drive, Fuel, Odometer, Paint Color, Title Status, Transmission, Type, Year, Make, Model, Post Title, Price, Location, Posting Date, post URL, VIN, (vehicle identification number) and Size

2. Data Preparation

   Craigslist car postings were messy and incomplete. There is no standard for which attributes were listed. For example, location included multiple cities like Oakland/Berkeley. I used the Craigslist post URL instead to record a general location (like East Bay). Additionally, I dropped columns like make, model, paint color, etc. that would be hard to use as categorical variables in a regression model because they had many different values as well as many null values.

   For other columns, I imputed data. I filled in a row of data where fuel was missing by going to the actual post and recording the fuel attribute from the post title. For title status, I imputed null values as "missing".

   Finally, I made log transforms of my odometer, car age, and price features. The untransformed features were heavily skewed right. The residual plot looked more like a normal distribution when the values were transformed.

3. Exploratory Data Analysis

   How do my features compare to price? Are there any relationships I can spot before I build my model to help me select the right features?

   For my numeric features, I plotted scatter plots of each feature against price. I saw slight negative correlation between car age and price as well as odometer reading and price.

   For my categorical variables, I made box plots of each feature against price. Diesel cars had slightly higher prices. Additionally, posts made in San Francisco had slightly higher price than normal and posts made in the Peninsula had slightly lower prices. However, it was hard to see the magnitude of these correlations because there were many outliers with high prices.

4. Regression Model Building

   My first step was to split my data using an 80/20 train test split. Then, I standardized my numeric features using StandardScaler to convert the values into unitless z-scores. Additionally, I used dummy variables and dropped one categorical dummy column per feature to avoid the colinear dummy variable trap.

   Then I started my linear regression modeling. At first, my model started with 20 different features ranging from price, attribute number, post title length, title status, odometer reading, etc. Many features had high P values according to Statsmodel's summary regression table. This indicates a high probability the coefficients were made by chance assuming there was no relationship between the feature and price. I also ran LASSO regularization on the data to help me select which features were most important to the model. After iterating several times and removing features with a 0 coefficient using LASSO regularization and had high p-values, I finalized my model.

   My final model has 5 features: diesel fuel, peninsula location, san francisco location, car age (log), and odometer (log). Using ordinary least squares LinearRegression model, my model has an R^2 value of 0.437. This means that 0.437 of the variation in price can be explained by my model.

   Afterwards, I ran KFolds cross-validation (5 folds each) and validated my model using LinearRegression, Lasso, and Ridge models. All of them gave similar R^2 values around 0.44. Finally, I tested my model and plotted the residuals of my training vs test set. Surprisingly, my test R^2 was a little higher, around 0.486. The residuals were also normally distributed, which helps validate my model.

5. Conclusions

   After finalizing my model, I came to the following conclusions about what factors affect car prices on Craigslist the most and by how much.

- For every **10%** increase in car age by year, the price decreases by **6.24%**. In other words, a 10 year old car that's $10,000 would be $624 cheaper if it was an 11 year old car.
- For every **10%** increase in odometer mile reading, the price decreases by **0.33%**. In other words, a car with 10,000 miles that's $10,000 would be $33 cheaper if it had 11,000 miles.
- Diesel cars are **96.8%** more expensive
- Posts in the Peninsula Craigslist are cheaper by **8.5%**.
- Posts in the San Francisco Craigslist are more expensive by **13.3%**.

#### Design Decisions

- I transformed price, car age, and odometer readings with a log function to reduce skewness and have a residual plot that's more normal.
- I removed certain features like transmission (manual, automatic, or other) and title status (clean, salvage, parts only, etc.) because the large majority of cars were one value and because the p-values and Lasso coefficients were high and close to zero respectively.
- I did some feature engineering, I created a feature called attribute number to count the number of attributes collected per car post. I also created a feature to count the length of each post's title. I didn't end up using either of these features in my final model.

#### Additional Notes

Techniques

* Exploratory Data Analysis
* Web Scraping
  * HTML, HTTP requests
* Linear Regression
  * Ordinary Least Squares
  * Regularization (Ridge and Lasso)
  * KFold Cross Validation

Programs

* Jupyter Notebook
* BeautifulSoup
* Matplotlib
* Numpy
* Pandas
* Seaborn
* Statsmodels
* Scikit-learn
* Yellowbrick

Language

* Python