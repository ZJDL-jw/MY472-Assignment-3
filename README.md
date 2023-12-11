# MY472-Assignment-3

Dr Thomas Robinson and Dr Dan de Kadt

AT 2023
# Introduction #

This is a summative assignment, and will constitute 25% of your final grade. You should use feedback from seminars and your formative assessment to ensure you meet both the substantive and formatting standards for this module.

For clarity, the formatting requirements for each assignment are:

* You must present all results in full sentences, as you would in a report or academic piece of writing
  * If the exercise requires generating a table or figure, you should include at least one sentence introducing and explaining it. E.g. “The table below reports the counts of Wikipedia articles mentioning the LSE, by type of article.”
* Unless stated otherwise, all code used to answer the exercises should be included as a code appendix at the end of the script. This formatting can be achieved by following the guidance in the template.Rmd file (see Assignment 1).
* All code should be annotated with comments, to help the marker understand what you have done
* Your output should be replicable. Any result/table/figure that cannot be traced back to your code will not be marked

Exercise 1 (5 Marks)
This assignment will take us through a workflow that a data scientist might encounter in the real world, from data collection right through to analysis. Throughout this assignment we are going to use a local relational database to store a variety of different but related tables that we collect and may want to combine in various ways. We will want to ensure that each table within our database can be joined to every other table using a primary key.

We will start by creating an empty local relational database. You should store this new database in a ‘database’ folder that you create within your assignment folder. Follow these steps:

Use the DBI::dbConnect() function in R to create a new SQLite database (either YOUR_DB_NAME.sqlite or YOUR_DB_NAME.db) in your database folder.
Use the file.exists() function in R to check for the existence of your relational database.
Include in the main text of your .html submission the code that created the database and the code that checks for its existence and the output of that check.

Exercise 2 (25 marks)
a. Gathering structured data
Write an automatic webscraping function in R that constructs a table (as e.g. a data frame, a tibble, or a data table) of all R1 (Very High Research Activity) and R2 (High Research Activity) Research Universities in the United States of America. These data can be found on wikipedia.

Your initial scraper should collect five variables:

The university’s name
It’s status (public or private)
The city in which it is located
The state in which it is located
The URL of the university’s dedicated Wikipedia page
b. Gathering unstructured data
Extend your webscraping function (or create a new function) so that it navigates to the dedicated Wikipedia page for each university, and captures three additional variables:

The geographic coordinates of the (main) university campus
The endowment of the university in USD dollars
The total number of students (including both undergraduate and postgraduate)
c. Data munging
Download from the course website the ivyleague.csv file and store it appropriately on your local machine. We are going to use this file to focus our attention only on the subset of U.S. universities that are known as the “Ivy League.”1 This file includes a shortened version of each university’s name, the County and State in which the university’s main campus is located (we will use these variables later), and the university’s Employer Identification Number (EIN – we will use this variable later). Call this file into R and create three new variables in your main table:

An indicator for whether the university is an Ivy League institution
The university’s county (it would be wise to concatenate both county and state into a single string, separated by “,”)
The university’s EIN (which can be missing for those universities not in the Ivy League)
d. Writing to your relational database
Once you have combined all of the above data into a single table, ensure that it is in a tidy format where each row is a unique university, and each column is a variable (of which there should be exactly 11). Write this table to your relational database, making sure you give it an appropriate and clear name. Remember to ensure you have a primary key (e.g. the university name) that uniquely identifies each unit (university) in your table.

Create a function to check for the existence and correct dimensionality of your written table. The function should take two arguments: the name of your database, and the name of your table. If the table exists, the function should report as output the number of rows in the table, the number of columns in the table, and the names of the columns. For all of Exercise 2 it is sufficient for you to include just the code chunk that defines the function and the output of the function in the main section of your submitted .html file.

Exercise 3 (20 marks)
We are now going to use the Rselenium package to explore the Academic Ranking of World Universities.2

a. Scraping annual rank
Create a webscraper that returns, for the Ivy League university only:

The ARWU ranking for the university for the years 2003, 2013, and 2023. If the university’s rank is given as a range e.g. 76-100, convert this to the midpoint of the range – in this case 88.
Your final table should be in tidy long format, where each row uniquely identifies a combination of university and year (e.g., Harvard-2003). Write the data as a new table – appropriately and clearly named – to your relational database. Check for the existence and correct dimensionality of your written table using the function you wrote in Exercise 2.d. Include only the call to the function and the output in the main section of your .html file.

