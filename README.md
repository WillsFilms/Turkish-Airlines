# **Turkish Airlines pdf transformation project**
## Background
This was a quick project inspired after my girlfriend was travelling overseas via Turkish Airlines (other airlines are available) and received a pdf file. The pdf file contained a table split over multiple pages containing information on the various films that were available to watch on the flight (in repository). She knew I was a data nerd so wondered whether I would be able to find a way to process this unstructured data and produce some analysis to help her pick a film.

![image](https://github.com/user-attachments/assets/2f7ec126-3e17-4071-9ddd-99709928c145)

Being me, I accepted this challenge with relish.

## Step 1 - Import the pdf file into Power Query
My first step was to import the raw pdf file into the fantastic tool `Power Query` in Power BI by creating a new report and using the Get Data > PDF option:

![image](https://github.com/user-attachments/assets/d24316cf-fded-42f8-9933-57c5978eddf9)

Once selected from my file browser, the pdf was read into the Power Query editor for transformation.

## Step 2 - Data processing and transformation 
At first look, the data isn't looking too hot and certainly isn't currently in the condition to be useful for analysis:

![image](https://github.com/user-attachments/assets/7123f38c-0d4b-4da7-b568-61a991c09625)

There are multiple null values, the columns have no headers, and some of the data is spilled over into adjacent rows and columns. In other words, it is MESSY and will need some serious cleaning up.

Another thing to notice is that Power Query has produced an individual query (or table) for every page present in the pdf (41 total!). These will need to be appended/concatenated into a single query before loading into the report. Unfortunately, the column structures in each individual table do not all match meaning they cannot be appended in their current state. I therefore went through each table and ensured they were all transformed to have a matching schema (see below). This being Column 3 (Category), Column 4 (Film Name), Column 5 (IMDb Score), and Column 10 (Runtime).

![image](https://github.com/user-attachments/assets/ed1781fe-4c06-4d2d-adad-e0353d899621)

I prioritised combining the data tables together before doing the data cleaning as it would be more efficient. I also dropped any unnecessary data columns that would not be used in my analysis to reduce the size of the dataset and improve efficiency. Now all tables had the same column structure I could concatenate them all into one large table that contained all the data. I used the append queries as new function (see below) as this would allow me to keep the original raw data as a reference point. I made sure to remove the 'enable load' option for these original tables to make sure they were not loaded into the report.

![image](https://github.com/user-attachments/assets/b5dbfb72-a6ee-4a0e-b84f-09badfed0571)
![image](https://github.com/user-attachments/assets/2ca44ab6-9dbf-4b09-9206-cbd89e56d302)

The tables have all been combined, but there are still some data cleaning issues to resolve before the dataset is ready. I started by removing the rows which contained NULL values, then removed any straggling columns, promoted the first row to column headers, updated the data types of the columns, extracted text before delimeter, and then finally renamed the columns. The `M` code producing these data transformations can be found in the repository.

![cleaned data](https://github.com/user-attachments/assets/afac6cc0-74ad-49fa-957f-0e5b2147770e)

## Step 3 - Saving the new produced dataset as a csv file
Power Query in Power BI is designed to be used as a prepatory tool for data modelling, rather than a data processing tool like `pandas` or `dplyr`. Therefore, there is no way to automate the saving of the nice, new cleaned dataframe into a csv format for further analysis and/or visualisation outside of Power BI. As a solution to this, I plotted all the data within the dataframe within an unfiltered table visual and then used the export data option on the visual to export the data within the table as a csv file:

![image](https://github.com/user-attachments/assets/c889b54c-b6ef-4b77-bf51-317a70da30ea)

This file can be found within the repository.

## Step 4 - Some fun with Power BI
Now that the data is cleaned and available within the Power BI report, it would be rude not to have some fun with it! After all I will need to provide some data-driven recommendations. I decided to create a TopN measure within the report so that I could calculate the top 10 highest rated films by IMDb score for the whole dataset, and for each individual genre. This should provide a good recommendation base for my partner.

To do this, I plotted a horizontal bar chart showing the IMDb score vs. film name to show the 10 highest rated films in the database.

![image](https://github.com/user-attachments/assets/d6ea41e2-8524-4dd6-881b-489d7a6c6f04)

I filtered the scores using the TopN function in advanced filtering to only return the 10 films with the top 10 highest IMDb scores.

![image](https://github.com/user-attachments/assets/e1573c85-2482-4e3a-b591-4af7400db249)

I then filtered the data to not show Turkish cinema, World cinema or others as these did not interest my partner so would be redundant to include.

![image](https://github.com/user-attachments/assets/2bc1560a-f5fb-467c-b457-bf61fab2ce60)

I have set up a page in the report with the top 10 bar chart combined with a slicer for film category so that you can view the top 10 by genre/category.

![image](https://github.com/user-attachments/assets/a150d0f9-e04d-43d1-bf5a-cc44b8821829)

This `pbix` file can be found in this repository.

Thanks for reading! :) 

# Notes
This could be done with `Power Query` within `MS Excel` to extract the data into an Excel spreadsheet; the fundamental processing steps would remain the same and would allow you to produce and save data from the pdf with fewer steps.
