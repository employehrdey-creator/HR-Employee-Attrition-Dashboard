# HR-Employee-Attrition-Dashboard
I’m excited to share my latest Power BI project – HR Employee Attrition Analysis. In this project, I analyzed employee attrition patterns to understand which employee groups and workplace factors are associated with higher attrition.
# Project Overview

This project analyzes employee attrition and identifies the major factors contributing to employees leaving the organization.

# Tools Used
• Sql
• Power BI
• Power Query
• DAX
• Data Cleaning
• Data Analysis
# Sql
• 1 Total employees
select count(*) as total_employees
from    employee_attrition;  

• 2 Unique deparments;
select distinct Department
from employee_attrition;

• 3 Employees who left
select   count(*) as left_employee
from     employee_attrition     
where Attrition ="Yes";
• 4 Active employees
select   count(*) as left_employee
from     employee_attrition     
where Attrition ="No";
• 5 Employee Atrrition Rate
SELECT
    round(
        sum(case when Attrition = 'Yes' then 1 else 0 end) * 100.0
        / count(*),
        2
    ) as attrition_rate
from   employee_attrition ;    
• 6 Employees by department
select department,count(*) as department_employyes
from employee_attrition 
group by department;
• 7 Attrition rate by department
select department,count(*) as total_employees,
sum(case when Attrition ="Yes" then 1 else 0 end) as left_employee,
round(sum(case when Attrition="Yes"then 1 else 0 end)*100/count(*),2) as attrition_rate
from employee_attrition 
group by department
order by  attrition_rate desc;
• 8 Attrition Rate by job role
select JobRole, count(*) as total_employes,
sum(case when Attrition ="Yes" then 1 else 0 end) as left_employes,
round(Sum(case when attrition="Yes" then 1 else 0 end)*100/count(*),2) as attrition_rate
from  employee_attrition
group by JobRole
order by attrition_rate desc;
• 9 Top 5 roles with highest attrition
select Jobrole,count(*) as left__employees
from employee_attrition
where Attrition="Yes"
group by Jobrole
order by left__employees  desc
limit 5;
• 10 Does overtime affect attrition?
select Overtime,count(*) as left_employes,
round(sum(case when Attrition="Yes" then 1 else 0 end)*100/count(*),2) as overtime_attrition_rate
from employee_attrition
group by overtime;
• 11 Count gender employess 
select gender,count(*) as Gender_count
from employee_attrition
group by  gender;
• 12 gender attrition rate
select gender,count(*) as left_employes,
round(sum(case when Attrition="Yes" then 1 else 0 end)*100/count(*),2) as overtime_attrition_rate
from employee_attrition
group by gender;
• 13 MonthlyIncome group
SELECT
    CASE
        WHEN MonthlyIncome > 12000 THEN 'High Salary'
        WHEN MonthlyIncome > 7000 THEN 'Medium Salary'
        ELSE 'Low Salary'
    END AS Salary_Group,
    COUNT(*) AS Total_Employees
FROM employee_attrition
GROUP BY
    CASE
        WHEN MonthlyIncome > 12000 THEN 'High Salary'
        WHEN MonthlyIncome > 7000 THEN 'Medium Salary'
        ELSE 'Low Salary'
    END;
