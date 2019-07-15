# electric-car-sharing-overtime
An analysis of the electric car sharing overtime
import numpy as np
import pandas as pd
Validity
#Dropping some columns
df.drop(['Unnamed: 0','Address','ID','Geo point','minute','Displayed comment'], axis = 1 , inplace = True)
df.head()
#We notice that the column names lack uniformity,we therefore change the names to lowercase.
df.columns=df.columns.str.lower()
df.columns
#changing my "kind" column to lowecase for uniformity
df['kind'] = df['kind'].str.lower() 
df.head()
df.to_csv('cleaned_data')
OUTLIERS
# Making the dataframe from csv file 
import pandas as pd
outliers = pd.read_csv("cleaned_data") 

day= pd.DataFrame(outliers)

outliers.head(5)
#most popular hour
#using the IQR method
Q1=day.quantile(0.25)
Q2=day.quantile(0.50)
Q3=day.quantile(0.75)
IQR=Q3-Q1
print(IQR)
RANGE1=Q1-(1.5*IQR)
RANGE2=Q3+(1.5*IQR)
RANGE1
RANGE2
#Hence anything above the followig values are outliers:
#Bluecar counter  10.0
#Utilib counter  0.0
#Utilib counter 1.4  0.0
#Charge slots 0.0
#Slots 7.5
#month 4.0
#day 13th
#In this case well withhold the outliers since they can give reliable and valuable information on our project
#Checking for any missing values
day.isnull().head()
day.isnull().any()
day.isnull().sum()
#ignoring all the null values in "scheduled at" since it is not usedful in our analysis
day.columns
CONSISTENCY
#There are no duplicate columns 
day1=day.drop_duplicates(inplace=False)
ANALYSIS
day.describe()
#renaming the columns 
day.columns = day.columns.str.lower().str.strip().str.replace(' ', '_').str.replace('.', '')

day.columns
#creating columns for the  differences of the 3 types of cars
day['bluecar_new'] = day.bluecar_counter.diff(periods=-1)
day['utilib_new'] = day.utilib_counter.diff(periods=-1)
day['utilib_14_new']=day.utilib_14_counter.diff(periods=-1)
#dropping the previous car columns
columns_dropped = ['bluecar_counter','utilib_counter','utilib_14_counter']
day.drop(columns_dropped, inplace  = True, axis = 1)
#MOST POPULAR HOUR FOR PICKING A BLUECAR OVER APRIL IN PARIS

day[(day.city == 'Paris') & (day.bluecar_new < 0)].groupby('hour')['hour'].count().sort_values(ascending=False).head(1)

#MOST POPULAR HOUR FOR PICKING A BLUECAR 
day[(day.bluecar_new < 0)].groupby('hour')['hour'].count().sort_values(ascending = False).head(1)
day["public_name"].unique()
day["kind"].unique()
#MOST POPULAR STATION OVERALLY
day[day.kind=="station"].groupby("public_name")["public_name"].value_counts().sort_values(ascending=False).head()
#MOST POPULAR STATION AT THE MOST POPULAR PICKING HOUR
day[day.hour==21].groupby("public_name").count().sort_values(by="public_name",ascending=False).head()
#MOST POPULAR POSTAL CODE
day.groupby("postal_code").count().sort_values(by="postal_code").head()
#DOES THE MOST POPULAR STATION BELONG TO THE MOST POPULARPOSTAL CODE
no it does not.
#WHAT POSTAL CODE IS MOST POPULAR FOR PICKING AT THE MOST POPULAR HOUR
day[day.hour==21].groupby("postal_code").count().sort_values(by="postal_code",ascending=False).head()
#MOST POPULAR HOUR FOR PICKING A UTILIB 
day[(day.utilib_new < 0)].groupby('hour')['hour'].count().sort_values(ascending = False).head(1)
#MOST POPULAR HOUR FOR PICKING A BLUECAR 
day[(day.utilib_14_new < 0)].groupby('hour')['hour'].count().sort_values(ascending = False).head(1)


