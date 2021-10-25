# Executive Summary


## Background information

In the wake of COVID and popularization of retail stock trading via Robinhood and the Gamestop/AMC frenzy, many people are beginning to trade on the stock market for themselves. 

One of the most popular methods for this new group of traders is a methodology called Day Trading, where positions are held short term, or intraday, utilizing what is called Technical Analysis to predict where changes in price will take place instead of fundamental analysis of the business itself.

**With this in mind, we at the Association for Retail Traders (ART) want to know if the most commonly recommended day trading indicators are effective in predicting stock price movements. We will be testing these indicators through a series of Time-Series Machine Learning Models and looking for the lowest error using Root Mean Squared Error.**


## Data Collection

We obtained our data via AlphaVantage’s free API. We were able to get 2 years’ worth of 1 minute data from the SPDR S&P500 ETF Trust, an ETF following the S&P500 index, for market, pre-market, and after hours. This was more data than we were expecting, but more data is never a bad thing. 

One thing that needed to be done immediately though is that our data was in the order from most recent to furthest back. When calculating moving averages and predicting the Time series data, we want this to be in order from earliest to most recent.

Additionally, there is very little fluid movement late after hours and very early pre-market so we wanted to filter that. There is some significant movement in these off-hour times, so we chose to keep the data from 30 minutes prior and after market hours, 9am – 430pm ET.


## Feature Engineering

The most commonly recommended indicators for use in Day Trading are:
- Volume Weighted Average Price (VWAP):
 - The volume-weighted average price (VWAP) is a trading benchmark used by traders that gives the average price a security has traded at throughout the day, based on both volume and price.
 - VWAP is important because it provides traders with insight into both the trend and value of a security.
 - VWAP is calculated by adding up the dollars traded for every transaction (price multiplied by the number of shares traded) and then dividing by the total shares traded.
- VWAP 1st, 2nd, and 3rd Standard Deviation:
 - Standard deviation of a rolling 12 period for VWAP
- 9 and 20 Exponential Moving Averages (EMA)
 - 2 exponential moving averages, 9 and 20 period, which weight recent terms heavier; think of decay.
- Crossing of the 9 and 20 EMAs
 - The crossing of the 9 EMA through the 20 EMA indicates a possible reverse in trend.
- Note: Close prices and volume are also being used in this case as well as these engineered features.


## Exploratory Data Analysis

The first thing we want to check when doing Time-Series models is that we want Stationary Data. Our original dataset, when using the Augmented Dickey-Fuller Test and the interpreting function written by Joseph Nelson, we saw a p-value of 0.92 which tells us are data is NOT stationary. To achieve that stationary requirement for time series, we differenced once and achieved a p-value of essentially 0.

We then want to check for seasonality and trend via seasonal decompose and correlation checks.
- Looking at our entire dataset on a seasonal decomposition does not give us much information. Given it has been an abnormal few years because of COVID this is not unexpected.
- Through our auto correlation and partial autocorrelation, we also don’t get much useful information.
- HOWEVER, since we are looking for intraday trading, these long timeframes don’t help us too much, we would benefit more from intraday time frames. We therefore set our ‘lag’ to about 450, which represents 450 minutes or 7.5 hours; the time from 9am-430pm.
- We still don’t see much in terms of seasonality within a day, but that makes sense considering we don’t have much in terms of a repeating cycle within a single day.
- Looking to our correlation visualizations, we still do not see much correlation between lags beyond the first few lags. This makes sense considering, again, our data is within a single day and the prices most close to our current price have the greatest effect on future prices in the short term. 
 - Note: this is also why EMA is considered so powerful for day trading.
- Lastly we want to look to our target variable, which is %change in close price.
 - Our target variable appears to be normally distributed with a mean of essentially 0 and a standard deviation of 0.056%
- We also categorized our target in binary and multiclass classification models
 - There is a common saying among stock market folk, “The market tends to go up.” 
  - We observed that 51.1% of the time, on 1 minute intervals, the market went DOWN and 48.9% of the time it went up.  
  - This is a bit misleading however, as we have a lot of values very close to 0 and these values caused some errors. Therefore values close to 0 were added to the ‘DOWN’ class.
 - To expand upon this further, we look to our multiclass classification of down, up, and flat.
  - We observed that (approximately):
   - 36% of the time the market stayed flat
   - Slightly more than 32% of the time the market went up
   - And slightly less than 32% of the time the market went down.
   - Looking back to our binary classification, we see now why ‘down’ was the majority class considering how much of the ‘flat’ multiclass was absorbed.


## Modeling

Unfortunately, we experienced many blockers during the modeling and additional work needs to be done in order to come to more concrete conclusions. 

We were able to fit a simple linear model to get an initial feel for the success of our day trading indicators.

This very basic model was able to capture 76.7% of our variability with a Root Mean Squared Error of 0.0127.
- This RMSE may appear very low, but remember that our average %change was 0 with a standard deviation of 0.0555.
- When comparing our RMSE with our STD, we see our RMSE is about 1/5 of our standard deviation. After looking at this, we can conclude that our RSME is reasonably low when considering the simplicity of this model, however this still can be the difference between a positive and negative % change.
- One can assume a more complex and optimized model will be more accurate with a lower RMSE.
- The graph is difficult to see due to the frequency and amount of data we have, however we do not see large deviations from our test data and therefore can conclude a reasonable degree of accuracy, especially when remembering the simplicity of the model.


## Conclusion & Recommendations

- Commonly recommended Day Trading Indicators appear to be effective tools in predicting price movements.
- Algorithmic day trading is best done on a high frequency, therefore the time period of 1 minute is a little long. 
- Day Trading is a mix of art and science. This prediction tool is best used as an additional indicator to help inform trading analysis and not relied upon exclusively.


## Improvements & Next Steps

- Initial Improvements here are obvious. The blockers and errors experienced during the modeling process need to be addressed and more modeling needs to be done, followed by extensive optimization.
- A major tool used by Day Traders, and more generally all technical traders, are ‘price levels’. These are difficult to hard code and need to be created and updated as time goes on but would theoretically improve our model’s effectiveness immensely.
- Building upon that previous point, this model would best be used to calculate the next 15 or so time periods within a single day, then re compiled to incorporate new data, refresh the model to most accurately predict the next short term period, and adjust/add/remove the previously mentioned levels.
