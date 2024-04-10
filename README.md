# NCAA Men's D1 Basketball Win Percentage Prediction Model

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning/Preparation](#data-cleaningpreparation)
- [Data Analysis](#data-analysis)
- [Results/Findings](#resultsfindings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)

### Project Overview

This project, initiated in January 2022, aims to establish the most influential elements for winning basketball games in Men's D1 NCAA Basketball. By creating a predictive model for win percentage, the objective is to understand which factors contribute most significantly to a team's success. The motivation behind this project is to provide insights that can aid basketball teams in strategizing effectively to enhance their chances of winning. By identifying the key predictors of success, teams can make informed decisions regarding training regimens, game tactics, player recruitment, and other aspects crucial to achieving favorable outcomes.

### Data Sources

This project utilizes [The College Basketball Dataset](https://www.kaggle.com/datasets/andrewsundberg/college-basketball-dataset) created by Andrew Sundberg. The dataset encompasses data from the 2013 to 2019 Division 1 men's college basketball seasons. It comprises 24 variables, ranging from the conference the school participates in, to more advanced statistics such as adjusted offensive efficiency.

### Tools

#### Programming Language
- **[R](https://www.r-project.org/about.html)**:
  - Data preprocessing
  - Statistical analysis
  - Predictive linear modeling
  - Linear model assumption analysis
  - Residual analysis
  - Model transformation
  - Data visualization
  

#### Libraries and Packages

| Package       | Uses                                                                          |
|---------------|-------------------------------------------------------------------------------|
| [car](https://cran.r-project.org/web/packages/car/index.html)         | Graphical regression diagnostics and ANOVA testing                                               |
| [tidyverse](https://cran.r-project.org/web/packages/tidyverse/index.html)       | Data manipulation, exploration, and visualization                                         |

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

Using all variables from the filtered dataset, three methods were employed to construct a model predicting win percentage: backward, forward, and stepwise selection. These methods created identical linear models featuring thirteen predictor variables. An analysis of the five largest absolute residuals of the model was conducted to assess the degree of their influence and determine if any data points should be removed. Utilizing Cook's distance with a threshold of 0.01, there appeared to be no significant concern for influence in the model, and consequently, no data points were excluded.

```r
# Create a model predicting win percentage using the training data and the backward selection method
full = lm(win_pct~., data=cbb_train)
mse = (summary(full)$sigma)^2
cbb_mod1 = step(full, scale = mse, trace = FALSE)
```
```r
# Calculate Cook's distance for 5 largest absolute residuals to estimate the influence of these points
indices = sort(abs(cbb_mod1$resid), decreasing = TRUE, index.return=TRUE)$ix[1:5]
head(sort(cooks.distance(cbb_mod1)[indices], decreasing=TRUE))
```
#### Cook's Distance for the 5 Largest Absolute Residuals
| Observation | Cook's Distance |
|-------------|-----------------|
| 697         | 0.008070677     |
| 1352        | 0.007535711     |
| 249         | 0.006565230     |
| 1561        | 0.004088719     |
| 949         | 0.003614799     |

Further experimentation was undertaken with the existing model to explore opportunities for enhancing predictions of win percentage. Various transformations were applied to both the response and predictor variables in an attempt to improve the variance explained by the model's predictors (adjusted r-squared). Despite these efforts, none of the variable transformations resulted in a notable increase in the adjusted r-squared. However, certain variable interactions exhibited promise for enhancing the model:
  - Adjusted Offensive Efficiency with Turnover Percentage Committed
  - Adjusted Offensive Efficiency with Effective Field Goal Percentage Shot
  - Turnover Percentage Committed with Adjusted Tempo
  - Adjusted Defensive Efficiency with Effective Field Goal Percentage Allowed
  - Adjusted Defensive Efficiency with Free Throw Rate

These five variable interactions were integrated into the original model, resulting in an increased adjusted r-squared. Although this addition appeared to enhance the model's predictive ability in terms of explained variance by the predictors, uncertainty persisted regarding the necessity of all five interactions. Backward selection methodology was employed to construct the most efficient model, incorporating the five variable interactions alongside all variables from the original model as potential predictors of winning percentage. Notably, this selection approach retained all predictors from the original model and incorporated all five variable interactions.

```r
# Develop a model using backward selection methodology, taking into account the five potential variable interactions
full_2 = lm(win_pct ~ ADJOE + EFG_D + CONF + ADJDE + EFG_O + TOR + ORB + ADJ_T + FTR + FTRD + TORD + DRB + `3P_D` + ADJOE*TORD + EFG_O*ADJOE + EFG_D*ADJDE + ADJ_T*TORD + FTR*ADJDE, data=cbb_train)
mse_2 = (summary(full_2)$sigma)^2
cbb_final_mod = step(full_2, scale = mse_2, trace = FALSE)
```

To validate that the new model incorporating interaction terms represents an improvement over the original model, ANOVA testing was conducted to compare the two models. The test yielded an extremely small p-value of 1.247e-08, indicating that the slope relating win percentage to the interaction terms is non-zero for at least one of the variable interactions. This is the expected result, and it supports the continuation of the analysis using this new, finalized model. 

After verifying the model satisfies the assumptions of linear modeling, the last step is to see how the finalized model performs when it comes to predicting the winning percentage for a new set of data. A fairly small shrinkage value was computed, indicating the model similarly predicts win percentage for both the training and testing data. A graphical representation of the model’s ability to predict win percentage for the testing data is shown in the figure below. If the model predicts efficiently, the testing data points data should relatively follow the red curve. This appears to be the case, further supporting the model’s ability to accurately predict the winning percentage for out-of-sample data.

```r
# Produce a plot displaying the relationship between actual and predicted win percentage
yhat = predict(cbb_final_mod, newdata = cbb_test)
win.pct.test = cbb_test$win_pct
plot(win.pct.test, yhat, ylab = "Predicted Win Percentage", xlab = "Actual Win Percentage")
abline(0,1, col = 'red')
```
#### Actual Win Percentage vs Predicted Win Percentage

<img width="572" alt="Screenshot 2024-04-09 at 2 15 31 PM" src="https://github.com/austincicale/NCAA_BBall_WinPred/assets/77798880/e9d9331a-5014-496d-a779-c050f5969b9d">

### Results/Findings

The coefficient of each variable signifies its impact on predicting winning percentage. Furthermore, the p-values indicate the statistical significance of the variables within the model. The conclusive results of the model are outlined in the following table: 

#### Final Model Predictor Variable Summary
<img width="576" alt="Screenshot 2024-04-09 at 2 16 22 PM" src="https://github.com/austincicale/NCAA_BBall_WinPred/assets/77798880/bbfbc32b-f76d-4a90-97d3-3b9af7cfd836">

*Note. Categorical variable Conference (CONF) not included for spacing constraints. Refer to variable descriptions [here](https://www.kaggle.com/datasets/andrewsundberg/college-basketball-dataset).*

### Recommendations

From a coaching perspective, the following actions are recommended:
- Prioritize the variables included in the finalized model when developing offensive and defensive strategies that focus on optimizing these predictors to their benefit
- When recruiting, look for players that excel in areas positively influencing winning percentage according to the model.

For future research, the following actions are recommended:
- Explore additional data sources, incorporating variables not considered in this model.
- Experiment with advanced modeling techniques beyond traditional regression analysis. Consider machine learning algorithms to uncover complex relationships and nonlinearities within the data.

### Limitations

##### 1. Limited Number of Variables
  - The dataset used in this project was limited to 24 variables, which may have restricted the comprehensiveness of the analysis and potentially overlooked important predictors of winning percentage not included in the data.

##### 2. Focus on Regression Analysis
  - The analysis primarily relied on traditional regression analysis techniques to model the relationship between predictor variables and winning percentage, which may not fully capture the complexity of interactions and nonlinearities in the data.

