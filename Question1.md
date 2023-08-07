# Question 1: Time-Series Model. 

> **I have not included the data folder, so configure the data to match the path given in the notebooks**

Transform the provided record-level dataset into a time-series model. The main objective of this model is to gain insights into the temporal patterns of vehicle listings, with a particular emphasis on conducting an inventory analysis over time, segmented by regions. For instance, the model should facilitate the creation of a time-series chart that represents the number of available vehicles over time, filtered by specific criteria such as region, vehicle type, etc. This will aid in understanding regional demand-supply dynamics, seasonal trends, and other relevant insights. 


### **Step 1:**
I will import the necessary packages, handle missing values, drop unnecessary columns, and convert the "posting_date" to a datetime data type.


```python
#load all the neccessary packages that i will use in the notebook. 
import pytz
import warnings
import pandas as pd
import plotly.express as px
import matplotlib.pyplot as plt
import plotly.graph_objects as go
from statsmodels.tsa.seasonal import seasonal_decompose
warnings.filterwarnings('ignore', category=FutureWarning)


# load the dataset into my notebook
data_path = "data/craigslist_vehicles.csv"
data = pd.read_csv(data_path)

#preview the first 5 rows of 'craigslist_vehicles.csv' dataset
data.head() 



#list the columns i have in my dataframe
data.columns



# check if columns exist before dropping
columns_to_drop = ['Unnamed: 0', 'url', 'region_url', 'VIN', 'image_url', 'description', 'county', 'lat', 'long', 'removal_date']

# filter the columns to drop only those that exist in the DataFrame
columns_to_drop_existing = [col for col in columns_to_drop if col in data.columns]

# drop the existing columns
data = data.drop(columns=columns_to_drop_existing)


# convert 'posting_date' to datetime data type
data['posting_date'] = pd.to_datetime(data['posting_date'],  utc=True)


#preview the first 5 rows of 'craigslist_vehicles.csv' dataset after droping unncessary columns and coverting "post)time" to date. 
data.head() 



# then i handle missing values: I fill the numeric ones with mean and categorical ones with mode. 
def handle_missing_values(data):
    # fill missing numerical values with mean
    numerical_columns = ['year', 'odometer']
    data[numerical_columns] = data[numerical_columns].fillna(data[numerical_columns].mean())

    # fill missing categorical values with mode
    categorical_columns = ['manufacturer', 'model', 'condition', 'cylinders', 'fuel', 'title_status',
                           'transmission', 'drive', 'size', 'type', 'paint_color', 'posting_date']
    data[categorical_columns] = data[categorical_columns].apply(lambda x: x.fillna(x.mode().iloc[0]))

    return data

data = handle_missing_values(data)
```

### **Step 2:**
After successfully handling missing values and cleaning the data i willl move to the next step where i will aggregate the data based on the "posting_date," "region," and "type" of vehicle, to be able to analyze the temporal patterns, seasonal trends, and demand-supply dynamics.

This will allow me to perform various analyses and gain insights into how the inventory varies over time in different regions and vehicle types


```python
def convert_to_tz_aware(posting_date):
    if not posting_date.tzinfo:
        return posting_date.replace(tzinfo=pytz.utc)
    else:
        return posting_date

data['posting_date'] = data['posting_date'].apply(convert_to_tz_aware)

data_agg = data.groupby(['region', 'type', 'posting_date']).size().reset_index(name='count')

data_agg = data_agg.sort_values(by='posting_date')

print(data_agg.head())
```

![Data Warehouse Structure Image](https://github.com/HarunMbaabu/Craigslist-Used-Vehicles-Solution-Athena/blob/main/Image/step2.1.png) 


```python
# Create an interactive time-series chart
fig = px.line(data_agg, x='posting_date', y='count', color='region', line_group='type',
              title='Number of Available Vehicles Over Time by Region and Vehicle Type',
              labels={'count': 'Number of Vehicles'})

# Customize the layout
fig.update_layout(
    xaxis_title='Posting Date',
    yaxis_title='Number of Vehicles',
    hovermode='x',
    showlegend=True,
)

# Show the chart
fig.show()
```

![Data Warehouse Structure Image](https://github.com/HarunMbaabu/Craigslist-Used-Vehicles-Solution-Athena/blob/main/Image/simple_trash.png)


```python
# group the data by day and count the number of listings
data_freq = data_agg.groupby(pd.Grouper(key='posting_date', freq='D')).sum().reset_index()

# create the time frequency graph
fig_freq = go.Figure(data=go.Bar(
    x=data_freq['posting_date'],
    y=data_freq['count'],
    marker_color='royalblue',
    opacity=0.8
))

# customize the layout
fig_freq.update_layout(
    title='Time Frequency Graph: Number of Vehicle Listings per Day',
    xaxis_title='Posting Date',
    yaxis_title='Number of Vehicle Listings',
    xaxis_tickangle=-45,
)

# show the time frequency graph
fig_freq.show()
```
![Data Warehouse Structure Image](https://github.com/HarunMbaabu/Craigslist-Used-Vehicles-Solution-Athena/blob/main/Image/step2.3.png)


```python
# perform seasonal decomposition
data_agg = data_agg.set_index('posting_date')
result = seasonal_decompose(data_agg['count'], model='additive', period=365)

# create a new DataFrame to store the decomposition components
decomposed_data = pd.DataFrame({
    'trend': result.trend,
    'seasonal': result.seasonal,
    'residual': result.resid,
})

# reset the index for plotting
decomposed_data = decomposed_data.reset_index()

# plot the seasonal decomposition
fig_decompose = go.Figure()

fig_decompose.add_trace(go.Scatter(x=decomposed_data['posting_date'], y=decomposed_data['trend'],
                                   mode='lines', name='Trend'))
fig_decompose.add_trace(go.Scatter(x=decomposed_data['posting_date'], y=decomposed_data['seasonal'],
                                   mode='lines', name='Seasonal'))
fig_decompose.add_trace(go.Scatter(x=decomposed_data['posting_date'], y=decomposed_data['residual'],
                                   mode='lines', name='Residual'))

# customize the layout
fig_decompose.update_layout(title='Seasonal Decomposition of Time Series',
                            xaxis_title='Posting Date',
                            yaxis_title='Counts',
                            showlegend=True)

# show the plot
fig_decompose.show()
```

![Data Warehouse Structure Image](https://github.com/HarunMbaabu/Craigslist-Used-Vehicles-Solution-Athena/blob/main/Image/step2.4.png)