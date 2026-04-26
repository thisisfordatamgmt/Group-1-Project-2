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

## Description
Why we chose it (going to be written better, but waiting till we have more done)
We chose US Bureau of Labor Statistics (BLS) which targets Consumer Price Index (CPI), Average Prices (AP), Job Openings, and Labor Turnover Survey (JOLTS), State and Metro Area Employment, Hours, & Earnings (SAE), and Local Area Unemployment Statistics (LAUS) reports. 

## Two analytical questions 

### Question 1
- How did the turnover rates react to major economic disruptions (i.e COVID), and which industries were most affected?
### Question 2
- How did unemployment rates vary across U.S. regions over time, and which regions experienced the most significant changes during major economic disruptions such as COVID-19?
### Question 3
- 
for each: which columns are relevant, what makes it non-
trivial, and why it is interesting or meaningful


## Description 
(going to be written better, but waiting till we have more done)
number of tables, approximate row counts, key columns and data types

# Component 1: Snowsight Dashboard

## Question 1 

### Question: How did the turnover rates react to major economic disruptions (i.e COVID), and which industries were most affected?
### <img width="1282" height="288" alt="image" src="https://github.com/user-attachments/assets/66fd2efd-1629-485b-a85e-b7a98b828a74" />

### Interpretation: 

The chart is a line graph showing average turnover rates over the years (2000-2025 in the picture) for each industry. The x-axis displays the years, and the y-axis displays the average turnover rate expressed in a decimal format. The data were sorted by industry so that the reader/viewer can determine which industries were most affected by major economic disruptions. The COVID-19 pandemic occurred around 2020, and we can see a surge in average turnover rates across almost every industry. The Federal industry was honestly “unaffected” by the pandemic, as there was little to no change in its average turnover rate. However, the Financial industry remained unaffected, with no surge/shift, showing a straight line in its average turnover rate. On the other hand, industries like the Education and health services, along with Accommodation and food services, were both highly sensitive to the pandemic and reacted very significantly. Both industries saw a massive shift in their turnover rate, which can possibly be due to the fact that both industries required a more “personal” or “in-person” responsibility/factor and were not able to retain their workers because of the difficulty that would come from transforming  many of these jobs to a more virtual situation. The Accommodation and Food industry seemed to have the highest turnover rate overall, as it is seen as more of an industry for part-time/temporary/seasonal workers who are only working to earn a temporary wage, such as high school seniors working in fast food restaurants to help pay for college. The most stable industries were those that remained unaffected by disruptors and showed a low overall turnover rate throughout the graph. These industries were the Federal, Financial/Insurance, and Educational Services industries. These industries were probably able to shift to a virtual setting, given that they consist mainly of “behind-the-desk” jobs. This also shows why many students at Terry are Finance majors or are looking to go into/switch to Finance, since Finance is a very stable and safe career.

### Justification 

This question is highly meaningful for economic purposes. It examines how economic disruptions affect turnover rates. For major disruptors like COVID, one would want to see the effect on the workforce and how many exited accordingly. This question is also meaningful for social reasons. It specifies which industries are most affected by major economic disruptions. Industries least affected will automatically be categorized as stable career fields, and vice versa. Society will always value in-demand, strong, or stable industries and career fields. Students and young professionals will likely be more interested in a stable industry that’s not affected by major disruptions. The younger demographic seems to prefer stability amid rapid societal change. This question and its answers will be very beneficial to them. The BLS Employment_Timeseries and BLS Employment_Attributes tables were used to create this query. The date and value columns were used from the  BLS Employment_Timeseries table, and the industry and measure columns were used from the BLS Employment_Attributes tables.

## Question 2

### Question: 
How did unemployment rates vary across U.S. regions over time, and which regions experienced the most significant changes during major economic disruptions such as COVID-19?
### <img width="1234" height="496" alt="image" src="https://github.com/user-attachments/assets/6135e4a9-4989-4660-ac3e-50e2a71d88c7" />


### Interpretation

The chart shows the average unemployment rate for each U.S. region from 2009 through 2025. All four mainland regions (Northeast, Midwest, South, West) followed a similar downward trend after the 2008 recession, converging around 3-4% by 2019 before spiking sharply to nearly 15% in April 2020 due to COVID-19 lockdowns. The Territory line (Puerto Rico) stays consistently higher than all mainland regions throughout the entire period, demonstrating persistent structural unemployment that exists independent of national economic cycles. The COVID spike is the most dramatic feature of the chart, but what's notable is how quickly all regions recovered to pre-pandemic levels by 2022, suggesting the pandemic shock, while severe, was shorter-lived than the slow recovery from the 2008 financial crisis.

### Justification 

This question is meaningful because unemployment is one of the most important indicators of economic health, and regional differences reveal how economic shocks affect different parts of the country unevenly. Rather than asking a simple lookup question (e.g., "what was the unemployment rate in 2020?"), this question requires aggregating state-level data into regional groupings, comparing trends across multiple regions, and analyzing how those regions responded to a major disruption like COVID-19. The answer has real social and economic implications: it shows which regions are more economically resilient, which are more vulnerable to shocks, and how recovery patterns differ geographically. This information is relevant for policymakers, businesses considering expansion, and workers evaluating job markets.

