# Portfolio_Management

import numpy as np
from scipy.stats import norm

# Define function to calculate call price
def call_price(S, K, r, q, T, sigma):
    d1 = (np.log(S / K) + (r - q + 0.5 * sigma ** 2) * T) / (sigma * np.sqrt(T))
    d2 = d1 - sigma * np.sqrt(T)
    return S * np.exp(-q * T) * norm.cdf(d1) - K * np.exp(-r * T) * norm.cdf(d2)

# Define function to calculate put price
def put_price(S, K, r, q, T, sigma):
    d1 = (np.log(S / K) + (r - q + 0.5 * sigma ** 2) * T) / (sigma * np.sqrt(T))
    d2 = d1 - sigma * np.sqrt(T)
    return K * np.exp(-r * T) * norm.cdf(-d2) - S * np.exp(-q * T) * norm.cdf(-d1)

# Define function to calculate call-put parity
def call_put_parity(S, K, r, q, T, sigma, call_price, put_price):
    return call_price(S, K, r, q, T, sigma) - put_price(S, K, r, q, T, sigma) - S * np.exp(-q * T) + K * np.exp(-r * T)

# Example usage
tickers = ['AAPL', 'MSFT', 'AMZN', 'GOOG', 'FB']
prices = [176.57, 117.97, 3182.62, 2316.16, 320.08] # current prices as of 5/1/2023
strikes = [180, 120, 3200, 2300, 325] # strike prices
expirations = [0.5, 0.5, 0.5, 0.5, 0.5] # time to expiration (in years)
risk_free_rate = 0.01
dividend_yields = [0.005, 0.007, 0.0, 0.0, 0.0]
volatilities = [0.2, 0.25, 0.3, 0.3, 0.35]

for i in range(len(tickers)):
    ticker = tickers[i]
    price = prices[i]
    strike = strikes[i]
    expiration = expirations[i]
    r = risk_free_rate
    q = dividend_yields[i]
    sigma = volatilities[i]
    
    # Calculate call price
    call = call_price(price, strike, r, q, expiration, sigma)
    
    # Calculate put price
    put = put_price(price, strike, r, q, expiration, sigma)
    
    # Calculate call-put parity
    parity = call_put_parity(price, strike, r, q, expiration, sigma, call_price, put_price)
    
    # Print results
    print(f"{ticker}: Call Price = {call:.2f}, Put Price = {put:.2f}, Call-Put Parity = {parity:.2f}")
