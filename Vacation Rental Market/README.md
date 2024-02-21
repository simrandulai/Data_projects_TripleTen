# File Title: nyc_airbnb_data

This was my first project I worked on in the TripleTen Business Intelligence Analytics Program. It was an independent project designed to showcase what I had learned for Advanced Spreadsheets.

Google Speadsheet can be found <a href='https://docs.google.com/spreadsheets/d/1p6cVEDzgZiiKLJV2daDAEzRK0GeIFd5e_yQdiyztLv8/edit?usp=sharing' target=_blank><u>here</u>.</a>

### Data
The data was one Google spreadsheet file provided by TripleTen:
- `'nyc_airbnb_data.csv'`: each row corresponds to one listing on AirBnB in September 2022
    - `'data_dictionary'`: summary of field titles seen in file and it's data type
    - `'listings'`: uniquely listings available with all available data
    - `'calendar'`: listings with upcoming availabilities and date type data

### Description:
- 11 page spreadsheet
- Includes raw data, processed data, data analysis, pivot tables, and a bar chart.
- Purpose was to determine what types of properties should be targeted for the vacation rental market, in the Manhattan borough of New York City based on available AirBnB data.

### Assumptions:
- Airbnb rentals are equivalent to the general vacation rental statistics.	
- Rental activity is assumed through the number of reviews in the last 12 months.
- Properties with no reviews in the last 12 months were considered inactive.
- High review ratings are greater than or equal to 4.5.
- Very actively rented properties are rented greater than or equal to 40 times over the last 12 months.
- Extremely low-priced listings are below $100 and super luxury listings are above $1000 and both were filtered from the analysis.

### Process:
- I first explored, filtered, and cleaned the data.
- Then I created aggregations and pivot tables to determine the type of properties that should be targeted.
- I performed calculations, pivot tables and charts to determine occupancy and estimated revenue.
- Lastly I finalized formatting for the client's readability.

### Findings:
1. The top 10 attractive neighborhoods for vacation rentals are as follows: Hell's Kitchen, Lower East Side, Harlem, Midtown, Upper West Side, Chelsea, East Village, East Harlem, West Village, Nolita.
2. The most popular vacation rental size is 1 bedroom overall. 
3. Fridays have the highest occupancy rate out of the week.
4. I identified the Lower East Side and 1 bedroom as a good investment. Estimated revenue for similar properties to recommendations is $74,850.33.
