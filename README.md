# Cryptocurrency API Data Collection and Preparation

Collecting data by using <b>CryptoCompare API</b> and <b>Guardian API</b>. <br>
* Retrieving historical cryptocurrency data; in particular: open, high, low, close, volumefrom and volumeto daily historical data for Bitcoin and Etherium. 
* Retrieving all pieces of content on Bitcoin published by Guardian since 2017-04-01

## Two API sources were chosen for this project, provided by:

1) https://www.cryptocompare.com/api/#introduction Specifically, the historic 'HistoDay' daily data API - https://www.cryptocompare.com/api/#-api-data-histoday

--> Retrieves open, high, low, close, volumefrom and volumeto daily historical data for cryptocurrency of your choice. The values are based on 00:00 GMT time. It uses BTC conversion if data is not available because the coin is not trading in the specified currency.

2) http://open-platform.theguardian.com/ Specifically, all pieces of content on Bitcoin published in last year (since 2017-04-01) in the API - https://content.guardianapis.com/search?q=bitcoin&from-date=2017-04-01&to-date=2018-03-31&order-by=newest&show-fields=all&page-size=200&api-key=9d7f80a7-70ae-4b38-b22b-7a0c58c31c16

How it works: The Open Platform is a public web service for accessing all the content the Guardian creates, categorised by tags and section. To get started, You need an key to successfully authenticate against the API. The level of access I chose is Developer as it is free for non-commercial usage. I have register developer key at https://bonobo.capi.gutools.co.uk/register/developer

(Up to 12 calls per second, Up to 5,000 calls per day, Access to article text, Access to over 1,900,000 pieces of content, Free for non-commercial usage)

The API key I received which works here is 9d7f80a7-70ae-4b38-b22b-7a0c58c31c16

## Collecting data via API(s) & Parsing the collected data, and storing it in an appropriate file format
<b>CryptoCompare API Parameters</b>
Endpoint URL: https://min-api.cryptocompare.com/data/histoday?
fsym - REQUIRED The cryptocurrency symbol of interest [Max character length: 10] --> BTC and ETH
tsym - REQUIRED The currency symbol to convert into [Max character length: 10] --> I have selected EUR
limit - The number of data points to return --> 364 (returns 365 days of data)
toTs - Last unix timestamp to return data for --> 1522454400 (equivalent to 2018-03-31)

<b>The Guardian API Parameters</b>
Endpoint URL: https://content.guardianapis.com/search
api-key - The API key used for the query (retrieved key for which I registered) --> mine is 9d7f80a7-70ae-4b38-b22b-7a0c58c31c16
q - Request content containing this free text. Supports AND, OR and NOT operators, and exact phrase queries using double quotes. --> bitcoin
use-date - Changes which type of date is used to filter the results using from-date (2017-04-01) and to-date (2018-03-31)
order-by Returns results in the specified order (relevance - Default where q parameter is specified, oldest, newest - Default in all other cases) --> I have slected newest, as default would be relevance due to the fact that q is specified
show-fields - Add fields associated with the content --> all
page-size - Modify the number of items displayed per page --> 200

## Loading and representing the data using an appropriate data structure. Appling pre-processing steps to clean/filter/combine the data

I have loaded and represented data as a Pandas DataFrame. Some of the pre-processing steps I applyed before analysing the data were:
* converting unix timestamp string to readable date,\
* adding new 'Cryptocurrency' column to cryptoCompare data in order to specify the name of the cryptocurrancy, 
* merging data for Bitcoin and Ethereum within the same dataframe, and doing the same for gardian data from different time frames, filtering Guardian data for only valuable information/metrics, 
* slicing the nested columns, 
* imputing missing values, 
* creating a new DF based on the mean of the groupby object (getting Monthly averages to create a reduced size data set that can be easier visualised) etc


## Analysing and summarising the cleaned dataset

