# Isurance_Data_Analysis_using_SQL

````sql
SELECT 
  sales.customer_id, 
  SUM(menu.price) AS total_sales
FROM dannys_diner.sales
INNER JOIN dannys_diner.menu
  ON sales.product_id = menu.product_id
GROUP BY sales.customer_id
ORDER BY sales.customer_id ASC; 
````


''''sql
  CREATE TABLE insurance (
  index int,
  PatientID int,
  age int,
  gender VARCHAR(50),
  bmi float,
  bloodpressure int,
  diabetic VARCHAR(50),	
  children int,
  smoker varchar(255),
  region varchar(255),
  claim float);
''''
***
''''sql
COPY insurance(index, PatientID, age, gender, bmi, bloodpressure, diabetic, children, smoker, region, claim)
FROM 'D:\Data\SQL_data\insurance_data.csv'
DELIMITER ','
CSV HEADER;
''''

''''sql
-- 1. Select all columns for all patients.
Select * from insurance;

''''
''''sql
-- 2. Display the average claim amount for patients in each region.

Select region, avg(claim) as claim
from insurance
group by region;
''''

-- 3. Select the maximum and minimum BMI values in the table.

Select min(bmi) as min_bmi, max(bmi) as max_bmi 
from insurance;


-- 4. Select the PatientID, age, and BMI for patients with a BMI between 40 and 50.

select PatientID,age,bmi from insurance where bmi between 40 and 50;


-- 5. Select the number of smokers in each region.

Select region, count(Patientid) as no_of_smoker
from insurance
where smoker = 'Yes'
group by region;


-- 6. What is the average claim amount for patients who are both diabetic and smokers?


Select  avg(claim) as avg_claim 
from insurance
where diabetic = 'Yes' and smoker = 'Yes';

-- 7. Retrieve all patients who have a BMI greater than the average BMI of patients who are smokers.

Select * from insurance;

select * 
from insurance where smoker='Yes' and  
bmi > (select avg(bmi) from insurance where smoker='Yes');

-- 8. Select the average claim amount for patients in each age group.

select 
	case when age < 18 then 'Under 18'
    when age between 18 and 30 then '18-30'
    when age between 31 and 50 then '31-50'
    else 'Over 50'
end as age_group,
avg(claim) as avg_claim
from insurance
group by age_group;

-- 9. Retrieve the total claim amount for each patient, 
-- along with the average claim amount across all patients.

select *,
sum(claim) over(partition by PatientID) as total_claim,
avg(claim) over() as avg_claim from insurance;

-- 10. Retrieve the top 3 patients with the highest claim amount, along with their 
-- respective claim amounts and the total claim amount for all patients.
select PatientID, claim,sum(claim) over() as total_claim from insurance
order by claim desc limit 3;



-- 11. Select the details of patients who have a claim amount 
-- greater than the average claim amount for their region.

select * from insurance t1
where claim > (select avg(claim) from insurance t2 where t2.region = t1.region);

select * from (select *, avg(claim)  over(partition by region) 
as avg_claim from insurance) as subquery
where claim > avg_claim;


-- 12. Retrieve the rank of each patient based on their claim amount.
select * , rank() over(order by claim desc) from insurance;



-- 13. Select the details of patients along with their claim amount, 
-- and their rank based on claim amount within their region.

select *, rank() over(partition by region order by claim desc) from insurance;




































