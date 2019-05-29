# Stock-Price-Prediction---Multivariate-Multistep-LSTM-forecasting

In this repository the stock price values of the 21 companies of NIFTY50 is taken as input and are then used to predict the next 4 day stock prices of any particular stock.

## CONCEPT

The basic idea in taking 21 stocks is that the stock changes of any stock is not just a cause of the company's activity but it is majorly determined by other fators like:
   * Performance of fellow companies in the same sector.
   * Performance of the companies that provide either an input to this sector or are direct consumer of the services provided by this sector.
   * The amount of trade occuring in the company that are competitor to target company.
   
   Thus as it can derived from here that the stock changes in a company are not sole results of the company's deeds but it actually a contribution of the work by several stock companies that are there in NIFTY.

## DATASET

The dataset collected is for the companies in NIFTY50. The companies are selected from each sector in a way that the basic movement of the sector can be captured. There are several companies in each sector of NIFTY but for the project I have selected 21 companies. 

The companies are namely ASIANPAINT, BHARTIARTL, CIPLA, HCLTECH, HDFCBANK, HINDUNILVR, ICICIBANK, INDUSINDBK, INFY, ITC, KOTAKBANK, LT, M&M, MARUTI, NTPC, ONGC, POWERGRID, RELIANCE, SBIN, SUNPHARMA, TATAMOTORS, TCS, TECHM, VEDL.

The stock prices for the companies are collected for past 9 years (2010-2018). The closing price is then selected and is used in further steps.

## Requirements
    Version of Pyhton: python 3.5
	

## Model Steps
### Step1: Data gathering.

	*   The stocks data for each company is downloaded from nseindia.com .
	*   The data is downloaded from 01-01-2010 till 01-06-2018.
	*   The closing price from the data is selected and stored in a csv file.
  
  The code is present in *Prepare_dataframe.ipynb*.
  
  
### Step2: Residual estimation.
    The stock price time series is decomposed into its components that are:
    *  Trend
    *  Seasonality
    *  Residual
    The trend is the general motion of the series after removing the minute details or the fluctuations in the market.

    Seasonality correspond to the changes that occur over a duration of time and repeat over time having the same periodicity. The seasonal components are used to denote the fluctuations that occur in the series due to external events and have less dependence on the series alone.

    Residual is the component that is not explained by the trend or seasonality and is most difficult and import to keep track of.
    
   The residual component is saved into *scaled_residual_df.csv* with its scaling measures stored in *scaled_mean_and_std.csv*.
   
   The code is present in *Generate_residual_components.ipynb*.


|![residual][res]|
|:---:|
|Original series along with Trend, Seasonality and Residual components of the original series|


### Step3: Data Preparation
    The dataset is then prepared to use previous 7 days (lags) stock price of all 21 time series to predict the next 4 days (predictions) of one of the stocks.
    The method for preparing the dataset was learnt and adopted from the website machinelearningmastery.com run by Jason Brownlee sir.
    The choice of taking lag of 7 days and other features like number of neurons and several others were decided after comparing the models accuracy when the parameters are set to those values.
The factors that were evaluated for better RMSE on prediction as well as lower losses were:

    *  PARAMETER : VALUES CHECKED FOR THAT PARAMETER
    *  Number of Epochs : 1, 5, 10
    *  Lag to be considered for creation of the dataset : 3, 5, 7, 10, 15, 25
    *  Number of neurons in layer 1 : 3, 5, 7, 8, 12,16
    *  Number of neurons in layer 2 : 3, 5, 7
    *  The optimizer algorithm to be used : sgd, adam, rmsprop
    *  Batch size : 1
    *  Dropout ratio in layer1 : 0.2
    *  Dropout ratio in layer2 : 0.2

### Step4: Results

    The prediction from the LSTM were scaled back so that they can be compared with the residuals that the LSTM network was predicting.
    
## Actual residuals and the predictions

|![actuals][act]|
|:---:|
|Actual residual values that were expected|
||
|![predictions][pred]|
|Predicted values of the 2nd day (timestep+2) by the LSTM network|

## ERRORS

|![absolute][abs]|
|:---:|
|Error between the actual value and the one predicted by the model,i.e., (Actual_value - Predicted_value)|
||
|![mape][ma]|
|Mean Absolute Percentage Error|

The MAPE error goes high at times due to the value of actual being very low, close to 0 and a large value of difference between the actual and the predicted values.

## Output
The output of the LSTM prediction for a duration of 200 days is plotted for the company L&T.

|![output][out]|
|:---:|
|Output for the prediction of the stock price|

## References
    Dataset from www.nseindia.com.
    Information about companies and NIFTY 50 from www.niftyindices.com.
    LSTM tutorials by Jason Brownlee at machinelearningmastery.com.

<!--Images-->
[act]:misc/images/actual.png "act"
[pred]:misc/images/prediction.png "pred"
[abs]:misc/images/absolute.png "abs"
[ma]:misc/images/MAPE.png "ma"
[out]:misc/images/output.png "out"
[res]:misc/images/residual.png "res"
