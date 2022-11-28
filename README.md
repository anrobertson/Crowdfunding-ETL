# Crowdfunding-ETL

## Overview of analysis
Independent Funding received a dataset that contains information about the backers who have pledged to the live projects. My job on this project was to perform both an ETL (extract, transform and load) process and a dataset process.

## Resources
Software used
* PgAdmin 4
* Visual Studio Code
* Jupyter Notebook
* QuickDBD

## ETL
The first step I took was extracting the data. Using Jupyter Notebook, I imported the backer_info.csv into my DataFrame and converted each row into a dictionary values using the code shown below, after turning the rows into dictionary values, we can organize the data by adding columns and seperating the information.
```
# Iterate through the backers DataFrame and convert each row to a dictionary.
dict_values = []
for i, row in backers_info_df.iterrows():
    # Iterate through each dictionary (row) and get the values for each row using list comprehension.
    data = row['backer_info']
    converted_data = json.loads(data)
    row_values = [v for k, v in converted_data.items()]
    # Append the list of values for each row to a list. 
    dict_values.append(row_values)
```
The next step is the cleaning process. This required me to take the DataFrame I created and making changes such as splitting the full name into a "first name" and "last name" column by using ".str.split(' ', n=1, expand=True)". After doing so, I dropped the column that contained the combined first name/last name "name" column and reorganized my columns. 
```
#  Drop the name column
backers_cleaned = backers_df.drop(['name'], axis=1)

# Reorder the columns
backers_cleaned = backers_cleaned[["backer_id", "cf_id", "first_name", "last_name", "email"]]
backers_cleaned
```
After exporting the csv labeled "backers.csv", I switched to the ERD I created earlier on and created a "backers" table. By doing this, I was able to take the tables I created within QuickDBD and load the information into a PgAdmin 4 query and import the csv file into the table itself.
