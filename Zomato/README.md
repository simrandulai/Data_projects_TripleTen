# File Title: Zomato_TB

This was my Final project for the TripleTen Business Intelligence Analytics Program. It was an independent project designed to showcase what I have learned throughout the TripleTen Program.

Tableau Public Share Link: <a href='https://public.tableau.com/app/profile/simran.dulai/viz/Zomato_17138507108980/Dashboard1' target=_blank><u>here</u>.</a>

### Table of Contents
| File Number | Title | Description |
| :-----------: | ----------- |----------- |
| 1 | [Raw Data for CSA](https://github.com/Tiffany-Bergett/Data_projects_TripleTen/tree/main/Zomato/Raw%20Data%20for%20CSA) | Folder containing 2 .xlxs files, one for each table used in the analysis. |
| 2 | [Final Project Report.pdf](https://github.com/Tiffany-Bergett/Data_projects_TripleTen/blob/main/Zomato/Final%20Project%20Report.pdf) | A .pdf file with with the written report for this project. |
| 2 | [Final.pdf](https://github.com/Tiffany-Bergett/Data_projects_TripleTen/blob/main/Zomato/Final.pdf) | A .pdf file with with the written report for this project. |
| 3 | README.md | This current page with all relevant information about the project, just past the Table of contents. |
| 4 | [Requirements.txt](https://github.com/Tiffany-Bergett/Data_projects_TripleTen/blob/main/Zomato/Requirements.txt) | A simple .txt file with the provided project requirements as provided by TripleTen. |
| 5 | [Zomato.TB.pbix](https://github.com/Tiffany-Bergett/Data_projects_TripleTen/blob/main/Zomato/Zomato.TB.pbix) | Power BI save file for easy access to specific DAX, calculations, and measures. |

| Section Title | Description |
| ----------- |----------- |
| Data | Describes the source of data, included files, tables, and fields. |
| Description | Describes the final products purpose, software, format, and included visuals. |
| Assumptions | Describes assumptions to include given by TripleTen and assumptions made based on the data and task. |
| Process | A general outline of how this project formed from start to finish. |
| Findings | Insights learned from the data analysis. |

### Data
TripleTen provided an archived file of 5 seperate excel files from the mock company Zomato. I used 2 for this project.
- `'Zomato data.zip'`: Compressed exel files provided by team lead
    - `'orders'`: All orders made from the menu by all customers at all restaurants between Oct. 4th 2017 and June 26th 2020.
    - `'users'`: All customers who completed orders during the designated time frame and their demographic information.
- `'Measures Table'`: Created table for analysis and to maintain integrity of original files. Housing all used measures.
- `'Calendar'`: Created table for analysis and to maintain integrity of original files. Housing all date information for potential calculations.
- `'Segmentations'`: Created table for analysis and to maintain integrity of original files. Housing all segments and RFM scores needed for incusion.
- `'RFM Table'`: Created table for analysis and to maintain integrity of original files. Housing all RFM calculations.

### Description:
- This was a Customer Segmentation Analysis.
- 19 page Tableau Visualization
- Includes KPI cards, Pie charts, bar charts, RFM analysis, and dashboards.
- Purpose was to successfully complete the Zomato onboarding project to showcase analytical skills to the mock company.

### Assumptions:
- The provided test datasets are accurate, complete, and consistent.
- Missing values or inconsistencies are minimal and will not significantly impact the analysis.
- The column descriptions accurately reflect the content of each table.
- The provided tables (orders and users) contain all the necessary information for the chosen analysis.
- Zomato's business context and industry trends are considered while interpreting the data.

### Process:
I first learned of the problem presented in it's entirety and it's requirements for approval.
Then, I chose the software and created my first submission, the "Decomposition Plan".
After, I analyzed the data and created visualizations and dashboards for a second submission.
Lastly, I presented my findings in a report as my 3rd and final submission piece.

### Findings:
1. Customers mostly consist of 23-year-old unmarried men.
2. There is a natural distribution for age however the range is small at 18-34.
3. Women are close behind, but there are significantly more customers who are single than married.
4. Zomato’s customers usually have small family sizes (2-3), educated, but unemployed.
5. Employed customers tend to be below middle class (50,000 INR/yr).