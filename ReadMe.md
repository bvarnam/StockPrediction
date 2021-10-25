# Intraday Trading via Day Trading Techniques & Indicators
---

### Data collected via AlphaVantage free API using extended intraday data. 
> https://www.alphavantage.co/documentation/

---

## Problem Statement 

In the wake of COVID and popularization of retail stock trading via Robinhood and the Gamestop/AMC frenzy, many people are beginning to trade on the stock market for themselves. 

One of the most popular methods for this new group of traders is a methodology called Day Trading, where positions are held short term, or intraday, utilizing what is called Technical Analysis to predict where changes in price will take place instead of fundamental analysis of the business itself.

**With this in mind, we at the Association for Retail Traders (ART) want to know if the most commonly recommended day trading indicators are effective in predicting stock price movements. We will be testing these indicators through a series of Time-Series Machine Learning Models and looking for the lowest error using Root Mean Squared Error.**

From our analysis, commonly recommended Day Trading Indicators appear to be effective tools in predicting price movements. Thought algorithmic intraday trading is best done on a high frequency, therefore the time period of 1 minute is a little long. Remember though that Day Trading is a mix of art and science. This prediction tool is best used as an additional indicator to help inform trading analysis and not relied upon exclusively.

*A detailed explanation of the process and results can be found in the Exec.md file.*

## Contents

Folders
- 00_Code contains the jupyter notebooks used to conduct our analysis and modeling.
- 01_Data contains the datasets used for the jupyter notebook analysis.
- 02_Assets contains the images and figures used for the presentation.

## Software Packages

- Pandas for data ingestions and cleaning.
- Numpy for mathematical operations.
- Scikitlearn for model preprocessing, selection, and training.
- Statsmodels for data analysis through visualizations and modeling.
- Tensorflow and keras for additional modeling.

## Data Dictionary

|Feature|Type|Description|
|---|---|---|
|time|*DateTime*|Timestamp of row(Index).| 
|open|*float*|First price for that time period.| 
|high|*float*|Highest price for that time period.|
|low|*float*|Lowest price for that time period.|
|close|*float*|Final price for that time period.|
|close_first_diff|*float*|Final price for that time period, differenced once.|
|volume|*float*|How many shares were traded in the time period.|
|vwap|*float*|Volume Weighted Average Price.|
|vwap_Distance|*float*|Distance from close price to vwap.|
|vwap_1std_above|*float*|Standard Deviation from rolling 12 period of VWAP added to VWAP.|
|vwap_1std_above_Distance|*float*|Distance from close price to vwap_1std_above.|
|vwap_2std_above|*float*|2 Standard Deviation from rolling 12 period of VWAP added to VWAP.|
|vwap_2std_above_Distance|*float*|Distance from close price to vwap_2std_above.|
|vwap_3std_above|*float*|3 Standard Deviations from rolling 12 period of VWAP added to VWAP.|
|vwap_3std_above_Distance|*float*|Distance from close price to vwap_3std_above.|
|vwap_1std_below|*float*|Standard Deviation from rolling 12 period of VWAP subtracted from VWAP.|
|vwap_1std_below_Distance|*float*|Distance from close price to vwap_1std_above.|
|vwap_2std_below|*float*|2 Standard Deviation from rolling 12 period of VWAP subtracted from VWAP.|
|vwap_2std_below_Distance|*float*|Distance from close price to vwap_2std_above.|
|vwap_3std_below|*float*|3 Standard Deviations from rolling 12 period of VWAP subtracted from VWAP.|
|vwap_3std_below_Distance|*float*|Distance from close price to vwap_3std_above.|
|9_EMA|*float*|Exponential Moving Average of period 9 from close price.|
|9_EMA_Distance|*float*|Exponential Moving Average of period 9 from close price subtracted from close price.|
|20_EMA|*float*|Exponential Moving Average of period 20 from close price.|
|20_EMA_Distance|*float*|Exponential Moving Average of period 20 from close price subtracted from close price.|
|EMA_Distance|*float*|9_EMA - 20_EMA.|
|EMA_cross|*str*|Where the 9 EMA is at in relation to the 20 EMA.|
|target|*float*|Percent change in close price from previous period.|
|target_binary_class|*str*|Class of target variable to classify positive and negative via 'up' and 'down'.|
|target_multi_class|*str*|Class of target variable to classify positive and negative via 'up', 'down', and 'flat'.|