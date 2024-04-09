# NCAA Men's D1 Basketball Win Percentage Prediction Model

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning/Preparation](#data-cleaningpreparation)
- [Data Analysis](#data-analysis)
- [Results/Findings](#resultsfindings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)

### Project Overview

This project, initiated in January 2022, aims to establish the most influential elements for winning basketball games in Men's D1 NCAA Basketball. By creating a predictive model for win percentage, the objective is to understand which factors contribute most significantly to a team's success. The motivation behind this project is to provide insights that can aid basketball teams in strategizing effectively to enhance their chances of winning. By identifying the key predictors of success, teams can make informed decisions regarding training regimens, game tactics, player recruitment, and other aspects crucial to achieving favorable outcomes.

### Data Sources

This project utilizes [The College Basketball Dataset](https://www.kaggle.com/datasets/andrewsundberg/college-basketball-dataset) created by Andrew Sundberg. The dataset encompasses data from the 2013 to 2019 Division 1 men's college basketball seasons. It comprises 24 variables, ranging from the conference the school participates in, to more advanced statistics such as adjusted offensive efficiency.

### Tools

#### Programming Language
- **[R](https://www.r-project.org/about.html)**:
  - Data preprocessing
  - Exploratory Data Analysis (EDA)
  - Statistical analysis
  - Data visualization
  - Machine learning model development
  - Reporting and documentation using R Markdown

#### Libraries and Packages

| Package       | Uses                                                                          |
|---------------|-------------------------------------------------------------------------------|
| [car](https://cran.r-project.org/web/packages/car/index.html)         | Data manipulation and analysis                                               |
| [tidyverse](https://cran.r-project.org/web/packages/tidyverse/index.html)       | Data visualization and plot creation                                         |

### Data Cleaning/Preparation

In the initial data preparation phase, the following tasks were executed:
1. Added a win percentage variable to the data set, calculated by dividing games played by games won.
2. Removed unessential and redundant variables (team name, games played, games won, power rating, postseason elimination round, NCAA tournament seed, and year).
3. Split the data for cross-validation, randomly assigning 2000 observations to the training set and 455 observations to the testing set.

```r
#Randomized the rows of the data set
set.seed(12345)
rows = sample(nrow(cbb))
cbb_shuffled = cbb[rows,]

#Split the data into training and test sets for cross-validation.
cbb_train = cbb_shuffled[1:2000,]
cbb_test = cbb_shuffled[2001:2455,]
```
### Data Analysis

<img width="576" alt="Screenshot 2024-04-09 at 2 16 22 PM" src="https://github.com/austincicale/NCAA_BBall_WinPred/assets/77798880/bbfbc32b-f76d-4a90-97d3-3b9af7cfd836">

<img width="572" alt="Screenshot 2024-04-09 at 2 15 31 PM" src="https://github.com/austincicale/NCAA_BBall_WinPred/assets/77798880/e9d9331a-5014-496d-a779-c050f5969b9d">
