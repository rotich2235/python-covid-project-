# python-covid-project-
 **COVID-19 Global Data Tracker**
Author:ROTICH ELKANA
Last Updated: 2025-05-12
Data Source: Our World in Data - COVID-19 Dataset

**1. Project Overview**
This notebook explores the global spread and response to the COVID-19 pandemic using real-world data. We'll focus on total cases, deaths, vaccination rollouts, and country comparisons. The goal is to derive insights and visualize key trends.

**2. Data Collection & Loading**
import pandas as pd

# Load the dataset
df = pd.read_csv('owid-covid-data.csv')

# Display basic info
print(df.shape)
df.head()
**3. Initial Exploration**
# Check column names
df.columns

# Check for missing values
df.isnull().sum().sort_values(ascending=False).head(10)
# Check column names
df.columns

# Check for missing values
df.isnull().sum().sort_values(ascending=False).head(10)
# Check column names
df.columns

# Check for missing values
df.isnull().sum().sort_values(ascending=False).head(10)
**4. Data Cleaning**
# Filter relevant columns
cols = ['date', 'location', 'iso_code', 'total_cases', 'new_cases',
        'total_deaths', 'new_deaths', 'total_vaccinations', 'people_vaccinated',
        'population']

df = df[cols]

# Convert date to datetime
df['date'] = pd.to_datetime(df['date'])

# Filter countries of interest
countries = ['United States', 'India', 'Kenya']
df = df[df['location'].isin(countries)]

# Handle missing values
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
plt.figure(figsize=(12, 6))
for country in countries:
    country_df = df[df['location'] == country]
    plt.plot(country_df['date'], country_df['total_vaccinations'], label=country)

plt.title('Total Vaccinations Over Time')
plt.xlabel('Date')
plt.ylabel('Total Vaccinations')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()
 **Percent of Population Vaccinated**
 df['percent_vaccinated'] = (df['people_vaccinated'] / df['population']) * 100

plt.figure(figsize=(12, 6))
for country in countries:
    country_df = df[df['location'] == country]
    plt.plot(country_df['date'], country_df['percent_vaccinated'], label=country)

plt.title('Percent of Population Vaccinated Over Time')
plt.xlabel('Date')
plt.ylabel('% Vaccinated')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()
**7.Choropleth Map with Plotly**
import plotly.express as px

# Get latest date
latest_date = df['date'].max()
latest_df = df[df['date'] == latest_date]

fig = px.choropleth(latest_df,
                    locations="iso_code",
                    color="total_cases",
                    hover_name="location",
                    color_continuous_scale="Reds",
                    title=f"Total COVID-19 Cases by Country as of {latest_date.date()}")
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
