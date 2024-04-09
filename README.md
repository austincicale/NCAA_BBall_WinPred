# NCAA Men's D1 Basketball Win Percentage Prediction Model

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning/Preparation](#data-cleaningpreparation)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
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
| [dplyr](https://cran.r-project.org/web/packages/dplyr/index.html)         | Data manipulation and analysis                                               |
| [ggplot2](https://cran.r-project.org/web/packages/ggplot2/index.html)       | Data visualization and plot creation                                         |

### Data Cleaning/Preparation

In the initial data preparation phase, the following tasks were executed:
1. Removed all NA values and individuals not in the labor force, reducing the data from 3,252,599 observations to 261,202 observations.
2. Removed unnecessary and redundant variables.
3. Manipulated certain variables to simplify the data, such as merging similar education levels rather than having levels for each grade.
4. Converted the data types for certain variables to better reflect the data and enhance analysis.