• 14 Attrition rate by Salary group
select
case 
when MonthlyIncome>12000 Then "High Salary"
when MonthlyIncome>7000 then "med salary"
else "Low Salary" end, count(*) as total_employees,
sum(case when Attrition ="Yes" then 1 else 0 end) as left_employees,
round(sum(case when Attrition ="Yes" then 1 else 0 end)*100/count(*),2) as Attrion_rate
from employee_attrition
group by 
case 
when MonthlyIncome>12000 Then "High Salary"
when MonthlyIncome>7000 then "Med salary"
else "Low Salary" end;
• 15 Attrition by job satisfaction
select 
case
when JobSatisfaction=4 then "High Job satisfaction"
when JobSatisfaction=3 then "Med Job satisfaction"
when JobSatisfaction=2 then "Avg Job satisfaction"
else "Bad Job satisfaction" end as statisfaction_group, count(*) as total_Employyes,
Sum(case when  Attrition ="Yes" then 1 else 0 end) as Left_employees,
round(sum(case when Attrition="Yes" then 1 else 0 end)*100/count(*),2) as attrition_rate_by_jobrole
from employee_attrition
group by  
case
when JobSatisfaction=4 then "High Job satisfaction"
when JobSatisfaction=3 then "Med Job satisfaction"
when JobSatisfaction=2 then "Avg Job satisfaction"
else "Bad Job satisfaction" end
order by attrition_rate_by_jobrole desc;
• 16 Top 10 highest paid employees
 select Department,Jobrole,Gender,MonthlyIncome
from employee_attrition
order by  MonthlyIncome desc
limit 10;
• 17 Find department wise Second-highest salary
select Department,Jobrole,Gender,MonthlyIncome
from (Select  Department,Jobrole,Gender,MonthlyIncome,rank() over(partition by Department order by MonthlyIncome desc) as rnk from employee_attrition)t
where rnk=2;
• 18 Rank employees by salary
select Department,Jobrole,Gender,MonthlyIncome,TotalWorkingYears,rank() over (order by MonthlyIncome desc)as ranks
from employee_attrition;
• 19 Average performance rating by department
select Department,round(avg(PerformanceRating),2) as Avg_rating
from employee_attrition
group by department;
• 20 How many  employees earning above department average?
select count(*) as total_employes
from employee_attrition as e
where MonthlyIncome>(select avg(MonthlyIncome)
from employee_attrition
where department=e.department
);
• 21 Does work-life balance affect employee attrition?
SELECT
    CASE
        WHEN WorkLifeBalance = 1 THEN 'Bad'
        WHEN WorkLifeBalance = 2 THEN 'Good'
        WHEN WorkLifeBalance = 3 THEN 'Better'
        WHEN WorkLifeBalance = 4 THEN 'Best'
    END AS work_life_group,

    COUNT(*) AS total_employees,

    SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS employees_left,

    ROUND(
        SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0
        / COUNT(*),
        2
    ) AS attrition_rate

FROM employee_attrition
GROUP BY WorkLifeBalance
ORDER BY WorkLifeBalance;
• 22 Does business travel affect employee attrition?
SELECT
    BusinessTravel,
    COUNT(*) AS total_employees,
    SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS employees_left,
    ROUND(
        SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0
        / COUNT(*),
        2
    ) AS attrition_rate
FROM employee_attrition
GROUP BY BusinessTravel
ORDER BY attrition_rate DESC;

#  Key KPIs
• Total Employees: 1,470
• Active Employees: 1,233
• Employees Left: 237
• Attrition Rate: 16.12%
• Average Age: 36.92
• Average Monthly Income: 6.50K
# Analysis Performed
• Attrition by Department
• Attrition by Job Role
• Attrition by Age Group
• Attrition by Gender
• Attrition by Job Satisfaction
• Attrition by Salary Group
• Attrition by Work-Life Balance
• Attrition by Overtime
• Employee Performance and Promotion Analysis

# Dashboard
<img width="965" height="545" alt="hr dashboard(40)" src="https://github.com/user-attachments/assets/e59782e9-91ef-4bd0-be19-d0835060407d" />

# Key Business Insights
• Young employees have the highest attrition rate.
• Employees working overtime are more likely to leave.
• Low job satisfaction is strongly associated with higher attrition.
• Employees in lower salary groups show higher turnover.
• Specific job roles require targeted retention strategies.
# Business Recommendations
• Improve work-life balance for high-risk employee groups.
• Review overtime policies.
• Create better career growth and promotion opportunities.
• Improve compensation for low-salary employee groups.
• Conduct targeted employee engagement programs.
