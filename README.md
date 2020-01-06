#importing the neeeded libraries
import pandas as pd
import openpyxl
import xlrd
import numpy as np
import sklearn
from sklearn.compose import make_column_transformer
from sklearn.preprocessing import OneHotEncoder
from sklearn.neighbors import KNeighborsClassifier
from sklearn.pipeline import make_pipeline
from sklearn.model_selection import cross_val_score

#reading the excel file into pandas
emp_left = pd.read_excel("C:\Users\prince\TakenMind-Python-Analytics-Problem-case-study-1-1.xlsx",
                         sheet_name= "Employees who have left")
#checking for null values                        
print emp_left.isna().values.any()

#splitting the data into features and target
x = emp_left.drop(columns= ["Emp ID", "time_spend_company"])
y = emp_left.time_spend_company

#encoding the "dept" and "salary" column
column_trans = make_column_transformer((OneHotEncoder(), ["dept", "salary"]), remainder='passthrough')

instantiating the Knn model
KNN = KNeighborsClassifier()

#uing pipline to combine the encoded data to the model
pipe = make_pipeline(column_trans, KNN)

#alculate the mean cross validation score
print "Cross validation score:", cross_val_score(pipe, x, y, cv=10, scoring='accuracy').mean()

#mporting the new data to make prediction
exist_emp = pd.read_excel("C:\Users\prince\TakenMind-Python-Analytics-Problem-case-study-1-1.xlsx",
                         sheet_name= "Existing employees")
x_new = exist_emp.drop(columns= ["Emp ID", "time_spend_company"])

#fiting model
pipe.fit(x, y)

#make prediction
print  pipe.predict(x_new)
