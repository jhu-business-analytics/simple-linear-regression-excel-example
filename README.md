# Simple Linear Regression

We can use a __simple linear regression__ to approximate how two variables in a dataset are related. In our data for simple linear regression, we’ll have: 
 - An __independent variable__, which is our explanatory variable. This variable can be directly controlled. 
 - A __dependent variable__, which is our response variable. This variable cannot be directly controlled, and can depend on our independent variable.
 
If we think back to a general line graph, we know that our independent variable runs along the x-axis, and our dependent variable runs along the y-axis, and our graphed, straight line gives the formula y = mx + b, where m is the slope of the line and b is the y-intercept. 

![Alt Text] 

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

![Alt Text]

We’re going to perform a simple linear regression to see if we can create a model to help us determine an employee’s contract salary based on the number of years that they’ve worked in Baltimore City government based on a specific Baltimore City government department.

# Data Cleaning
## DESCR Column

First, we’ll clean the DESCR column to remove the department subcategory numbers so we can organize departments by name only. We’ll use the Text to Columns tool in Excel to separate out the department name from the subcategory number by splitting the column on a delimiter as shown in the gif below:

![Alt Text]

To do this, we:
1. Highlight the column we want to edit (DESCR)
2. Click on the Data menu 
3. Click on Text to Columns
4. Select “Delimited” then Next-- this means that we have a character/space/tab that can act as a boundary between the data we want to keep and separate in the column
5. Identify the delimiter in our column by selecting “Other” and then typing ( in the box. You’ll see a preview of how your data will be separated in the window below. Click Next.  
6. We only want to keep the column with the department name, so we can click on the column with the department subcategory numbers and then select “Do not import column (Skip).” <br><br>If we remember back to the Excel spreadsheet, we’ll notice that most rows in the DESCR column have the department name followed by a number in parentheses, however, some columns may have extra parentheses. <br><br>In order to make sure that we remove all extra columns created by our delimiter, we need to scroll down to find one of these extra columns appear, select it, and then click “Do not import column (Skip)” for this column as well. If you don’t do this, then excel will give us an error message to let us know that splitting our selected column will create a new column that will save over our data.
7. Click “Finish” and see your new columns with only the department name.
