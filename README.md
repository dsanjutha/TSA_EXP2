# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date:29.10.2026
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Create a mock cars_dataset.csv so the code runs immediately
try:
    data = pd.read_csv('cars_dataset.csv')
except FileNotFoundError:
    print("cars_dataset.csv not found. Generating a synthetic dataset to run the code...")
    np.random.seed(42)
    years_arr = np.repeat(np.arange(2010, 2024), 10)
    base_price = 15000 + (years_arr - 2010) * 1200 + (years_arr - 2010)**2 * 50
    noise = np.random.normal(0, 3000, len(years_arr))
    prices_arr = np.maximum(5000, base_price + noise)
    
    mock_df = pd.DataFrame({
        'year': years_arr,
        'price': prices_arr
    })
    mock_df.to_csv('cars_dataset.csv', index=False)
    data = pd.read_csv('cars_dataset.csv')

# Display the head of the raw dataset
print("First 5 rows of the dataset:")
print(data.head())

# Calculate average car price for each year
resampled_data = data.groupby('year')['price'].mean().reset_index()
resampled_data.rename(columns={'year': 'Year', 'price': 'Price'}, inplace=True)

print("\nAverage Car Price per Year:")
print(resampled_data.head())

years = resampled_data['Year'].tolist()
prices = resampled_data['Price'].tolist()

# Linear Trend Estimation
X = [i - years[len(years) // 2] for i in years]
x2 = [i ** 2 for i in X]
xy = [i * j for i, j in zip(X, prices)]

n = len(years)

b = (n * sum(xy) - sum(prices) * sum(X)) / (
    n * sum(x2) - sum(X) ** 2
)

a = (sum(prices) - b * sum(X)) / n
linear_trend = [a + b * X[i] for i in range(n)]

# Polynomial Trend Estimation (Degree 2)
x3 = [i ** 3 for i in X]
x4 = [i ** 4 for i in X]
x2y = [i * j for i, j in zip(x2, prices)]

coeff = [
    [n, sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(prices), sum(xy), sum(x2y)]

A = np.array(coeff)
B = np.array(Y)
solution = np.linalg.solve(A, B)
a_poly, b_poly, c_poly = solution

poly_trend = [
    a_poly + b_poly * X[i] + c_poly * X[i] ** 2
    for i in range(n)
]

# Display Trend Equations
print(f"\nLinear Trend Equation: y = {a:.2f} + {b:.2f}x")
print(f"Polynomial Trend Equation: y = {a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x^2")

# Add Trend Values
resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend
resampled_data.set_index('Year', inplace=True)

# Visualizing both Trend Estimations
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(16, 6))

# Linear Trend Plot
resampled_data['Price'].plot(kind='line', color='blue', marker='o', label='Average Price', ax=ax1)
resampled_data['Linear Trend'].plot(kind='line', color='black', linestyle='--', label='Linear Trend', ax=ax1)
ax1.set_title('Linear Trend Estimation')
ax1.set_xlabel('Year')
ax1.set_ylabel('Average Car Price')
ax1.legend()
ax1.grid(True, linestyle=':', alpha=0.6)

# Polynomial Trend Plot
resampled_data['Price'].plot(kind='line', color='blue', marker='o', label='Average Price', ax=ax2)
resampled_data['Polynomial Trend'].plot(kind='line', color='red', linestyle='-.', label='Polynomial Trend (Deg 2)', ax=ax2)
ax2.set_title('Polynomial Trend Estimation (Degree 2)')
ax2.set_xlabel('Year')
ax2.set_ylabel('Average Car Price')
ax2.legend()
ax2.grid(True, linestyle=':', alpha=0.6)

plt.tight_layout()
plt.show()
```

### OUTPUT:
A - LINEAR TREND ESTIMATION
<img width="905" height="340" alt="Screenshot 2026-10-29 192645" src="https://github.com/user-attachments/assets/f423b87c-5c5b-4140-9889-f7e78e634ff8" />


<img width="798" height="587" alt="Screenshot 2026-10-29 193101" src="https://github.com/user-attachments/assets/ad60d492-0756-4d23-9aed-c6d426d68f3a" />




B- POLYNOMIAL TREND ESTIMATION


<img width="788" height="577" alt="image" src="https://github.com/user-attachments/assets/6b46fac7-af9d-4a52-b422-bdae3b4fdc4a" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
