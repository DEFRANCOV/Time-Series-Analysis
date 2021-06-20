# TIME SERIES HOMEWORK ASSIGNMENT

## TIME SERIES FORECASTING

* The accompanying code file is Val_time_series_analysis.ipynb.   You can look at it [here](Val_time_series_analysis.ipynb)

- Without the use of any mathematical tools, and based upon a pure visual observation, we can see the ¥ strengthened against the $C in the early to mid 1990s, with a modest weakening from 1996 to 2008, and then a “flattish” trend from 2008 to present.

### Hodrick-Prescott Filter
- Imported from the statsmodel library, we utilize the Hodrick-Prescott (HP) filter to separate the time series into two components:  trend and noise.   
- The next step is to create a new dataframe, with the creative name of “cad_jpy_df2”.   cad_jpy_df2 has three columns (and an index).   It keeps the price column from the first dataframe and adds the calculated (from previous step) trend and noise columns.
- Analyzing the time series from 2015 to present, using cad_jpy_df2 dataframe, and observing from the plot, we see that:
  - From 2015 to late 2016, the ¥ strengthened materially.  
  - From late 2016 to 2018, it weakened with some strength,  
  - From 2018 to present, it's modestly increased.

### ARMA Model
- We import the ARMA model from statsmodel library.
- To ensure stationarity, we create another dataframe called “returns”.   returns employs the pct_change function on the Price column of the first dataframe.  
- We then must remove any NAs and/or infinity calculations.
- As per instructions, we employ the ARMA model with an order of (2,1).   Looking at the results table, we see that the p-value for the autoregressive-lag of 2 is not significant (p-value > 0.05), so this parameter doesn’t really add value.  In fact, when running the ARMA model with an order of (1,1), the AIC and BIC are slightly lower indicating a little more predictive power. 

### ARIMA Model
- We import the ARIMA model from you guessed it, the statsmodel library.
- We employ the ARIMA model on the cad_jpy_df2 dataframe, with the column “Price”.
- A per instructions, we use an order of (5,1,1).   Looking at the results table, we see that none of the parameters are significant (p-values are all > 0.05), so the model isn’t that great.  
- Despite the results, if we use the model to forecast a 5-day return of the ¥, we should expect a very modest strengthening.


### GARCH
- We import the GARCH model from arch library.
- A per instructions, we use an order of (2,1).   Looking at the results table, we see that the p-value for the autoregressive-lag of 2 is not significant, so this parameter doesn’t really add value.  
- The last bit of code creates a 5-day forecast of volatility starting from the last day of the time series.   We should expect the volatility to moderately expand in the next five days.



## REGRESSION ANALYSIS

- The accompanying code file is Val_regression_analysis.ipynb. You can look at it [here](Val_regression_analysis.ipynb)
- We import LinearRegression from sklearn library.
- Within the original dataframe (from Time Series), we create a column called “Return”, which employs the pct_change function on the Price column and removes NAs and Infinities.    
- We then create another column, “Lagged_Return” which employs the shift function on “Return”.   In this case, the shift function takes the prior value of Return.  Ultimately, we will attempt to see if there is any meaningful relationship with Return and Lagged_Return.
- Now we must split the data (along rows).   The train data is from the beginning until the end of 2017.  (Note that the creation of Lagged_Return creates a “NA” at time = 1, so we must remove this).  The test data is from the beginning of 2018 until present.  
- We then split the two newly created dataframes into 2 subsequent dataframes each (so four in total), by columns.  The column Return is used for X_train and X_test.   The column “Lagged_Return” is used for y_train and y_test.  
- We employ the LinearRegression function to fit X_train and y_train.   
- Finally, we use the fitted data model to predict the values for y_test.  
- Plotting the first twenty predicted values against the actual twenty values, we see that there is room for improvement in the model.
- Finally (for real this time), we try the fitted data model on y_train values.   We see that via the RMSE, the model is less effective for the train data (in-sample) vs. test data (out of sample).   While this is not normal, it can happen.  With some knowledge of financial history, there were significant events that impacted either the ¥ or $C within the train timeframe, and not within the test timeframe.
