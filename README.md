# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date:
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv('World-Stock-Prices-Dataset.csv')

df.columns = df.columns.str.strip().str.lower()

df['date'] = pd.to_datetime(df['date'], errors='coerce', utc=True)
df = df.dropna(subset=['date'])

if 'ticker' in df.columns:
    df = df[df['ticker'].str.lower() == 'aapl'].copy()


df = df.sort_values('date')
df = df.set_index('date')


df_close = df[['close']].groupby(level=0).mean()
resampled_data = df_close.resample('A').mean()


resampled_data.index = resampled_data.index.year
resampled_data.reset_index(inplace=True)
resampled_data.rename(columns={'date': 'Year'}, inplace=True)


years = resampled_data['Year'].tolist()
price = resampled_data['close'].tolist()

X = [i - years[len(years)//2] for i in years]
n = len(X)

if n < 2:
    print("Not enough data for trend analysis")
else:
    x2 = [i**2 for i in X]
    xy = [i*j for i, j in zip(X, price)]

    b = (n*sum(xy) - sum(price)*sum(X)) / (n*sum(x2) - (sum(X)**2))
    a = (sum(price) - b*sum(X)) / n

    linear_trend = [a + b*X[i] for i in range(n)]

  
    coeffs = np.polyfit(X, price, 2)   # avoids singular matrix error
    a_poly, b_poly, c_poly = coeffs

    poly_trend = [a_poly*(X[i]**2) + b_poly*X[i] + c_poly for i in range(n)]

  
    print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")
    print(f"Polynomial Trend: y = {a_poly:.2f}x² + {b_poly:.2f}x + {c_poly:.2f}")

    resampled_data['Linear Trend'] = linear_trend
    resampled_data['Polynomial Trend'] = poly_trend

    resampled_data.set_index('Year', inplace=True)

  
    resampled_data['close'].plot(marker='o', label='Actual Close Price')
    resampled_data['Linear Trend'].plot(linestyle='--', label='Linear Trend')
    resampled_data['Polynomial Trend'].plot(marker='o', label='Polynomial Trend')

    plt.title('Stock Price Trend Analysis (AAPL)')
    plt.xlabel('Year')
    plt.ylabel('Price')
    plt.legend()
    plt.grid(True)

    plt.show()
```
## OUTPUT
<img width="807" height="622" alt="image" src="https://github.com/user-attachments/assets/3331e406-a983-4f6d-b2c4-5abbb8337d38" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
