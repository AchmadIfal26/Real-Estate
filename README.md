# -*- coding: utf-8 -*-
"""
Real Estate Project.ipynb

This script analyzes real estate data to find the year with the highest average sale amount.

Original file is located at
    https://colab.research.google.com/drive/1mEwHT09Fddnn0mSobAr0oxUQmMXgHB9P
"""

# Import libraries
import pandas as pd
import numpy as np

# Read data
estate = pd.read_csv('/content/real_estate.csv')

# Data cleaning
estate.dropna(subset=['Date Recorded'], inplace=True)  # Remove rows with missing dates

# Address column cleaning
estate['Address'] = estate['Address'].str.replace('Street', '')
estate['Address'] = estate['Address'].fillna('Unknown')

# Imputation for numerical columns
estate['Assessed Value'] = estate['Assessed Value'].fillna(estate['Assessed Value'].median())

# Imputation for categorical columns
estate['Residential Type'] = estate['Residential Type'].fillna('Unknown')
estate['Property Type'] = estate['Property Type'].fillna('Unknown')

# Imputation for missing coordinates (consider replacing with appropriate strategy)
estate['Longitude'] = estate['Longitude'].fillna('Unknown')
estate['Latitude'] = estate['Latitude'].fillna('Unknown')

# Recheck for missing values
print(estate.isna().sum())  # Check for remaining missing values

# Feature engineering (limited due to prompt focus)
property_type_counts = estate['Property Type'].value_counts()
print("Property type distribution:\n", property_type_counts)

# Calculate year with highest average sale amount
highest_sale_amount_by_year = estate.groupby('Date Recorded')['Sale Amount'].mean().sort_values(ascending=False).head(1)
year_with_highest_sale_amount = highest_sale_amount_by_year.index[0]

# Print result
print(f"Year with highest average sale amount: {year_with_highest_sale_amount}")

# Save data for further analysis (optional)
estate.to_csv('Estate Fix.csv', index=False)

# **For GitHub:**
# 1. Add comments to explain code sections.
# 2. Consider unit tests to ensure code correctness.
# 3. Include a README.md file to provide project overview and instructions.
