## Summary

Dependencies:

*   Yahoos and finnhug APIs to get the stock price daily data and stock news.
*   catboost to train a binarry classification model
*   transformers to get LLM model for sentiment classification 
*   numpy, pandas, and sklearn  

Model development: 


*   Obtain the stock price data from yahoo finance API and news data from finnhub API. They are FREE APIs for this use case, at least.     
*   Extract features from news based on the LLM model. We fed the news to LLM (used default bert model under transformer) which was categorizing a news being positive with a given probability. We added additional news features, like category, source, and news time being pre-market or during market.     
*   We included additional features based on daily price moving average (10 and 20 days). From reading online, these seems to be a common way of incorporating prices. 
*   We used NASDAQ listing to select a set of stocks with a price above 20$, and traded volume > 500K. There are lots of stocks with low prices that fluctuate under different circumstances, thus without doing a bit more deep dive, I excluded them. I randomly picked 120 stocks for this assignment.  
*   Labels are defined as the price difference between close and open of a given day. The assumption here is that news mostly have a short term effect on stock volatility. This can be further refined based on various assumptions, but this is the assumption used in the analysis. Basically, we predict given the daily Open price and news in a given day (premarket or during market), can we predict if Close price will be higher (>5%) than the Open. This can be fine-tuned based on different assumptions and conditions.     
*   Catboost model used with a handful of features from news and price, as mentioned above.  
*   Model trained on the data between 2024-01-28 and 2024-10-28 
*   Model tested on the data between 2024-10-29 and 2025-01-28 

Model evaluation:


*   I used ROC metric to evaluate the model's performance
*   I optimized probability from ROC by miximing true positive rate and minimize false positive rate. This turned out to be 0.258
*   I provided two representations for model's performance: 1. ntile the test data into 1% level based on probability (from high to low), and check which bin captures most of the positive labels. Typically higher probability bins should have higher capture rates, which is seen from the above table. 
*   I also classified all data points as positives with probability higher than the threshold (see above). This selects best stocks subset, which has  4.3X higher capture rate for positive labels (stocks with Close price being higher by 5% than Open).   

Model future improvement:


*   Settle of volatile definitions. This is critical to align as to what we call volatility, and how to quantify. 
*   Sentiment analysis via LLM can be improved by fine tunning the best model on the data we typically see under these news. So the model has more accurate understanding
*   Incorporate some key words that are important for Stocks, this requires some domain knowledge, and also the problem we are trying to solve. 
*   Include more data sources capturing different aspects of the stock market.  
*   Tune the catboost model with better parameter subsets by performing cross-validation. 
