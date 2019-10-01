# Simple Linear Regression in Excel

# Table of Contents
We'll cover how to conduct a simple linear regression analysis in Excel with Baltimore City Open Data on Baltimore City government employee salaries in Fiscal Year 2018. This tutorial covers:

 - [Overview](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/README.md#overview)
 - [Data Cleaning](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example#data-cleaning)
     - [DESCR Column](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example#descr-column)
     - [HIRE_DT Column](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example#hire_dt-column)
     - [Filtering Data](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example#filtering-data-for-one-department-descr-column)
 - [Simple Linear Regression](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example#simple-linear-regression-1)
     - [Least Squares Line](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example#least-squares-line)
     - [Calculating Errors](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example#calculating-errors)
     - [Standard Error of Residual and Outliers](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example#standard-error-of-residual-and-outliers)
 - [Further Analysis](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example#further-analysis)
 
 The original files were exported from the [Baltimore City Open Data portal](https://data.baltimorecity.gov/City-Government/Baltimore-City-Employee-Salaries-FY2018/biyh-j8tc), and are available in this repository as an [Excel](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/Baltimore_City_Employee_Salaries_FY2018.xlsx) document and as a [CSV document](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/Baltimore_City_Employee_Salaries_FY2018.csv). The final excel document with the example covered in this tutorial is also available in this repository as an [Excel document](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/Baltimore_City_Employee_Salaries_FY2018_bus_analytics_in_class.xlsx).

# Overview

We can use a __simple linear regression__ to approximate how two variables in a dataset are related. In our data for simple linear regression, we’ll have: 
 - An __independent variable__, which is our explanatory variable. This variable can be directly controlled. 
 - A __dependent variable__, which is our response variable. This variable cannot be directly controlled, and can depend on our independent variable.
 
If we think back to a general line graph, we know that our independent variable runs along the x-axis, and our dependent variable runs along the y-axis, and our graphed, straight line gives the formula y = mx + b, where m is the slope of the line and b is the y-intercept. 

![Alt Text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/simple_linear_reg_images/line_example.png)

In this case, the dollars spent on advertising is our independent variable, and our sales are our dependent variable, and our line shows the trend y = 7x + 2. In most cases, our data won’t be clean enough for us to get an exact line through all of our data points, so we use linear regression to statistically determine the best-fit trendline with our data to help us predict the dependent variable based on the independent variable. 

We’ll use the FY18 Baltimore City Salary data from Baltimore City open data to run through an example of how to use linear regression to predict a person’s salary based on the number of years they’ve worked in Baltimore City government. The data in this example was exported from [Baltimore City’s open data portal](https://data.baltimorecity.gov/browse?category=City+Government) on September 24, 2019 and is also available in this repository. 

When we first look at our data we see that we can see the following information about all non-contract Baltimore City government employees in Fiscal Year 2018:
 - __NAME__: First and last name
 - __JOBTITLE__: Civil service or non-civil service job title in 
 - __DEPTID__: Baltimore City government department ID number
 - __DESCR__: Baltimore City government department name and subsection number
 - __HIRE_DT__: Date employee was hired in Baltimore City government
 - __ANNUAL__: Employee’s annual salary as noted in their contract 
 - __GROSS__: Employee’s actual earned income from Baltimore City government
 
Which looks like this in our excel spreadsheet: 

![Alt Text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/simple_linear_reg_images/baltimore_city_salary_raw_screenshot.png)

We’re going to perform a simple linear regression to see if we can create a model to help us determine an employee’s contract salary based on the number of years that they’ve worked in Baltimore City government based on a specific Baltimore City government department.

# Data Cleaning
## DESCR Column

First, we’ll clean the DESCR column to remove the department subcategory numbers so we can organize departments by name only. We’ll use the Text to Columns tool in Excel to separate out the department name from the subcategory number by splitting the column on a delimiter.

To do this, we:
1. Highlight the column we want to edit (DESCR)
2. Click on the Data menu 
3. Click on Text to Columns
4. Select “Delimited” then Next-- this means that we have a character/space/tab that can act as a boundary between the data we want to keep and separate in the column

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/descr_col_1.gif)

5. Identify the delimiter in our column by selecting “Other” and then typing ( in the box. You’ll see a preview of how your data will be separated in the window below. Click Next.  

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/descr_col_2.gif)

6. We only want to keep the column with the department name, so we can click on the column with the department subcategory numbers and then select “Do not import column (Skip).” <br><br>If we remember back to the Excel spreadsheet, we’ll notice that most rows in the DESCR column have the department name followed by a number in parentheses, however, some columns may have extra parentheses. 

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/simple_linear_reg_images/parentheses_excel_sheet.png)
In order to make sure that we remove all extra columns created by our delimiter, we need to scroll down to find one of these extra columns appear, select it, and then click “Do not import column (Skip)” for this column as well. If you don’t do this, then excel will give us an error message to let us know that splitting our selected column will create a new column that will save over our data.

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/descr_col_3.gif)

7. Click “Finish” and see your new columns with only the department name.

## HIRE_DT Column

One of the things we want to check is whether we can use length of time someone has been employed in Baltimore City government as an indicator for their offered salary. We have data about their hire date into city government (although, this may or may not reflect their hire date into this specific position), so we can calculate their employment time by using the TODAY Excel formula.

1. Insert a new column after the HIRE_DT column and name this employment_time_years
2. In the first cell of this column type in `=TODAY()-` and then click on the cell in that row under the HIRE_DT column.

The `TODAY()` function in Excel will return today’s date. Since we can perform simple arithmetic functions with any form of numbered cells, we can calculate the number of days that the employee has worked for Baltimore City government by subtracting the date that the employee was hired from today’s date.

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/hire_dt_1.gif)

3. Hit return/enter on your keyboard to return the time from the employee’s employment hire date to today in days

If this returns a date or other formatted value, reformat the cells in that column to produce a number with 2 decimal places

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/hire_dt_2.gif)

It’s not really useful for us to use number of days as the unit for evaluating length of hire, so we’ll edit our Excel formula in the formula bar to divide this new value by 365 (365 days in a year)

4. Once we have the number of years of employment in Baltimore City, we’ll drag this value down for the length of our column to calculate this value for every person in this dataset

## Filtering Data for one Department (DESCR Column)

Our last step to clean our dataset for a linear regression analysis is to filter our dataset for one Baltimore City government department. We can do this in a number of ways, but the easiest way is for us to add a filter onto our column headers, and then select the department from our DESCR column that we want to analyze. Here, we’re selecting the Police Department. 

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/filter_police.gif)

This will filter our data to include only the values in the DESCR column that have Police Department as the Baltimore City department description. Then, we’ll copy this filtered data into a new excel spreadsheet in our Excel workbook, and rename this “police_data”

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/simple_linear_reg_images/police_data_spreadsheet_name.png)

# Simple Linear Regression

## Least Squares Line

Now that we have cleaned data, we can conduct a simple linear regression to see how well we can use Baltimore City employment time to determine contracted salary. The easiest way to do this is through creating a scatter plot of all of our data points and then adding in a trendline to show the relationship between the length of time a person has been employed in Baltimore City government (independent variable) and their salary (dependent variable), and how well this relationship speaks for all of the data we’ve presented. 

1. Highlight both the employment_time_years and ANNUAL_RT columns and click on the Scatter chart option under the Insert menu

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/trendline_1.gif)

This gives us a plot of our data with the length of employment in Baltimore City on our x-axis and the contracted salary on the y-axis (what we want!)

2. Label the x-axis, y-axis, and chart title to that we remember what this data tells us by clicking on Add Chart Element and selecting the appropriate option for each label

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/trendline_2.gif)

3. Click on the data points in your chart and then click on Add Chart Element > Trendline > Linear to insert a trendline for this data

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/trendline_3.gif)

4. Sometimes this shows up as a line that’s a similar color or dashed line that blends in with our data points, so we reformat our data by right-clicking on the trendline, and changing the color, weight, and style of the trendline so that we can visualize it easier
5. Check the boxes to display the trendline equation and the R2 value on the chart
![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/trendline_4.gif)

The trendline equation gives us our best-fit line, or least squares line, which means that this line minimizes the error, or distance, between the “predicted” value from our line equation and the actual or observed value in our dataset for all of the data points

The R2 value tells us how well this line represents our data as a percentage, meaning an R2 value of 0.2424 tells us that the trendline given can explain approximately 24% of the data in our dataset
6. Although the scatter plot and trendline are helpful in visualizing what the least squares line and R2 value represent, we can also get these values without a graph with the following equations:

```
=SLOPE(known ys, known xs)
=INTERCEPT(known ys, known xs)
=RSQ(known ys, known xs)
```

Where known ys and known xs are the values we put in for our independent and dependent variable in our chart, respectively. We label these values in our dataset in cells M21:N24 so that the value for the slope is in N21, the value for the intercept is in N22, and the value for R2 is in N24.

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/simple_linear_reg_images/slope_intercept_rsq_excel.png)

## Calculating Errors

Now that we have our best fit line, we can calculate the error that this gives for each dependent datapoint. This will ultimately determine the spread of our data and identify any outliers in our data. 

1. Create a new column in the dataset named predicted_annual_salary
2. In the first cell of this column below the column header, we type the equation of our best fit line, replacing x with the cell of the value for x (value in the ANNUAL_RT column) in the same row. We can either type in the numbers as shown in our graph, or select and freeze (adding $ in front of the letter and number for the cell we want to freeze) the values for slope and intercept that we found with Excel formulas. You can freeze the cell value/add the $s with keystrokes by using the F4 function key after selecting that cell in the formula.

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/error_1.gif)