### Data Manipulation 
#### Query:
```
SELECT ts.value as "Unemployment Rate", ts.date, 
CASE
    WHEN geo_name IN ('Connecticut','Maine','Massachusetts','New Hampshire','Rhode Island','Vermont','New Jersey','New York','Pennsylvania') THEN 'Northeast'
    WHEN geo_name IN ('Illinois','Indiana','Michigan','Ohio','Wisconsin','Iowa','Kansas','Minnesota','Missouri','Nebraska','North Dakota','South Dakota') THEN 'Midwest'
    WHEN geo_name IN ('Delaware','Florida','Georgia','Maryland','North Carolina','South Carolina','Virginia','District of Columbia','West Virginia','Alabama','Kentucky','Mississippi','Tennessee','Arkansas','Louisiana','Oklahoma','Texas') THEN 'South'
    WHEN geo_name IN ('Arizona','Colorado','Idaho','Montana','Nevada','New Mexico','Utah','Wyoming','Alaska','California','Hawaii','Oregon','Washington') THEN 'West'
    WHEN geo_name = 'Puerto Rico' THEN 'Territory'
  END AS region
FROM BUREAU_OF_LABOR_STATISTICS_EMPLOYMENT_ATTRIBUTES as att
JOIN BUREAU_OF_LABOR_STATISTICS_EMPLOYMENT_TIMESERIES as ts ON att.variable = ts.variable
JOIN GEOGRAPHY_INDEX as gi ON ts.geo_id = gi.geo_id 
WHERE att.measure = 'Unemployment Rate' 
AND att.unit = 'Percent'  
AND att.variable_name = 'Local Area Unemployment: Unemployment Rate, Seasonally adjusted, Monthly' 
AND gi.level = 'State' ORDER BY ts.date desc LIMIT 10000;
```

This query joins three tables to combine unemployment data with geographic information. The BUREAU_OF_LABOR_STATISTICS_EMPLOYMENT_ATTRIBUTES table contains metadata describing each measure, the BUREAU_OF_LABOR_STATISTICS_EMPLOYMENT_TIMESERIES table contains the actual unemployment values over time, and the GEOGRAPHY_INDEX table provides geographic identifiers like state names. The two BLS tables are joined on the variable column, and the time series is joined to the geography table on geo_id. The CASE statement is the key transformation — it groups individual states into the four U.S. Census regions (Northeast, Midwest, South, West) plus a Territory category for Puerto Rico, allowing regional-level analysis instead of state-by-state comparison. The WHERE clause filters the data down to only seasonally adjusted monthly unemployment rates at the state level (excluding national or county-level records), and the results are ordered by date with a 10,000-row limit to keep the dataset manageable for the dashboard. 

## Question 3

### Question: 

### <img width="1256" height="264" alt="image" src="https://github.com/user-attachments/assets/fb114403-980c-4fcc-9c53-de24e8ea68b8" />

### Interpretation
na

### Justification 
na

### Data Manipulation 

#### Query:
```
SELECT t.date,
(t.value - LAG(t.value,12) OVER (ORDER BY t.date)) 
        / LAG(t.value,12) OVER (ORDER BY t.date) AS inflation_rate
FROM BUREAU_OF_LABOR_STATISTICS_PRICE_ATTRIBUTES a
JOIN BUREAU_OF_LABOR_STATISTICS_PRICE_TIMESERIES t ON t.variable = a.variable
JOIN GEOGRAPHY_INDEX gi ON t.geo_id = gi.geo_id
WHERE a.variable_name = 'CPI: Gasoline, unleaded midgrade, Seasonally adjusted, Monthly, 1993-12 Index Date'
AND gi.geo_name='United States'
ORDER BY date desc
LIMIT 1000;
```

This query joins the ```BUREAU_OF_LABOR_STATISTICS_PRICE_ATTRIBUTES```, ```BUREAU_OF_LABOR_STATISTICS_PRICE_TIMESERIES```, and ```GEOGRAPHY_INDEX``` tables. The attributes table contains the metadata for the CPI and price statistics from the Bureau of Labor Statistics (BLS) while the timeseries table contains the individual measurements of the data described by the attributes table. The geography index table acts as a lookup table for the unique geographical ids found in the timeseries table for each measurement. This query then utilizes the ```LAG()``` function to get a year over year inflation rate over time. This utilizes the standard formula (current CPI value - previous CPI value) / previous CPI value. In this query specifically, ```t.value``` holds the current CPI value, and ```LAG(t.value,12) OVER (ORDER BY t.date)``` gets the previous CPI value for 12 periods prior (Since the data is recorded monthly, this gets the previous years value for CPI in the same month). Furthermore, the value is limited using the ```WHERE``` clause to the "CPI: Gasoline, unleaded midgrade, Seasonally adjusted, Monthly, 1993-12 Index Date" value and the United States as the geographical location. Finally, it is ordered by the date recorded from newest to oldest, and then limited by 1000 to reduce query time and cost. The rows returned to be used in the chart are the date of measurement/report and the calculated inflation_rate for that measurement.


