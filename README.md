# Manufacturing-Downtime
## Manufacturing Downtime Analysis
### Objective
The primary objective of this project is to gather, analyze, and interpret detailed productivity and downtime data from the soda bottling production line. By tracking key metrics such as operator performance, product type, start and end times for each batch, and downtime causes, the goal is to identify inefficiencies, bottlenecks, and areas for improvement in the production process. This will help optimize production scheduling, reduce downtime, improve operator performance, and ultimately enhance overall productivity on the production line.
### Goals
The goal is to provide actionable insights into the operational efficiency of the soda bottling production line. Specifically, the project aims to:
1.	Measure Line Efficiency: Assess the current line efficiency by comparing total production time with the minimum required time for completing batches.
2.	Evaluate Operator Performance: Identify underperforming operators and analyze whether there is a correlation between operator experience and downtime or production delays.
3.	Analyze Downtime Factors: Identify and categorize the leading causes of downtime (e.g., mechanical failures, maintenance issues, material shortages, or operator errors) to prioritize corrective actions.
4.	Investigate Operator Error Trends: Determine if certain operators are prone to specific errors and provide targeted training or process improvements to reduce mistakes.
### Solutions Delivered
STEP 1: Created a mock up question that the dashboard will be answering
1.	What's the current line efficiency? (total time / min time)
2.	Are any operators underperforming?
3.	What are the leading factors for downtime?
4.	Do any operators struggle with types of operator error?

STEP 2: Accessed data for quality and completeness in preparation for analysis

STEP 3: Conducted exploratory data analysis on the data with SQL

```sql

-- To find the current line efficiency by accessing the Minimum batch time against the production time

SELECT pp.Flavor,p.operator,p.Minbatchtime,Elapsetime
FROM productivity p
JOIN products pp ON
p.product_id=pp.product_id
WHERE Timediff_minutes>Minbatchtime;
```

```sql

-- Operators downtime and factors behind it

WITH CTE AS 
			(SELECT p.Operator, factors, f.Operator_Error,Timediff_minutes,Minbatchtime,p.Batch
				FROM Productivity p
					JOIN factors f ON 
						p.Batch=f.Batch
							WHERE Timediff_minutes>Minbatchtime
								AND Operator_Error='yes')
SELECT Operator, factors,COUNT(factors)
FROM CTE
GROUP BY Operator, factors;
