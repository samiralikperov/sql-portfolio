### dates

| **Column**       | **Description**                                         |
|------------------|---------------------------------------------------------|
| company_id       | A unique ID for the company.                           |
| date_joined      | The date that the company became a unicorn.           |
| year_founded     | The year that the company was founded.                |

### funding

| **Column**       | **Description**                                         |
|------------------|---------------------------------------------------------|
| company_id       | A unique ID for the company.                           |
| valuation        | Company value in US dollars.                           |
| funding          | The amount of funding raised in US dollars.           |
| select_investors | A list of key investors in the company.               |

### industries

| **Column**       | **Description**                                         |
|------------------|---------------------------------------------------------|
| company_id       | A unique ID for the company.                           |
| industry         | The industry that the company operates in.            |

### companies

| **Column**       | **Description**                                         |
|------------------|---------------------------------------------------------|
| company_id       | A unique ID for the company.                           |
| company          | The name of the company.                              |
| city             | The city where the company is headquartered.          |
| country          | The country where the company is headquartered.       |
| continent        | The continent where the company is headquartered.     |




| **Industry**                      | **Year** | **Number of Unicorns** | **Average Valuation (Billions)** |
|-----------------------------------|----------|------------------------|-----------------------------------|
| Fintech                           | 2021     | 138                    | 2.75                              |
| Internet Software & Services      | 2021     | 119                    | 2.15                              |
| E-commerce & Direct-to-Consumer   | 2021     | 47                     | 2.47                              |
| Internet Software & Services      | 2020     | 20                     | 4.35                              |
| E-commerce & Direct-to-Consumer   | 2020     | 16                     | 4.00                              |
| Fintech                           | 2020     | 15                     | 4.33                              |
| Fintech                           | 2019     | 20                     | 6.80                              |
| Internet Software & Services      | 2019     | 13                     | 4.23                              |
| E-commerce & Direct-to-Consumer   | 2019     | 12                     | 2.58                              |





## Situation:
Did you know that the average return from stock investments is around 10% per year? But who wants to be average?

## Task:
An investment firm asked for my help to analyze trends in high-growth companies. They wanted to find out which industries have the highest valuations and how fast new high-value companies are emerging.

## Action:
I accessed their unicorns database, which contains key data, and conducted an analysis to uncover these trends.

## Result:
By providing insights from my analysis, I helped the firm understand industry trends better and make informed decisions for structuring their portfolio.


## Explanation of the SQL Query
This SQL query analyzes trends among unicorn companies over the years 2019, 2020, and 2021, focusing on identifying the top industries by the number of unicorns and their average valuations.

### unicorn_years CTE:
This Common Table Expression (CTE) gathers relevant data from the industries, dates, and funding tables. It extracts the industry, date when the company became a unicorn, the year from that date, and the company’s valuation. The results are filtered to include only the specified years.

### top_industries CTE:
This CTE counts the number of unicorns in each industry based on the data from unicorn_years. It groups the results by industry, orders them by the number of unicorns in descending order, and limits the output to the top three industries.

### industry_data CTE:
This CTE calculates the number of unicorns and the average valuation (in billions) for the top industries identified in the previous CTE. It groups the data by industry and year, providing a clearer view of how these industries perform over time.

### Final Selection:
The main query selects the industry, year, number of unicorns, and average valuation from industry_data. The results are ordered first by year (in descending order) and then by the number of unicorns (also in descending order).