# Component 2: Streamlit in Snowflake Ap

## Question 1 

### Question: 
How did the turnover rates react to major economic disruptions (i.e COVID), and which industries were most affected?

### <img width="1240" height="474" alt="image" src="https://github.com/user-attachments/assets/d2a69a2f-650f-4e21-bf2e-7f44fd9620c0" />

### Analytical Value: 

I have added two interactive elements to my Streamlit application: a multi-select feature for different industry types and a date range selector to view specific time periods. The reader/viewer can use these features to better understand how specific industries are affected using the multi select feature. Furthermore, he/she can use the data range selector to examine how specific periods of time affected the turnover rate of that industry, or to view all industries’ turnover rates at once to discern noticeable trends. These elements add analytical value to this application, making it more valuable to industry researchers and early-career professionals who are deciding which career fields to pursue. Job stability is a major factor in deciding a future career. I have used AI to help me through this process, specifically ChatGPT. I asked it to make my Streamlit application more analytically valuable, and I pasted the instructions for the component 3 section of the project instructions to better guide ChatGPT. ChatGPT returned a Python script that wouldn't run, so I decided to specify exactly which features I wanted added (multi-select and date range selector), and ChatGPT successfully generated a functioning Python script that did just that.

## Question 2 

### Question: 
How did unemployment rates vary across U.S. regions over time, and which regions experienced the most significant changes during major economic disruptions such as COVID-19?

### <img width="1294" height="636" alt="image" src="https://github.com/user-attachments/assets/9bc24e77-3d63-405a-8785-ee2c06ea1418" />

### Analytical Value: (need to check if this is right)
We used ChatGPT to enhance our original Streamlit app with the prompt: "Add analytical value to this streamlit app to make it better than it was before." The improvements transformed the app from a static data viewer into an interactive analysis tool. The original version simply displayed a line chart of unemployment rates over time, which only answered the question "what are the unemployment rates?" The improved version answers much more useful analytical questions: where is unemployment worst, where is it improving, and which regions are most economically unstable. Specifically, the multi-select region filter lets users isolate and compare specific regions for hypothesis testing (e.g., comparing only the South vs. the West). The 3-month moving average toggle smooths out short-term noise so users can see true macro trends like post-recession recovery rather than monthly fluctuations. The KPI cards (Highest Region, Lowest Region, Regional Gap) deliver instant answers without requiring users to visually estimate values from the chart. The Month-over-Month change bar chart adds directional insight, showing which regions are accelerating or slowing — a leading indicator rather than a static snapshot. Finally, the Volatility (standard deviation) chart introduces a risk dimension by highlighting which regions are economically unstable, which is useful for policy or workforce planning decisions. We accepted ChatGPT's suggestions as written because they each added a distinct analytical layer (filtering, smoothing, KPIs, momentum, volatility) without changing the underlying SQL or core visualization. 

### AI Prompt
- “Add analytical value to this streamlit app to make it better than it was before”

## Question 3 

### Question: 

### image <img width="1266" height="458" alt="image" src="https://github.com/user-attachments/assets/1514e5b7-7713-4bb9-bc5c-f1a86eb0a229" />


### Analytical Value and how we used AI:
We used ChatGPT with the same prompt as Query 1: "Add analytical value to this streamlit app to make it better than it was before." The original app showed a single line chart of year-over-year gasoline inflation, which displayed the data but offered no way to interpret or interact with it. The improved version adds several analytical layers that make the dashboard genuinely useful for understanding inflation dynamics. Adding Month-over-Month inflation alongside Year-over-Year gives users both short-term and long-term views — YoY shows the macro inflation trend, while MoM captures recent momentum that YoY can mask. The adjustable rolling average smoothing window (1-12 months) lets users control how much short-term volatility to filter out, which is critical for gasoline data because gas prices are notoriously noisy month-to-month. The date range selector allows users to focus on specific economic periods (e.g., the 2008 oil price spike, the 2020 COVID drop, or the 2022 post-pandemic surge) rather than viewing the entire 30-year history at once. The KPI metrics at the top (Latest YoY, Latest MoM, Trend direction) deliver the current state of inflation at a glance instead of forcing users to read the right edge of the chart. The conditional Key Insight box automatically interprets the latest value — flagging elevated inflation above 5%, declining prices, or moderate ranges — turning raw numbers into actionable interpretation. Together these features shift the app from "here is a chart of gasoline inflation" to "here is a tool for analyzing gasoline inflation across any time period at any level of smoothing." We kept all of ChatGPT's suggested improvements because they aligned with the project's goal of analytically meaningful interactivity, though we noted that the recession-shading feature was added as a placeholder rather than fully implemented. 

### AI Prompt
- “Add analytical value to this streamlit app to make it better than it was before”
