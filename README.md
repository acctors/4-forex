
forex_analysis.py

‏```python
‏import numpy as np
‏import talib
‏import pandas as pd
‏import yfinance as yf
‏from newsapi import NewsApiClient

‏def download_data(currency_pair, start_date, end_date):
‏    symbol = f"{currency_pair}=X"
‏    data = yf.download(symbol, start=start_date, end=end_date)
‏    return data

‏def calculate_rsi(data, period=14):
‏    close_prices = data['Close']
‏    rsi = talib.RSI(close_prices, timeperiod=period)
‏    return rsi

‏def calculate_sma(data, period=14):
‏    close_prices = data['Close']
‏    sma = talib.SMA(close_prices, timeperiod=period)
‏    return sma

‏def calculate_macd(data, fast_period=12, slow_period=26, signal_period=9):
‏    close_prices = data['Close']
‏    macd, signal, hist = talib.MACD(close_prices, fastperiod=fast_period, slowperiod=slow_period, signalperiod=signal_period)
‏    return macd, signal, hist

‏def calculate_bollinger_bands(data, period=20, nb_std_dev=2):
‏    close_prices = data['Close']
‏    upper, middle, lower = talib.BBANDS(close_prices, timeperiod=period, nbdevup=nb_std_dev, nbdevdn=nb_std_dev)
‏    return upper, middle, lower

‏def get_news(currency_pair, start_date, end_date, api_key):
‏    newsapi = NewsApiClient(api_key=api_key)
‏    news = newsapi.get_everything(
‏        q=currency_pair,
‏        from_param=start_date,
‏        to=end_date,
‏        language='en',
‏        sort_by='relevancy',
‏        page_size=100
    )
‏    return news

‏currency_pair = "EURUSD"
‏start_date = "2020-01-01"
‏end_date = "2021-09-01"
‏data = download_data(currency_pair, start_date, end_date)

‏rsi = calculate_rsi(data)
‏sma = calculate_sma(data)
‏macd, signal, hist = calculate_macd(data)
‏upper, middle, lower = calculate_bollinger_bands(data)

‏data['RSI'] = rsi
‏data['SMA'] = sma
‏data['MACD'] = macd
‏data['MACD_signal'] = signal
‏data['MACD_hist'] = hist
‏data['BB_upper'] = upper
‏data['BB_middle'] = middle
‏data['BB_lower'] = lower

‏print(data.tail())

‏api_key = "your_newsapi_key_here"
‏news = get_news(currency_pair, start_date, end_date, api_key)
‏print(news['articles'])
```
