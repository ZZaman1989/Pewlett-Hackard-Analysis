# Pewlett-Hackard-Analysis

## Overview of Project
> Now that Bobby has proven his SQL chops, his manager has given both of you two more assignments: determine the number of retiring employees per title, and identify employees who are eligible to participate in a mentorship program. Then, you’ll write a report that summarizes your analysis and helps prepare Bobby’s manager for the “silver tsunami” as many current employees reach retirement age. 

1. ***Deliverable 1***: The Number of Retiring Employees by Title
2. ***Deliverable 2***: The Employees Eligible for the Mentorship Program
3. ***Deliverable 3***: A written report on the employee database analysis 

## Deliverable 1:  The Number of Retiring Employees by Title
### Deliverable Requirements:
Using the ERD you created in this module as a reference and your knowledge of SQL queries, create a Retirement Titles table that holds all the titles of employees who were born between January 1, 1952 and December 31, 1955. Because some employees may have multiple titles in the database—for example, due to promotions—you’ll need to use the DISTINCT ON statement to create a table that contains the most recent title of each employee. Then, use the COUNT() function to create a table that has the number of retirement-age employees by most recent job title. Finally, because we want to include only current employees in our analysis, be sure to exclude those employees who have already left the company.

1. A query is written and executed to create a Retirement Titles table for employees who are born between January 1, 1952 and December 31, 1955 
2. The Retirement Titles table is exported as `retirement_titles.csv`
3. ​A query is written and executed to create a Unique Titles table that contains the employee number, first and last name, and most recent title.
4. The Unique Titles table is exported as `unique_titles.csv`  
5. A query is written and executed to create a Retiring Titles table that contains the number of titles filled by employees who are retiring. 
6. The Retiring Titles table is exported as `retiring_titles.csv`

 
### Results with detail analysis:

1. **A query is written and executed to create a Retirement Titles table for employees who are born between January 1, 1952 and December 31, 1955.**

> Image with `SQL` Code below.

**Code**

````SQL
-- Follow the instructions below to complete Deliverable 1.
SELECT e.emp_no,
       e.first_name,
       e.last_name,
       t.title,
       t.from_date,
       t.to_date
INTO retirement_titles
FROM employees as e
INNER JOIN titles as t
ON (e.emp_no = t.emp_no)
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
order by e.emp_no;
````

2. **The Retirement Titles table is exported as `retirement_titles.csv`**

> `retirement_titles.csv` Image below.

**Image**

![name-of-you-image](https://github.com/ZZaman1989/Pewlett-Hackard-Analysis/blob/main/Resources/Retirement_Titles.png)

3. ​***A query is written and executed to create a Unique Titles table that contains the employee number, first and last name, and most recent title.**

> Image with `SQL` Code below.

**Code**

````SQL
-- Use Dictinct with Orderby to remove duplicate rows
SELECT DISTINCT ON (emp_no) emp_no,
    first_name,
    last_name,
    title
INTO unique_titles
FROM retirement_titles
WHERE to_date = ('9999-01-01')
ORDER BY emp_no, title DESC;
````

4. **The Unique Titles table is exported as `unique_titles.csv`**

> `unique_titles.csv` Image below.

**Image**

![name-of-you-image](https://github.com/ZZaman1989/Pewlett-Hackard-Analysis/blob/main/Resources/Unique_Titles.png)

5. **A query is written and executed to create a Retiring Titles table that contains the number of titles filled by employees who are retiring.**

> Image with `SQL` Code below.

**Code**

````SQL
-- Retrieve the number of employees by their most recent job title who are about to retire.
SELECT COUNT(ut.emp_no),
ut.title
INTO retiring_titles
FROM unique_titles as ut
GROUP BY title 
ORDER BY emp_no DESC;
````

6. **The Retiring Titles table is exported as `retiring_titles.csv`**

> `retiring_titles.csv` Image below.

**Image**

![name-of-you-image](https://github.com/ZZaman1989/Pewlett-Hackard-Analysis/blob/main/Resources/Retiring_Titles.png)



## Deliverable 2: The Employees Eligible for the Mentorship Program
### Deliverable Requirements:
Using the ERD you created in this module as a reference and your knowledge of SQL queries, create a mentorship-eligibility table that holds the current employees who were born between January 1, 1965 and December 31, 1965.

1. A query is written and executed to create a Mentorship Eligibility table for current employees who were born between January 1, 1965 and December 31, 1965.
2. The Mentorship Eligibility table is exported and saved as `mentorship_eligibilty.csv`


### Results with detail analysis:

1. **A query is written and executed to create a Mentorship Eligibility table for current employees who were born between January 1, 1965 and December 31, 1965.**

> Image with `SQL` Code below.

**Code**


````SQL
-- Write a query to create a Mentorship Eligibility table that holds the employees who are eligible to participate in a mentorship program.
SELECT DISTINCT ON(e.emp_no) e.emp_no, 
    e.first_name, 
    e.last_name, 
    e.birth_date,
    de.from_date,
    de.to_date,
    t.title
INTO mentorship_eligibilty
FROM employees as e
Inner Join dept_emp as de
ON (e.emp_no = de.emp_no)
Inner Join titles as t
ON (e.emp_no = t.emp_no)
WHERE de.to_date = ('9999-01-01')
AND (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
ORDER BY e.emp_no;
````

2. **The Mentorship Eligibility table is exported and saved as `mentorship_eligibilty.csv`"**

> Exported `mentorship_eligibilty.csv` Image below.

**Image**

![name-of-you-image](https://github.com/ZZaman1989/Pewlett-Hackard-Analysis/blob/main/Resources/mentorship_elgibility.png)



## Deliverable 3: A written report on the employee database analysis
### The analysis should contain the following:

1. **Overview of the analysis** 
* Explain the purpose of this analysis.:

    > The purpose of this analysis was to understand the demographics of retirement ready or soon-to-be retirement ready employees, and identify employees who are eligible to participate in a mentorship program. Then, write a report that summarizes our analysis and helps prepare the manager for the “silver tsunami” as many current employees reach retirement age.


2. **Results** 
* Provide a bulleted list with four major points from the two analysis deliverables. Use images as support where needed:

- Pewlett Hackard has 72,459 employees that were born beteween 1952 and 1955.

- Senior Engineers and Senior Staff are the largest population closest to retirement.

- There are not enough staff to help fill the retirement void.

- In order to prepare for "the silver tsunami", Pewlett Hackard is interested in starting a mentorship (succession planning) program of employees that have experience. With the mentorship program, this could help replace those who are retiring without losing all the knowledge. Pewlett Hackard has a pool of 1,549 employees to utilize as mentees in the mentorship program (mentorship_eligibility.csv)

3. **Summary** 
* Provide high-level responses to the following questions, then provide two additional queries or tables that may provide more insight into the upcoming "silver tsunami.":

    > **1)** How many roles will need to be filled as the "silver tsunami" begins to make an impact?

     There are 72,458 employess who will be retiring soon. The exact list of retirees can be referenced in the unique_titles.csv. 
     
    > **2)** Are there enough qualified, retirement-ready employees in the departments to mentor the next generation of Pewlett Hackard employees?  

    No, we have 1,549 employees who are eligible to participate in a mentorship program. 