b. Scraping subject ranks for 2023
Extend your webscraper (or create a new one) that gathers for each Ivy League university only:

The rankings of the university for every social science for which the university has been ranked. Again, if a range is given, take the midpoint.
Your final table should be in tidy long format, where each row uniquely identifies a combination of university and discipline (e.g., Harvard-Economics). Write the data as a new table – appropriately and clearly named – to your relational database. Check for the existence and correct dimensionality of your written table using the function you wrote in Exercise 2.d. Include only the call to the function and the output in the main section of your .html file.

Exercise 4 (30 marks)
We are now going to gather a variety of additional data for each Ivy League university only from two APIs.

a. Gathering financial data from a raw API
First, for each Ivy League university only we are going to gather financial data from the ProPublica API. Using httr, access the Organization Method endpoint for each Ivy League university, using the EIN variable provided in Exercise 2.c, to gather the following variables for the years 2010 - 2020:

Total revenue
Total assets
Once you have retrieved these data, format them in a tidy long format, where each row is a unique combination of university and year (e.g., Harvard-2020). Write the data as a new table – appropriately and clearly named – to your relational database. Check for the existence and correct dimensionality of your written table using the function you wrote in Exercise 2.d. Include only the call to the function and the output in the main section of your .html file.

b. Gathering local economic data from a packaged API
The tidycensus package provides a convenient front-end for access to the US Census Bureau’s API (you will want to consult the documentation for the package closely). Using the package, retrieve the names of all the Counties in the US and their estimated median household income for every county for both 2015 and 2020 (based on the American Community Survey (ACS)).

Create a table for the Ivy League universities that includes the university name, the name of the County in which the campus is located, and the estimated median household income for the County, for 2015 and for 2020.

Format the final table in a tidy long format, where each row is a unique combination of university and year (e.g. Harvard-2015). Write the data as a new table – appropriately and clearly named – to your relational database. Check for the existence and correct dimensionality of your written table using the function you wrote in Exercise 2.d. Include only the call to the function and the output in the main section of your .html file.

Note: To access the US Census Bureau API (via tidycensus or otherwise) you must create a unique access key; details of this process can be found here. Remember to store your key securely and privately – tidycensus has a built in function for this purpose. Points will be deducted if you include a hard-coded access key in your solution.

Exercise 5 (20 marks)
Once you have completed Exercises 1 - 4 you should have five distinct tables in your relational database. Our goal is now to bring the data stored in these tables together in a variety of ways using SQL, and then analyse the data using R.

a. Analysis and visualisation
Using SQL (embedded within R), call into R from your relational database an analysis table that includes, for the Ivy League institutions only:

University name
The average rank of the university across 2003, 2013, and 2023
The average rank of the university’s Economics, Political Science, and Sociology programs, if they were ranked
The current endowment per student (total endowment divided by total number of students), in USD
The average total revenue per student across the years 2015 - 2020, in USD
The average of the median household income for the County across the years 2015 and 2020, in USD
Using ggplot, include in the main section of your .html five compelling, well-labeled plots that show the relationships between:

average university ranking and average Econ/PS/Soc ranking
average university ranking and endowment per student
average endowment per student and average median household income
average revenue per student and average median household income
Comment on the relationships you find. Are any of them particularly interesting?

b. Visualisation of geographic data
Using SQL, call into R from your relational database a table that includes, for every R1 and R2 university:

University name
Geographic coordinates
Status (public vs. private)
Whether the university is an Ivy League institution
Retrieve a shapefile of the United States using the tigris package (or, if you prefer the tidycensus package, but tigris is easier). Using either the tmap package or the ggmap package, include in the main section of your .html a visually clear and compelling map that is appropriately labelled which shows:

every R1 and R2 university, excluding the Ivy League institutions, as a point
where the colour of the points varies by status (public vs. private)
Ivy League universities as contrasting points
Is there any notable pattern to where the Ivy League universities concentrated? What about private and public universities? Do any parts of the United States appear particularly under-resourced in terms of research universities? How might you explain the patterns you observe?

Note: You can decide what geographies to include (see the documentation for more details, but to retrieve just an outline of the USA you could use tigris::nation(), or to retrieve an outline of the states you could use tigris::states(), etc.). You may also need to use the sf and sp packages, depending on your workflow.
