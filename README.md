# TempTrack - Temperature Forecasting Model

![cover](https://github.com/GabrielAlvesDS/CarPrice_Pro/blob/main/images/carprice%20pro_cut.jpg)

<br>

## Objective
This project aims to deepen knowledge in time series analysis and explore new algorithms. Two models were developed to predict hourly temperature over a year using Prophet and XGBoost. The goal is to assess the usability of each model and identify which one performs better.

The choice of a one-year period with hourly predictions was made to allow the models to capture seasonal variations throughout the year.


## Data Set
The data used in this project were extracted from the complete historical records of INMET for the region of Seropédica, in the state of Rio de Janeiro, Brazil, where EMBRAPA is located. This data set includes various climate measurements recorded hourly since 2002. For this project, we exclusively used the variable 'TEMPERATURA DO AR - BULBO SECO, HORÁRIA (°C)', which provided a robust base for training the models. Below is a sample of the data used:

## Data Preprocessing
Common Preprocessing for Both Models
- **Standardizing Date and Time Format:** Ensured a consistent format for the date and time feature.
- **Handling Missing Values:** Identified intervals with missing temperature values and imputed them with the mean value. (Other approaches were considered but showed no significant impact on model performance, thus the simplest method with minimal processing load was chosen.)
- **Correcting Erroneous Values:** Detected instances where the temperature was incorrectly recorded as -9999. Applied the same imputation method used for missing values.

### Prophet Model
Feature Requirements:
Only required the date and target variable (temperature). No additional features were created.
### XGBoost Model
- **Feature Engineering:** Created new features based on the date: hour, dayofweek, quarter, month, year, dayofyear, dayofmonth, weekofyear, and season.
- **Data Analysis:** Conducted various analyses to better understand the data and identify key points for model development:
- **Temperature Distribution:** The air temperature distribution approximates a normal distribution (see image below).
  ![img1](https://github.com/GabrielAlvesDS/TempTrack/blob/main/img/Distribution%20of%20Air%20Temperature.png)

<br>

- **Temperature Range:** 50% of recorded temperatures fall within a 5°C range, from 21°C to 26°C. Note that this analysis was impacted by the imputed mean values (see image below).
 ![img2](https://github.com/GabrielAlvesDS/TempTrack/blob/main/img/Boxplot%20of%20Air%20Temperature.png)

<br>

- **Missing Data Impact:** Visualized the distribution by year to identify four significant gaps in 2002, 2004, 2010, and 2019 (see image below).
 ![img3](https://github.com/GabrielAlvesDS/TempTrack/blob/main/img/Missing%20values.png)

<br>

- **Seasonal Influence:** Identified the impact of seasons on temperature variation. The highest median temperature was in autumn and the lowest in spring, while the highest and lowest recorded temperatures were in summer and winter, respectively (see image below).
 ![img4](https://github.com/GabrielAlvesDS/TempTrack/blob/main/img/Season.png)

<br>

- **Daily Temperature Variation:** Analyzed temperature variation throughout the day (see image below).
 ![img5](https://github.com/GabrielAlvesDS/TempTrack/blob/main/img/Hour.png)

<br>

- **Data Preparation:** Applied MinMaxScaler to all features, and a logarithmic transformation to the target variable to stabilize variance and normalize distribution.

## Model training
The data was divided into training and testing sets, with the training set containing data from 2002 to 2022 and the testing set including only the last year, 2023.

### Prophet Model


### XGBoost Model
Initially, a basic model fit was performed to establish a baseline. Cross-validation was then applied to evaluate the model's performance using metrics such as MAE, MAPE, RMSE, and MSE.

To enhance the model, a hyperparameter tuning step was carried out using the Optuna library, which helps find the optimal values for parameters like learning rate, maximum tree depth, and the number of estimators.

With the best hyperparameters identified, the final model was trained. After training, predictions were made on the test set, and error metrics were calculated to assess the model's performance, focusing on its accuracy in predicting hourly temperatures.

This process allowed for fine-tuning the model, maximizing its accuracy and generalization ability to new data.


## Model Evaluation

### Prophet
The performance of the Prophet model was assessed using the following metrics:

- Mean Squared Error (MSE): 10.58
- Root Mean Squared Error (RMSE): 3.25
- Mean Absolute Error (MAE): 2.47
- Mean Absolute Percentage Error (MAPE): 9.96%

The Prophet model shows an RMSE of 3.25°C and an MAE of 2.47°C, indicating that the predictions deviate from actual temperatures by these amounts on average. Additionally, the MAPE of 9.96% suggests that the average prediction error is about 9.96% relative to the actual values. The Prophet model was notably easier to implement, as it required no additional feature creation and achieved these results without the need for hyperparameter tuning.

### XGBoost
The performance of the final XGBoost model was assessed using the following metrics:

- Mean Squared Error (MSE): 15.44
- Root Mean Squared Error (RMSE): 3.93
- Mean Absolute Error (MAE): 3.00
- Mean Absolute Percentage Error (MAPE): 12.34%

The XGBoost model achieved an MSE of 15.44, RMSE of 3.93, MAE of 3.00, and MAPE of 12.34%. The performance metrics for XGBoost were approximately 17.65% to 34.16% worse compared to Prophet.

### Performance Comparison:

- MSE: XGBoost's MSE is approximately 45.8% higher than Prophet's MSE.
- RMSE: XGBoost's RMSE is about 20.9% higher than Prophet's RMSE.
- MAE: XGBoost's MAE is roughly 21.5% higher than Prophet's MAE.
- MAPE: XGBoost's MAPE is about 23.8% higher than Prophet's MAPE.

It is important to note that the XGBoost model required an extensive phase for hyperparameter tuning, which consumed significant time. In contrast, the Prophet model's simplicity and effectiveness in this case suggest that it might be a more suitable choice for temperature forecasting in similar scenarios.



