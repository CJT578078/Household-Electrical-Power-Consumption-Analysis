## Household Electrical Power Consumption Analysis

**Cameron Tsai**

### Executive summary


**Project overview and goals:**

The goal of this project is to identify trends in household electrical power usage to provide actionable insights for energy providers and support proactive grid maintenance. To achieve this, we will develop predictive models capable of estimating power consumption at any given time, helping utilities optimize power distribution and improve infrastructure planning.

Specifically, we will train and evaluate three regression models on household electrical power consumption data, comparing their performance to determine the most effective approach. In addition, we will develop a statistical time series model to enhance our understanding of temporal consumption patterns and forecast future usage.

An in-depth analysis of the dataset will be conducted to extract meaningful insights and trends. Finally, we will outline potential future work and improvements that could build upon the findings of this project.

#### Rationale

With the increasing demand on electrical grids, especially due to smart appliances and electric vehicles, efficient energy consumption forecasting has become critical. Accurate predictions allow energy providers to reduce waste, prevent blackouts, and plan for maintenance more effectively. This project supports these efforts by analyzing real household power usage and building models that capture its underlying patterns.


#### Research Question

Can we accurately predict household active power consumption based on time-related features (e.g., day of the week, time of day) and other variables such as voltage, current, and sub-metered energy use?

#### Data Sources

**Dataset**: https://archive.ics.uci.edu/dataset/235/individual+household+electric+power+consumption 

This archive contains 2075259 measurements gathered in a house located in Sceaux (7km of Paris, France) between December 2006 and November 2010 (47 months).

**Cleaning and Preprocessing** 

The date and time are merged together to represent a specific moment that is inclusive of both date and time. Several columns of data which should be numerical were not and were thus converted. Redundant columns are dropped as well as any rows missing data. Data that sticks out too much, known as outliers, is removed to prevent it from skewing our models that may be sensitive to such things. 

New features are created to extract more information from the dataset. A training and holdout(test) set is created to be used to evaluate the predictive models. 

#### Methodology

Models are trained using the training set and validated with the test set. Pipeline objects were created to make implementation cleaner. GridSearchCV is also used to evaluate models using their mean squared error while also tuning their hyperparameters to maximize their mean squared error. 

Three regression models were trained. 

*Linear Regression Model:* A pipeline object is created to encode some columns that are numbered in a way that might skew the data, scale the data, and create a linear regresssion model. Linear regression was chosen as a baseline model used to understand linear relationships between input features (e.g., voltage, current, sub-metered usage) and the target variable Global_active_power. Its simplicity provides a benchmark for comparison.

*Ridge Regression Model:* A pipeline object is created to encode some columns that are numbered in a way that might skew the data, scale the data, and create a ridge regresssion model. GridSearchCV is used to hypertune the parameter alpha, with the best parameter being 0.1 out of the options [0.1, 0.5, 1, 3, 5]. Ridge regression is an extension of linear regression with L2 regularization. Ridge regression helps prevent overfitting and handles multicollinearity between features, especially useful for high-dimensional or correlated data.

*RandomForestRegressor Model:* A pipeline object is created to encode some columns that are numbered in a way that might skew the data and create a RandomForestRegressor model. GridSearchCV is used to hypertune the parameters n_estimators and max_depth, with the best being 100 and 10 respectively out of the options [50, 75, 100] and [5, 7, 10]. RandomForestRegressor is an ensemble model that uses multiple decision trees to capture non-linear relationships and feature interactions. It typically performs well on structured data and is robust to noise and overfitting.

Model performances are evaluated by comparing the root mean squared error of the training set vs the root mean squared error of the test set. 

*Auto Arima Model:*  The data which is sampled every minute is resampled to every day. This is done to lower the requirements of available memory and training time. Boxcox is used to transform the data to have a normal distribution. The Augmented Dickey-Fuller test is used to evalute if our data is stationary. Auto ARIMA picks the best parameters p, d, and q based on minimizing information criteria such as AIC. A seasonal period of 7 is chosen to represent the days in the week. Using the model, we are able to forecast global_active_power used at certain time. After converting the data back to how it was before applying Boxcox, we evalute the model by evaluating for far the root mean squared error is from the mean of the target variable. 

Graphs are made to display the ARIMA Forcast vs what the actual results are and to display the residuals (the difference between the original values and the forecasted values). 

Exploratory Data Analysis (EDA) is performed on the data to find various insights such as average global power used per hour over the course of a day and the average daily energy usage by day of the week. Finally, a graph breaking down the monthly average energy usage per sub-metering is shown.


#### Results

The best predictive model ended up being our baseline model, the Linear Regression model. It only outperformed the ridge regression model by a slight margin. Linear Regression Test RMSE: 0.040222263499 as opposed to  Ridge Regression Test RMSE: 0.0402222636758. The RandomForestRegressor model unfortunately may suffer from some overfitting as seen with its training set outperforming its test set (RandomForestReg Train RMSE: 0.034496326424 vs RandomForestReg Test RMSE: 0.0346498793184).

The Auto ARIMA model seemed to get caught at the average as most of the forecast formed a straight line through the actual Global_Active_Power data. 

Observing the data, it seems that electrical usage has a minor peak from 7 AM to 10 AM, with a major peak from 6 PM to 9 PM. Intuitively, this makes sense as these represent times outside the typical 9 AM - 5 PM work schedule. Average daily usage peaks on the Saturday with Sunday following closely after. 

#### Next steps

The current dataset only represents a single household over a few years. To improve the robustness and applicability of the models, incorporate data from multiple households with diverse usage patterns, geographies, and household sizes.

Including more features such as weather, can have an improved effect on the predictive capabilites of our models. Other features like holidays when families would stay at home or conversely go out on a trip, can also prove to be effective at improving predictive capabilities. 

Additional models could be tested and compared to further improve performance and interpretability. Gradient Boosting models for improved accuracy and better handling of non-linearity. Neural networks, particularly LSTM or GRU models, which are well-suited for capturing complex temporal dependencies in time series data.

#### Outline of project

- [Link to notebook](https://github.com/CJT578078/Household-Electrical-Power-Consumption-Analysis/blob/main/Capstone_Analysis.ipynb)
- [Link to dataset]()

##### Contact and Further Information
Cameron Tsai
ctsai081@ucr.edu 
