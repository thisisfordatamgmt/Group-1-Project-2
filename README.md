# MIST 4610 Team 1

## Team Name

Sp26_71552_Group 1

## Team Members

- Grace Buckley [@GraceBuckley](https://github.com/thisisfordatamgmt)
- Jerry Chen [@JerryChen](https://github.com/jakalin088)
- Sanjana Karanth [@SanjanaKaranth](https://github.com/SanjanaK1905)
- Sam Lashley [@SamLashley](https://github.com/samlashley)
- Sai Shankar Sadhu [@SaiShankarSadhu](https://github.com/saisadhu25-sketch)
- Anaya Stinson [@AnayaStinson](https://github.com/anayastinson)

# Dataset

## description
Why we chose it (going to be written better, but waiting till we have more done)
We chose US Bureau of Labor Statistics (BLS) which targets Consumer Price Index (CPI), Average Prices (AP), Job Openings, and Labor Turnover Survey (JOLTS), State and Metro Area Employment, Hours, & Earnings (SAE), and Local Area Unemployment Statistics (LAUS) reports. How is unemployment rate in the different areas of the US changed overtime? This is interesting because you can see the spike in uncomployment during covid.  

## Two analytical questions — 
for each: which columns are relevant, what makes it non-
trivial, and why it is interesting or meaningful


## Description 
(going to be written better, but waiting till we have more done)
number of tables, approximate row counts, key columns and data types

# Component 1: Snowsight Dashboard

## Question 1 

 ### Question: How did the turnover rates react to major economic disruptions (i.e COVID), and which industries were most affected?
### <img width="1282" height="288" alt="image" src="https://github.com/user-attachments/assets/66fd2efd-1629-485b-a85e-b7a98b828a74" />
### MySQL: 
SELECT 
    a.industry,
    t.date,  
    t.value AS turnover_rate,  
    t.value - t_prev.value AS turnover_trend  
FROM PUBLIC_DATA_FREE.BUREAU_OF_LABOR_STATISTICS_EMPLOYMENT_TIMESERIES t
JOIN PUBLIC_DATA_FREE.BUREAU_OF_LABOR_STATISTICS_EMPLOYMENT_ATTRIBUTES a 
    ON t.variable = a.variable
LEFT JOIN PUBLIC_DATA_FREE.BUREAU_OF_LABOR_STATISTICS_EMPLOYMENT_TIMESERIES t_prev
    ON t.variable = t_prev.variable
    AND t_prev.date = DATEADD(month, -1, t.date)
WHERE a.measure = 'Total separations'
ORDER BY a.industry, t.date
LIMIT 10000;

### Interpretation: 

The chart is a line graph showing average turnover rates over the years (2000-2025 in the picture) for each industry. The x-axis displays the years, and the y-axis displays the average turnover rate expressed in a decimal format. The data were sorted by industry so that the reader/viewer can determine which industries were most affected by major economic disruptions. The COVID-19 pandemic occurred around 2020, and we can see a surge in average turnover rates across almost every industry. The Federal industry was honestly “unaffected” by the pandemic, as there was little to no change in its average turnover rate. However, the Financial industry remained unaffected, with no surge/shift, showing a straight line in its average turnover rate. On the other hand, industries like the Education and health services, along with Accommodation and food services, were both highly sensitive to the pandemic and reacted very significantly. Both industries saw a massive shift in their turnover rate, which can possibly be due to the fact that both industries required a more “personal” or “in-person” responsibility/factor and were not able to retain their workers because of the difficulty that would come from transforming  many of these jobs to a more virtual situation. The Accommodation and Food industry seemed to have the highest turnover rate overall, as it is seen as more of an industry for part-time/temporary/seasonal workers who are only working to earn a temporary wage, such as high school seniors working in fast food restaurants to help pay for college. The most stable industries were those that remained unaffected by disruptors and showed a low overall turnover rate throughout the graph. These industries were the Federal, Financial/Insurance, and Educational Services industries. These industries were probably able to shift to a virtual setting, given that they consist mainly of “behind-the-desk” jobs. This also shows why many students at Terry are Finance majors or are looking to go into/switch to Finance, since Finance is a very stable and safe career.

# Component 2: Streamlit in Snowflake Ap

## Question 1 
### Question: 
### <img width="1240" height="474" alt="image" src="https://github.com/user-attachments/assets/d2a69a2f-650f-4e21-bf2e-7f44fd9620c0" />
### Analytical Value: 
I have added two interactive elements to my Streamlit application: a multi-select feature for different industry types and a date range selector to view specific time periods. The reader/viewer can use these features to better understand how specific industries are affected using the multi select feature. Furthermore, he/she can use the data range selector to examine how specific periods of time affected the turnover rate of that industry, or to view all industries’ turnover rates at once to discern noticeable trends. These elements add analytical value to this application, making it more valuable to industry researchers and early-career professionals who are deciding which career fields to pursue. Job stability is a major factor in deciding a future career. I have used AI to help me through this process, specifically ChatGPT. I asked it to make my Streamlit application more analytically valuable, and I pasted the instructions for the component 3 section of the project instructions to better guide ChatGPT. ChatGPT returned a Python script that wouldn't run, so I decided to specify exactly which features I wanted added (multi-select and date range selector), and ChatGPT successfully generated a functioning Python script that did just that.


