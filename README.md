# LA Crime Data Analysis Project

This project involves analyzing crime data from Los Angeles to uncover patterns and trends. The analysis is performed using Python in a Jupyter Notebook environment.

## Table of Contents

1. [Introduction](#introduction)
2. [Data](#data)
3. [Dependencies](#dependencies)
4. [Installation](#installation)
5. [Usage](#usage)
6. [Results](#results)
7. [Contributing](#contributing)
8. [License](#license)

## Introduction

The goal of this project is to analyze crime data in Los Angeles and identify key insights such as trends over time, the most common types of crimes, and spatial distribution of crime. The analysis uses various Python libraries to clean, visualize, and model the data.

## Data

The dataset used in this project consists of crime records from Los Angeles. Each record contains information such as:

- Date and time of the crime
- Type of crime
- Location of the crime
- Status of the investigation

The data is pre-processed to handle missing values and outliers before analysis.

## Dependencies

The project is built using Python and the following libraries:

- `pandas` for data manipulation
- `numpy` for numerical operations
- `matplotlib` and `seaborn` for data visualization
- `geopandas` for geospatial data analysis
- `scikit-learn` for machine learning models (if applicable)
  
You can install the required packages using the command:

```bash
pip install -r requirements.txt
```

## Installation
1. Clone the repository:
```bash
git clone https://github.com/yourusername/la-crime-data-analysis.git
```
2. Navigate to the project directory:
```bash
cd la-crime-data-analysis
```
3. Install the necessary dependencies:
```bash
pip install -r requirements.txt
```
4. Open the Jupyter Notebook:
```bash
jupyter notebook
```
## Usage
Once you open the Jupyter Notebook, you can run the cells sequentially to:

+ Load and clean the crime data
+ Visualize crime trends over time
+ Analyze crime types and geographic distribution
+ (Optional) Build a machine learning model to predict crime hotspots
## Results
Key insights from the analysis include:

+ Crime trends over the years
```python
# Removing incomplete data for the month & year of 12/2024
df = crime_df[~((crime_df['month'] == 12) & (crime_df['year'] == 2024))]

# Sorting by date occured
df.sort_values(by = 'DATE OCC')

### Plot of crimes over time by year & month
crime_over_time = df.groupby(['year', 'month']).size().plot()
```
![Crime trends over the years](https://github.com/njzmia/Python-Projects/blob/main/crime_over_time.png)
+ Distribution of crime according to victim age
```python
#Finding counts of victim ages
crime_df['Vict Age'].value_counts()

# Removing ages less than 0
df_age = crime_df[crime_df['Vict Age'] > 0]
df_age['Vict Age'].value_counts()

plt.hist(df_age['Vict Age'], edgecolor='white', bins=15)
```
![Victim Age](https://github.com/njzmia/Python-Projects/blob/main/age_plot.png)
+ Distribution of highest crime areas during a period of time
```python
# area with highest number of crimes during nighttime 8pm
nighttime_crime = df[(df['TIME OCC'] >= 2000) | (df['TIME OCC'] < 300)]
area_crime_count = nighttime_crime.groupby(by = nighttime_crime['AREA NAME']).size()

top_10_areas = area_crime_count.groupby('AREA NAME').mean().reset_index(name='avg_daily_count')\
    .sort_values(by= 'avg_daily_count', ascending=False).head(10)

top_10_areas_sorted = top_10_areas.sort_values(by='avg_daily_count', ascending=True)

plt.barh(top_10_areas_sorted['AREA NAME'], top_10_areas_sorted['avg_daily_count'])
```
![Crime Areas](https://github.com/njzmia/Python-Projects/blob/main/full_dash1.png)
+ Distribution of crime types (e.g., burglary, assault, theft)
```python
crime_count_per_day = df.groupby(by = [df['DATE OCC'], 'Crm Cd Desc']).size()

top_20_crimes = crime_count_per_day.groupby('Crm Cd Desc').mean().reset_index(name='avg_daily_count')\
    .sort_values(by= 'avg_daily_count', ascending=False).head(20)

top_20_crimes_sorted = top_20_crimes.sort_values(by='avg_daily_count', ascending=True)

plt.barh(top_20_crimes_sorted['Crm Cd Desc'], top_20_crimes_sorted['avg_daily_count'])
```
![Top 20 Crimes](https://github.com/njzmia/Python-Projects/blob/main/full_dash2.png)
The results are visualized using bar charts, line plots, and heatmaps for better understanding of the data.

## Contributing
Contributions are welcome! Feel free to open an issue or submit a pull request with any improvements or suggestions.

## License
This project is licensed under the MIT License - see the LICENSE file for details.