3. Drag these calculations down the entire column to calculate the predicted annual salary for every data point 
4. Next, we’ll calculate the error between our observed annual salary data and our predicted annual salary data for every data point in the next column. Label this column salary_error
5. In the first cell of this column below the column header, subtract the value in that row in the predicted_salary column from the value in the ANNUAL_RT column, which will look like

```
=G2-I2 
```

If ANNUAL_RT is in column G, and predicted_salary is in column I

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/error_2.gif)
 
6. Drag these calculations down the entire column to calculate the error between our observed annual salary and the predicted annual salary based on our least squares line 

These values can help us identify and potentially remove outliers in our data, which may help us refine our least squares line and understand more about the business operations around the outliers

## Standard Error of Residual and Outliers

While the R2 value tells us how well the least squares line predicts our data, this isn’t as helpful in telling us the spread of our dependent values. The standard error or residual tells us how closely all of our data fits our least squares line, where approximately 68% of our data should lie within one standard error of residual and approximately 95% of our data should lie within two standard errors of residual. Any value outside of 2 standard errors of residual are considered outliers in our data. 

To determine the outliers in our dataset, we conduct the following calculations: 

1. Calculate the standard error of the residual outside of our data set (this example calculates it in Excel cell N24) with the Excel formula

```
=STEXY(known ys, known xs) 
```
where the known ys are our independent variables (all of the values in the employement_time_years column) and the known xs are our dependent variables (all of the values in the ANNUAL_RT column)

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/ser.gif)

