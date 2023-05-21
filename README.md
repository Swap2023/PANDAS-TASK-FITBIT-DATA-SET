Fitbit Dataset

1. Read this dataset in pandas , mysql and mongodb 
2. while creting a table in mysql dont use manual approach to create it  ,always use a automation to create a table in mysql
 ## hint - use csvkit library to automate this task and to load a data in bulk in you mysql 
3. convert all the dates avaible in dataset to timestamp format in pandas and in sql you to convert it in date format
4 . Find out in this data that how many unique id's we have 
5 . which id is one of the active id that you have in whole dataset 
6 . how many of them have not logged there activity find out in terms of number of ids 
7 . Find out who is the laziest person id that we have in dataset 
8 . Explore over an internet that how much calories burn is required for a healthy person and find out how many healthy person we have in our dataset 
9. how many person are not a regular person with respect to activity try to find out those 
10 . who is the thired most active person in this dataset find out those in pandas and in sql both . 
11 . who is the 5th most laziest person avilable in dataset find it out 
12 . what is a totla acumulative calories burn for a person find out 


#SOLUTION QUE 1 Read this Fitbit Dataset in pandas , mysql and mongodb 

#Reading CSV Data using Pandas

import pandas as pd

fitbit=pd.read_csv(r"D:\STUDY\PYTHON\BIG DATA PRACTICE\INeuron_pandas\FitBit data.csv")

fitbit

#Reading Data in SQL

#First to load excel file directly into database .followthe steps.
#step 1 : Right click on table 
#step 2 : Select "Table Data Import Wizard"
#step 3 : Select the file which we have to load (in our case, it is FitBit data.csv) and press next.
#step 4 : Select create new table and give the table name and press next.
#step 5 : Select the required conditions and press next. The data is loaded Successfully 

##Pushing Data into Mongo DB
import pymongo
import pandas as pd
import json

from pymongo.mongo_client import MongoClient

uri = "mongodb+srv://New:new@cluster0.lidzegc.mongodb.net/?retryWrites=true&w=majority"

# Create a new client and connect to the server
client = MongoClient(uri)

# Send a ping to confirm a successful connection
try:
    client.admin.command('ping')
    print("Pinged your deployment. You successfully connected to MongoDB!")
except Exception as e:
    print(e)

#convert csv to json to uplaod dataset in mongodb

fitbit.to_json("fitbit.json")

pwd# checked in presnt work directory

data=pd.read_json(r"C:\Users\Swapnil\Ineuron Practice\fitbit.json")#crosscheckd data is fine

with open (r"C:\Users\Swapnil\Ineuron Practice\fitbit.json") as file:
    data=[(json.load(file))]

database = client['fitbitdata']#USING CONNECTIVITY WITH CLIENT DATABASE fitbitdata created

collection = database["pandastask"]#table pandastask created

print(database)

collection.insert_many(data)

# SOLUTION QUE 2 while creating a table in mysql dont use manual approach to create it  ,always use a automation to create a table in mysql
 ## hint - use csvkit library to automate this task and to load a data in bulk in you mysql 


#READING DATA IN SQL
import mysql.connector as conn
''' pip install mysql-connector-python'''
''' creating connection with sql  '''
mydb = conn.connect(host = "localhost" , user ="root" , passwd = "Ganpati@2022")
print(mydb)
'''connection established with sql '''
cursor=mydb.cursor()
cursor.execute("create database FitBitdataset")#data loaded in sql via connectivity from sql 

#SOLUTION 3 convert all the dates avaible in dataset to timestamp format in pandas and in sql you to convert it in date format

fitbit.head()

fitbit.dtypes

fitbit["ActivityDate"]=pd.to_datetime(fitbit["ActivityDate"])

fitbit.head(2)

fitbit.dtypes

#SOLUTION 4 Find out in this data that how many unique id's we have

fitbit.Id.unique()

len(fitbit.Id.unique())

#SOLUITION 5 which id is one of the active id that you have in whole dataset 

fitbit.head(2)

fitbit["TotalActiveMinutes"]=(fitbit['VeryActiveMinutes']+fitbit['FairlyActiveMinutes'] + fitbit['LightlyActiveMinutes'])

fitbit["TotalActiveMinutes"].max()

fitbit.sort_values('TotalActiveMinutes', ascending=False).iloc[[0]]

#SOLUTION 6 how many of them have not logged there activity find out in terms of number of ids

fitbit.head(2)

len(fitbit[fitbit['TotalActiveMinutes']== 0])

#SOLUTION 7 Find out who is the laziest person id that we have in dataset 

fitbit.head(2)

fitbit[fitbit['Calories']>0].iloc[0]

fitbit[fitbit['Calories']>0].sort_values(['Calories']).iloc[0]

#SOLUTION 8 Explore over an internet that how much calories burn is required for a healthy person 
#and find out how many healthy person we have in our dataset

fitbit.head(2)

#According to Internet: As a general guideline, women typically burn about 2,000 calories per day and men burn about 2,500 calories per day. These numbers are often used to determine 
#an approximate food intake for weight maintenance. As we don.t know the exact gender of the person, we will take average of both.
#i.e. 2250 calories are required to be burned per day for a healthy person.

len(fitbit[fitbit['Calories']>2250]['Id'].unique())

#And if we consider the minimum calories of the required calories burnt, then :
len(fitbit[fitbit['Calories']>2000]['Id'].unique())

#SOLUTION 9 how many person are not a regular person with respect to activity try to find out those 

fitbit

#SOLUTION 10  who is the third most active person in this dataset find out those in pandas and in sql both . 


fitbit.sort_values('TotalActiveMinutes', ascending=False)
