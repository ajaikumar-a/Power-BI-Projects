
## About the dataset:
The dataset is a transnational data set that contains all the transactions occurring between 01/12/2010 and 09/12/2011 for a UK-based and registered non-store online retail. The company mainly sells unique all-occasion gifts. Many customers of the company are wholesalers.

Link to the data source: http://bit.ly/3Xkc6FG

## Objective:
To identify the wholesalers whom the company needs to focus on and potential wholesalers who can benefit the company.

## Steps taken:
- Data loading, cleaning, and transformation are done using Power Query.
- Created a calendar table, a lookup table containing all the customer ids.
- Created a data model by establishing relationships between these tables.
- Created DAX measures and calculated columns necessary for the analysis.
- Created visualizations for further analysis.

Link to interactive dashboard: http://bit.ly/3konHFj
## Key insights:
- The cancellation rate seems to be on a steady trend over the years and there has been a decrease in cancellations from November 2011 to December 2011.

- Although cancellations are not on the rise, the company's sales seem to have significantly declined from November 2011 to December 2011.

- According to the customer segmentation analysis, the most number of wholesalers are in the 'At Risk' category, i.e, the majority of the company's wholesalers are not valuable to the company in terms of the number of orders placed, frequency of these orders, and the total monetary value of these orders. There is a chance that the company might lose the wholesalers in this category.

- The second most number of wholesalers is in the category of 'Cannot Lose Them'. These wholesalers are much more valuable to the company since they bring high monetary value as well as a reasonable amount of orders frequently.

- The number of new wholesalers acquired by the company is comparatively low. The company should focus on acquiring high-value wholesalers by replacing the wholesalers in the 'Lost' category or the 'About to Sleep' category since these categories don't bring any value to the company anymore.

## Visualizations used:
- Cards
- KPI cards
- Table
- Funnel chart
- Area chart
- Slicers

## DAX functions used:
- CALCULATE
- FILTER
- SELECTEDVALUE
- SUM
- MAX
- MAXX
- SWITCH
- DATEADD
- DATEDIFF
- ISBLANK
- DIVIDE
- PERCENTILE.INC

## Other tools used:
- Bookmarks
- Drillthrough filter
- Page navigation
