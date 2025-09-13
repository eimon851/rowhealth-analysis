# Row Health Wellness Program Analysis (2019-2023)

*The objective of this project is to analyze the effectiveness of marketing campaigns at Row Health, with the aim of providing strategic recommendations for the allocation of marketing budgets across various future campaign categories.*

## About This Project

Established in 2016, Row Health is a medical insurance provider catering to thousands of customers across the United States. In 2019, they introduced new marketing campaigns focusing on wellness tips, the affordability of their plans, and preventative care. Customers have the option to choose from four different plans: bronze, silver, gold, and platinum, each varying in premiums and coverage rates.

With the recent addition of a data team, Row Health is planning its marketing budget for the year. The company aims to better understand the effectiveness of these marketing campaigns in relation to customer signups and subsequent patient claims. The marketing budget is focused on achieving two main goals: increasing the number of customer signups and enhancing brand awareness nationwide.

The dataset I analyzed consist of three tables and includes information on customers, campaigns and claims. Here is the Entity Relationship Diagram for the data I'll be using 

![Health Data ERD](https://github.com/user-attachments/assets/522e6fbe-a922-4e9f-8434-157b044f365b)
  
  ## Excel Insights Summary
  
To assess the effectiveness of our campaign, we concentrated on the following essential metrics:

1.  **Signup Rate**: This measures the percentage of individuals who view a campaign and then enroll in a Row Health plan.
2.  **Cost per Signup**: This indicates the average expenditure required to secure each signup from the campaign.
3.  **Click-through Rate (CTR)**: This represents the percentage of viewers who click on the campaign's link after seeing it.

### Signup Rate

- Health For All has the highest sign-up rate (2.08%), second highest number of signups (3545)
- The high signup rate can be accredited to the campaign type Health Awareness which had the highest signup rate (0.41%) across all campaign type
-  Golden Years Security and Benefit Updates have the lowest signup rates (0.01% and 0.02%, respectively) and the lowest signup count (23 and 45, respectively) indicating inefficiency in converting impression to signups

### Cost per Signup

- Golden Years Security has the highest cost per signup ($176.73), almost 4 times then the next campaign category Benefit Updates with a $47.81 cost per signup
- As for campaign platform, TV has the highest at $10.48 with the lowest signup rates .08%
- Offer Annoucement has a $99.70 cost per sign up and .01% signup rates 

### Click through Rate

- Across all categories, Health For All and Benefit Updates achieved the highest CTR at 25.48% and 22.17% respectively.
- The Family Coverage Plan received a significant number of impressions but did not generate any clicks. This needs further investigation, as it can be due to missing data or problems within the campaign.
- When it comes to platforms, Email leads with a click-through rate (CTR) of 16.71%, which is 94% higher than the second-best platform, Social Media, at 8.62%. In contrast, TV has a CTR of 0%.

### Technical Analysis

Sample from [Excel Workbook](/RowHealth-Analysis/rowhealth.xlsx/rowhealth%20case%20study.xlsx)

![image](https://github.com/user-attachments/assets/938a063c-b1de-4e4c-9eb7-6b9f080e0ce2)


## SQL Insights Summary

  

In this section, I focused on addressing specific business queries using BigQuery SQL. My analysis involved using aggregation functions, window functions, joins, filtering, CASE expressions, common table expressions (CTEs), and the QUALIFY clause with ranking function like ROW_NUMBER() to refine the results. You can find the SQL queries for these insights [here](/RowHealth-Analysis/rowhealth_queries/allqueries.md). 

  

### Monthly Product Claim Totals for 2020

- In 2020, Hair Growth Supplement has consistently had the highest number of claim each month except in May where Vitamin B+ Advanced Complex had the highest (392).

### June 2023 Claims Summary

- In June 2023, the total number of claim was 1069 with a total cost of $13.7K and a total of $83K was covered for those cost.

### Top 2 Hair Products in June 2023

- The top two hair products in June 2023 were Hair Vitamins Trio ($18K) and Hair Growth Supplements (~$12K) in claim amount.

### State with Most Claims vs. Highest Claim Amounts in 2023

- New Jersey had the highest number of claims (3.9K), which was correlated with the highest claim amount ($479K)

### Top Covered Amount Category on Christmas 2022

- Hair has the highest covered amount of $570

### Top 10 Customers with Most Claims

- Eduardo and Marylee are tied for most claims (55). The remaining customer are very close to this number.

### Platinum Customers in 2023 or Signed Up in 2022

- 8 customers have a platinum plan and either signed up in 2022 or 2023

### Avg Percent Reimbursement for Hair Products in NY or Supplements

- On average, most customer makes claims every ~360 days (almost every year then).

### Most Common Second Product for Multi-Order Customers

Sample code where I used various SQL concepts/functions

```sql
 -- For customers who have more than 1 order, which product is most often bought as the second product?

with cte as 
(select cl.customer_id,
  cl.product_name,
  cl.claim_date,
  row_number() over(partition by cl.customer_id order by cl.claim_date)
from rowhealth.claims as cl
left join rowhealth.customers as cu
  on cl.customer_id = cu.customer_id
qualify row_number() over(partition by cl.customer_id order by cl.claim_date) = 2
order by 1)

select
  cte.product_name,
  count(2) as product_count
from cte
group by 1
order by 2 desc;
```

- Vitamin B+ Advanced Complex is the second most bought product (3822 purchases) with Hair Growth Supplements as the third most purchased (1946 purchases)

# Recommendation

### Increase signups

- Consider prioritizing campaigns in the Health for All category since it has the highest signup rate by allocating more to its budget (10-15% increase)
- Trade the spending on SEO to email campaigns as both platforms have similar signups and signup rates but SEO  cost per sign up is a dollar more. Otherwise, keep prioritizing Social Media as its doing the best across all metrics.
- This campaign has the lowest signup rate at 0.01% and the highest cost per signup at $99.70. It is not cost-effective and should be reevaluated. Consider reallocating resources to more successful campaigns or overhauling the strategy entirely.

### Increase brand awareness

- Priortiize campaigns run on emails as it has the highest click through rate and lowest cost per click.
- Invest in high-quality content and technical SEO improvements to enhance organic search visibility and traffic
- Examine why the cost per signup for COVID-related campaigns is unusually high, with two signups costing more than $1,000 each, compared to the average signup cost of $3.70. Consider the possibility of discontinuing these campaigns entirely.

# Presentation

Using google sheets, a presentation for the marketing team covers the insights and recommendations mentioned above and can be found here. Below are a few highlights for quick reference.

![row_health_presentation pptx (2)](https://github.com/user-attachments/assets/49892d3b-3cc9-425c-b2a0-276c1976ec5e)

![row_health_presentation pptx (1)](https://github.com/user-attachments/assets/de9bbb62-04c3-4c31-ba2c-684ba84ac853)

![row_health_presentation pptx](https://github.com/user-attachments/assets/6ecb420a-b1c8-44a4-b7ae-382c99998890)

# Tableau Dashboard
The dashboard can be found on Tableau Public [here](/https://public.tableau.com/app/profile/mohammad.eimon/viz/rowhealth_tableau_dashboard/Dashboard2?publish=yes). This dashboard focuses campaign performance primarily on signup metrics at the category and platform level while proving marketing metrics all of which can be filtered by the campaign's category and platform.

![Dashboard 2](https://github.com/user-attachments/assets/9bc8dd05-8852-4ad0-ada5-e234150b5e6f)

