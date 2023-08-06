## **Craigslist's Used Vehicles Solution** 

### Question 1). Time-Series Model. 

**My Approach:**

- First, I handled the missing values in the dataset. I resolved this by filling in the missing values with the mode of the respective column.

- Then, I converted the data types of the columns in the dataset to the appropriate data types, I converted the posting_date column should be converted to a datetime data type.

- I then used the posting_date column to create a datetime index for the dataset, this allowed me to easily analyze the temporal patterns of the data.

- Once I had clean data I explored it using different visualizations and statistical analysis approached. I used this step to analyze temporal patterns, seasonal trends, and demand-supply dynamics by region and vehicle type.And finally with a good understanding of the data, I created the time-series charts. 

**Solutions on Google Colabs:**
Pandas Explore the Data and Build the Model.ipynb on Google Colabs. 
	> https://colab.research.google.com/github/HarunMbaabu/Craigslist-Used-Vehicles-Solution-Athena/blob/main/Pandas%20Explore%20the%20Data%20and%20Build%20the%20Model%20-%20Complete.ipynb

PySpark Explore the Data and Build the Model.ipynb on Google Colabs.
	> Place Holder fppr the link

Note: There are two major reasons why I used both Pandas and PySpark, first pyspark processes large data better than Pandas and since craigslist_vehicles.csv is 1.4GB, it is quite huge for pandas.

Notebook Viewer for Pandas Scripts: https://nbviewer.org/github/HarunMbaabu/Craigslist-Used-Vehicles-Solution-Athena/blob/main/Pandas%20Explore%20the%20Data%20and%20Build%20the%20Model%20-%20Complete.ipynb 


The Second reason is it is impossible to preview the notebook on GitHub since the Panda's scripts file and visuals it too huge. To solve this I have used Notebook Viewer to show the notebook. 

### Question 2). Data Enrichment Recommendations.
- Data Enrichment Recommendations Google Docs
 	> https://docs.google.com/document/d/1xqjsStpwnFAlP7HwBJKqPG5Oyb8vPwDlgBU7rBXGe3k/edit?usp=sharing 

- Data Enrichment Recommendations on GitHub.
	> https://github.com/HarunMbaabu/Craigslist-Used-Vehicles-Solution-Athena/blob/main/Question%202%20and%203%20Docs/Data%20Enrichment%20Recommendations.pdf

### Question 3). Data Warehouse Structure.
- Data Warehouse Structure Google Docs.
 	> https://docs.google.com/document/d/1KQONTgf0KDme2s-GxvPmv0IfhoZR9Uf1XWKz6ZswBu4/edit?usp=sharing 

- Data Warehouse Structure on GitHub: 
	> https://github.com/HarunMbaabu/Craigslist-Used-Vehicles-Solution-Athena/blob/main/Question%202%20and%203%20Docs/Data%20Warehouse%20Structure.pdf


![Data Warehouse Structure Image](https://github.com/HarunMbaabu/Craigslist-Used-Vehicles-Solution-Athena/blob/main/Image/Screenshot%20from%202023-08-05%2021-08-18.png)

**PS:** 
To run this repository locally use conda environment or follow the steps below: 

(i).Check the Python packages and modules i used in the requirements.txt file. You can install these packages in your local virtual environment using 

```code 
pip install -r requirements.txt
```  

(ii). Make sure you configurethe dataset path, for this code i saved the dataset in ```data``` folder

You have commenter rights for the Google Docs, feel free to leave any feedback, or suggestions.  