2. Determine which of our data points are considered outliers based on our error and standard error of the residual calculations. We know that a data point is considered an outlier if the absolute value of the error is greater than 2 x the standard error of regression, so we can show if each value is an outlier with the Excel IF statement:

```
=IF(ABS(J2)>2*$N$24, “outlier”, “not outlier”)
```

Where J2 is the value in the error column and O5 is the value in the standard error of regression column. Create a new column named outliers and input this formula in the first cell in the column below the header

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/screenshare_gifs/outlier_identification.gif)

3. Drag this formula down the entire column to see which values are outliers and with values are not outliers 

If we filter the data set now, we can see that the column is filled with only two values (outlier or not outlier), and we can filter our dataset to view only the outliers.

By analyzing our outlier data points, we can improve our organization’s operations by determining the probability or potential cause of “good” or “bad” outliers, and adjust operations to improve employee’s quality of life, organization profitability, organization processes, etc. Identifying outliers can also help us identify any typos or mistakes in our data (for example, if our data set shows that an employee has worked for Baltimore City for 120 years, we can identify this as a mistake, remove it from the data set, and improve our linear regression model). However, we should not remove data points simply because they are outliers.  

# Further Analysis

We notice that our simple linear regression doesn’t do a great job at helping us predict the employee’s annual salary based on the number of years worked in Baltimore City government. Our least squares line only accounts for approximately 24% of the data, and our standard error of residual is 17,096.77. 

_How can we better determine a model for predicting the employee’s salary?_

We’ll notice that there are almost 100 unique types of positions in the Baltimore Police Department, but the majority (almost two-thirds) of employees fall under Police Officer categories (Police Officer, Police Officer Trainee, and Police Officer EID). Let’s filter our data once more to only include Police Officer employee position titles, and then perform the analysis again. 

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/simple_linear_reg_images/police_officer_linear_reg_annual.png)

This gives us a better model fit for our data with an R2 value of 0.761 and a standard error of residual of approximately $5760. These values only give us a model to predict only the Baltimore City government employee’s salary as determined on their contracts, which doesn’t necessarily take into account what the Baltimore Police Department actually pays their employees and how they might want to think through budgeting for salaries in the department. If we conduct the same analysis with the GROSS column data (the actual payout for the employee’s salary from the department), we see a very different output.

![Alt text](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example/blob/master/simple_linear_reg_images/police_officer_linear_reg_gross.png)

Now, the R2 value is only 0.3903 and the standard error of residual of approximately $26681.19. This tells us that the Baltimore City employee’s tenure is not actually a great predictor of their final salary payment from the City of Baltimore, and that there may be other independent variables that actually determine how much a Baltimore Police Officer gets paid in a given fiscal year. If we want a better predictor of a Baltimore City Police Officer’s salary, we may need to take into consideration other characteristics such as if they are part-time vs. if they are full time, where they are stationed, external conditions around their time of employment, or the number of other officers in their unit. We may also want to dive deeper into additional trends in this dataset such as the year or month that they were hired or their department subcategory (that we initially filtered out in cleaning our data).

