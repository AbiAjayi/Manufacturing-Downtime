# Manufacturing-Downtime

## Table of Content
- [Objective](#objective)
- [Goals](#goals)
- [Data Source](#data-source)
- [Solutions Delivered](#solutions-delivered)
- [Exploratory Data Analysis with SQL](#exploratory-data-analysis-with-sql)
- [Final Dashboard](#final-dashboard)
- [Business Insights](#business-insights)
- [Recommendations](recommendations)
  
## Manufacturing Downtime Analysis
### Objective
The primary objective of this project is to gather, analyze, and interpret detailed productivity and downtime data from the soda bottling production line. By tracking key metrics such as operator performance, product type, start and end times for each batch, and downtime causes, the goal is to identify inefficiencies, bottlenecks, and areas for improvement in the production process. This will help optimize production scheduling, reduce downtime, improve operator performance, and ultimately enhance overall productivity on the production line.
### Goals
The goal is to provide actionable insights into the operational efficiency of the soda bottling production line. Specifically, the project aims to:
1.	Measure Line Efficiency: Assess the current line efficiency by comparing total production time with the minimum required time for completing batches.
2.	Evaluate Operator Performance: Identify underperforming operators and analyze whether there is a correlation between operator experience and downtime or production delays.
3.	Analyze Downtime Factors: Identify and categorize the leading causes of downtime (e.g., mechanical failures, maintenance issues, material shortages, or operator errors) to prioritize corrective actions.
4.	Investigate Operator Error Trends: Determine if certain operators are prone to specific errors and provide targeted training or process improvements to reduce mistakes.

### Data Source
The primary data set used for this analysis is an Excel file containing detailed information about a soda manufacturing downtime details which include different varaibles such as the operators, factors leading to their downtime, their start and end time and their batches of production.

### Solutions Delivered
Created a mock up question that the dashboard will be answering
1.	What's the current line efficiency? (total time / min time)
2.	Are any operators underperforming?
3.	What are the leading factors for downtime?
4.	Do any operators struggle with types of operator error?

### Exploratory Data Analysis with SQL

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
```

### Final Dashboard


![Downtime Dashboard](https://github.com/user-attachments/assets/b514b053-c7cc-494a-9a9b-d706fce2b0f8)

### Business Insights

The production of soda is influenced by several factors, leading to downtime during operations. A total of 12 distinct factors contribute to this downtime, which are primarily divided into operator-related issues and mechanical or external causes. The production process involves four operators, each of whom encountered downtime due to different factors. The key insights observed are as follows:
1.	Operator-Related Issues:
- Machine Adjustment Issues: This was the most common error across all operators. While all operators faced machine adjustment challenges, Charlie, Dennis, and Dee encountered these problems more frequently, with each experiencing 3-4 instances of machine adjustment errors. In contrast, Mac only faced this issue once.
- Product Spill: This error was predominantly experienced by Charlie, Dennis, and Dee, while Mac did not face any product spillage issues during his shifts.
- Batch Coding Error: Charlie and Dee experienced a significant number of errors related to batch coding, while Dennis faced fewer such issues. Mac struggled primarily with batch coding errors and batch changes.
2.	Non-Operator Errors:
- Machine Failure: This was the most significant non-operator-related cause of downtime, affecting all operators. It was identified as the primary cause of mechanical downtime.
- Inventory Shortage: Inventory shortages were another common factor contributing to downtime, although not directly caused by the operators.
- Other Errors and Labeling Issues: These included errors related to labeling and other miscellaneous factors, which were not necessarily attributable to operator mistakes.
  
Key Problem Areas:
- Machine Adjustments: This was a recurrent issue for Charlie, Dennis, and Dee, leading to downtime.
- Machine Failures and Inventory Shortages: These issues are external to the operators but significantly impacted overall production efficiency.
- Batch Coding and Product Spills: These were common operator errors, particularly with Charlie, Dee, and Dennis.

### Recommendations
- Improve Machine Adjustment Protocols: Conduct a detailed review of machine adjustment procedures to identify root causes of frequent machine adjustment errors, particularly for Charlie, Dennis, and Dee. Training or standardization of adjustment processes should be implemented to reduce the frequency of these issues.
Consider introducing a checklist or automated adjustment system to guide operators in performing accurate machine adjustments, especially for those facing recurrent issues.
- Enhanced Operator Training and Support: Focus on providing additional training to Charlie, Dennis, and Dee in areas related to batch coding and product spill prevention. Special attention should be given to Charlie and Dee’s recurring batch coding errors, and Mac should be supported in improving his batch change efficiency.
Implement regular refresher courses or mentoring programs to ensure operators stay updated on best practices and error-free procedures.
- Address Machine Failures and Maintenance: Establish a more robust preventive maintenance program to reduce machine failures, which have been identified as a leading cause of downtime. A more proactive approach to machine upkeep could minimize production disruptions.
Develop a predictive maintenance system that can forecast potential machine failures based on historical data and performance trends, thus allowing for scheduled repairs and part replacements before breakdowns occur.
- Optimize Inventory Management: Investigate the causes of inventory shortages and develop strategies to address these. This could involve improving communication between inventory management and production teams to ensure that stock levels are maintained at optimal levels.
Consider implementing an automated inventory tracking system that can help monitor stock levels in real-time, reducing the likelihood of production delays due to inventory issues.
- Streamline Batch Coding and Labeling Processes: Review the batch coding process and identify potential areas for improvement. This includes reducing human error and improving system integrations to ensure accuracy.
Reinforce proper labeling techniques and conduct periodic audits to ensure compliance with labeling standards, helping reduce errors and improve production accuracy.

