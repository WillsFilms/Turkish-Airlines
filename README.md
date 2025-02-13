# **Turkish Airlines pdf transformation project**
This was a quick project inspired after my girlfriend was travelling overseas via Turkish Airlines (other airlines are available) and received a pdf file. The pdf file contained a table split over multiple pages containing information on the various films that were available to watch on the flight (in repository). She knew I was a data nerd so wondered whether I would be able to find a way to process this unstructured data and produce some analysis to help her pick a film.

![image](https://github.com/user-attachments/assets/2f7ec126-3e17-4071-9ddd-99709928c145)

Being me, I accepted this challenge with relish.

## Step 1 - Import the pdf file into Power Query
My first step was to import the raw pdf file into `Power Query` using the Get Data > PDF option:

![image](https://github.com/user-attachments/assets/d24316cf-fded-42f8-9933-57c5978eddf9)

Once selected, the pdf was read into the Power Query editor for transformation.

## Step 2 - Data processing and transformation 
At first look, the data isn't looking too hot and certainly isn't in the condition to be useful for analysis:
![image](https://github.com/user-attachments/assets/b44139a4-bd71-4438-9fbc-ac0ae550ba3e)

There are multiple null values, the columns have no headers, and some of the data is spilled over into adjacent rows and columns. In other words, it is MESSY.

Another thing to notice is that Power Query has produced an individual query (or table) for every page present in the pdf (41 total!). These will need to be appended/concatenated into a single query before loading into the report. Unfortunately, the column structures in each individual table do not all match meaning they cannot be appended. I therefore went through each table and ensured they were all transformed to have a matching schema

*(insert table schema that all individual tables had to follow)*

Now all tables had the same column structure I could append them all into one large table

*(show the append function)*

The tables have all been combined, but there are still some data cleaning issues to resolve before the dataset is ready.

## Step 3 - Saving the new produced dataset as a csv
Power Query in Power BI is designed to be used as a prepatory tool for data modelling, rather than a data processing tool like `pandas` or `dplyr`. Therefore, there is no way to automate the saving of the nice new cleaned dataframe into a csv format for further analysis and/or visualisation outside of Power BI. As a solution to this, I plotted all the data within the dataframe within a table visual and then used the export data option on the visual to export the data within the table as a csv file:

![image](https://github.com/user-attachments/assets/c889b54c-b6ef-4b77-bf51-317a70da30ea)
![image](https://github.com/user-attachments/assets/5bc59422-2b0f-4817-9fc9-218b8fd2569d)

This file can be found within the repository.

## Step 4 - Some fun with Power BI
Now that the data is cleaned and available within the Power BI report, it would be rude not to have some fun with it! After all I will need to provide some data-driven recommendations. I decided to create a TopN measure within the report so that I could calculate the top 10 highest rated films by IMDb score for the whole dataset, and for each individual genre. This shoudl provide a good recommendation base.


