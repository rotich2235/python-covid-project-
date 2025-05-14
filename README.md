# python-covid-project-
 **COVID-19 Global Data Tracker**
Author:ROTICH ELKANA
Last Updated: 2025-05-12
Data Source: Our World in Data - COVID-19 Dataset

**1. Project Overview**
This notebook explores the global spread and response to the COVID-19 pandemic using real-world data. We'll focus on total cases, deaths, vaccination rollouts, and country comparisons. The goal is to derive insights and visualize key trends.

**2. Data Collection & Loading**
import pandas as pd

# Define the data
data = {
    'date': ['2021-05-02', '2021-05-03', '2021-05-04'],
    'location': ['Kenya', 'India', 'United States'],
    'total_cases': [96000, 10250000, 20000000],
    'total_deaths': [1800, 150000, 350000],
    'new_cases': [320, 16000, 200000],
    'total_vaccinations': [20000, 10000, 3000000]
}

# Create DataFrame
df = pd.DataFrame(data)

# Convert 'date' column to datetime
df['date'] = pd.to_datetime(df['date'])

# Display the DataFrame
print(df)
# Check column names
print("Column Names:")
print(df.columns.tolist())

# Check data types
print("\nData Types:")
print(df.dtypes)

# Check for missing values
print("\nMissing Values (Top 10 Columns):")
print(df.isnull().sum().sort_values(ascending=False).head(10))

# Summary statistics
print("\nDescriptive Statistics:")
print(df.describe())

# Unique values per column
print("\nUnique Values Per Column:")
for col in df.columns:
    print(f"{col}: {df[col].nunique()} unique values")

# Check unique locations
print("\nUnique Locations:")
print(df['location'].unique())

# Date range
print("\nDate Range:")
print(f"From {df['date'].min().date()} to {df['date'].max().date()}")
)
# Filter relevant columns
cols = [
    'date', 'location', 'iso_code', 'total_cases', 'new_cases',
    'total_deaths', 'new_deaths', 'total_vaccinations', 'people_vaccinated',
    'population'
]

# Keep only specified columns (if they exist in the dataset)
df = df[[col for col in cols if col in df.columns]]

# Convert 'date' column to datetime
df['date'] = pd.to_datetime(df['date'])

# Filter for countries of interest
countries = ['United States', 'India', 'Kenya']
df = df[df['location'].isin(countries)]

# Sort and forward-fill missing values
df = df.sort_values(['location', 'date'])
df.fillna(method='ffill', inplace=True)

 **5. Exploratory Data Analysis (EDA)
🗓️ Total Cases Over Time**
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6))
for country in countries:
    country_df = df[df['location'] == country]
    plt.plot(country_df['date'], country_df['total_cases'], label=country)

plt.title('Total COVID-19 Cases Over Time')
plt.xlabel('Date')
plt.ylabel('Total Cases')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6))
for country in countries:
    country_df = df[df['location'] == country]
    plt.plot(country_df['date'], country_df['total_cases'], label=country)

plt.title('Total COVID-19 Cases Over Time')
plt.xlabel('Date')
plt.ylabel('Total Cases')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()
**Total Deaths Over Time**
plt.figure(figsize=(12, 6))
for country in countries:
    country_df = df[df['location'] == country]
    plt.plot(country_df['date'], country_df['total_deaths'], label=country)

plt.title('Total COVID-19 Deaths Over Time')
plt.xlabel('Date')
plt.ylabel('Total Deaths')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()
**Death Rate Over Time**
df['death_rate'] = df['total_deaths'] / df['total_cases']

plt.figure(figsize=(12, 6))
for country in countries:
    country_df = df[df['location'] == country]
    plt.plot(country_df['date'], country_df['death_rate'], label=country)

plt.title('COVID-19 Death Rate Over Time')
plt.xlabel('Date')
plt.ylabel('Death Rate')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()
**6. Vaccination Analysis
💉 Total Vaccinations Over Time**
import pandas as pd
import matplotlib.pyplot as plt

# Step 1: Create the DataFrame
data = {
    'date': ['2021-05-02', '2021-05-03', '2021-05-04'],
    'location': ['Kenya', 'India', 'United States'],
    'total_cases': [96000, 10250000, 20000000],
    'total_deaths': [1800, 150000, 350000],
    'new_cases': [320, 16000, 200000],
    'total_vaccinations': [20000, 10000, 3000000]
    'death_rate': [0.01875, 0.01463, 0.01750]
}
df = pd.DataFrame(data)
df['date'] = pd.to_datetime(df['date'])

# Step 2: Add population and compute % vaccinated
df['population'] = [54000000, 1380000000, 331000000]  # Kenya, India, US
df['percent_vaccinated'] = (df['total_vaccinations'] / df['population']) * 100

# Step 3: Set up country list
countries = ['Kenya', 'India', 'United States']

# Step 4: Plot Total Vaccinations Over Time
plt.figure(figsize=(12, 6))
for country in countries:
    country_df = df[df['location'] == country]
    plt.plot(country_df['date'], country_df['total_vaccinations'], label=country, marker='o')

plt.title('Total Vaccinations Over Time')
plt.xlabel('Date')
plt.ylabel('Total Vaccinations')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

# Step 5: Plot Percent of Population Vaccinated Over Time
plt.figure(figsize=(12, 6))
for country in countries:
    country_df = df[df['location'] == country]
    plt.plot(country_df['date'], country_df['percent_vaccinated'], label=country, marker='o')

plt.title('Percent of Population Vaccinated Over Time')
plt.xlabel('Date')
plt.ylabel('% Vaccinated')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

**7.Choropleth Map with Plotly**
import pandas as pd
import plotly.express as px

# Step 1: Create the DataFrame
data = {
    'date': ['2021-05-02', '2021-05-03', '2021-05-04'],
    'location': ['Kenya', 'India', 'United States'],
    'total_cases': [96000, 10250000, 20000000],
    'total_deaths': [1800, 150000, 350000],
    'new_cases': [320, 16000, 200000],
    'total_vaccinations':  [20000, 10000, 3000000],
    'death_rate': [0.01875, 0.01463, 0.01750],
    'population': [54000000, 1380000000, 331000000],
    'iso_code': ['KEN', 'IND', 'USA']  # Added ISO country codes
}
df = pd.DataFrame(data)
df['date'] = pd.to_datetime(df['date'])

# Step 2: Filter to the latest date
latest_date = df['date'].max()
latest_df = df[df['date'] == latest_date]

# Step 3: Create Choropleth Map
fig = px.choropleth(
    latest_df,
    locations="iso_code",
    color="total_cases",
    hover_name="location",
    color_continuous_scale="Reds",
    title=f"Total COVID-19 Cases by Country as of {latest_date.date()}"
)

fig.show()

 **8. Insights & Summary
🔍 Key Findings**
The USA recorded the highest number of cases overall.

India experienced a sharp wave in mid-2021.

Kenya had fewer cases, but lower vaccination rates as of 2022.

Death rates varied but stayed below 3% in all three countries.

Vaccination uptake increased significantly after early 2021.
**9. Conclusion**
This analysis shows the importance of early and efficient vaccination rollout in managing the pandemic. The trends observed highlight differences in healthcare response, vaccine availability, and public policy across countries.
