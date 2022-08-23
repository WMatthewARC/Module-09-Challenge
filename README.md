# Module-09-Challenge | sql-challenge

## Summary
I was asked to design a SQL DB from provided CSV files then run some statments to anwer given questions. 


## Data Modeling

Developed an ERD to assist in this task:

![ERD](DB_layout.JPG)


CREATE TABLE titles(
    title_id VARCHAR PRIMARY KEY,
    title VARCHAR
);

--SELECT * FROM titles; 

CREATE TABLE employees ( 
    emp_no INT PRIMARY KEY,
    emp_title_id VARCHAR,
    birth_date DATE,
    frist_name VARCHAR,
    last_name VARCHAR,
    sex VARCHAR,
    hire_date DATE,
    FOREIGN KEY (emp_title_id) REFERENCES titles (title_id)
);

--SELECT * FROM employees; 

CREATE TABLE departments ( 
    dept_no VARCHAR PRIMARY KEY,
    dept_name VARCHAR
);

--SELECT * FROM departments; 


CREATE TABLE dept_manager ( 
    dept_no VARCHAR,
    emp_no INT,
    FOREIGN KEY (emp_no) REFERENCES employees (emp_no),
    FOREIGN KEY (dept_no) REFERENCES departments (dept_no),
    PRIMARY KEY (emp_no, dept_no)
);

--SELECT * FROM dept_manager; 

CREATE TABLE dept_emp ( 
    emp_no INT,
    dept_no VARCHAR,
    FOREIGN KEY (emp_no) REFERENCES employees (emp_no),
    FOREIGN KEY (dept_no) REFERENCES departments (dept_no),
    PRIMARY KEY (emp_no, dept_no)
);

--SELECT * FROM dept_emp; 

CREATE TABLE salaries ( 
    emp_no INT PRIMARY KEY,
    salary INT,
    FOREIGN KEY (emp_no) REFERENCES employees (emp_no)
);

--SELECT * FROM salaries;


    --~`Setup is complete!
--
--
--
    --~`Starting on questions
--
--1. List the employee number, last name, first name, sex, and salary of each employee.

SELECT employees.emp_no, employees.last_name, employees.frist_name, employees.sex, salaries.salary
From employees
LEFT JOIN salaries
ON employees.emp_no = salaries.emp_no
ORDER BY employees.emp_no;


--2. List the first name, last name, and hire date for the employees who were hired in 1986.

SELECT Frist_name, last_name, hire_date
FROM employees
WHERE hire_date BETWEEN '1986-01-01' AND '1986-12-31'
ORDER BY hire_date;


--3. List the manager of each department along with their department number, department name, employee number, last name, and first name.

SELECT dept_manager.dept_no, departments.dept_name, employees.emp_no, employees.last_name, employees.frist_name
FROM dept_manager
INNER JOIN departments
ON dept_manager.dept_no = departments.dept_no
INNER JOIN employees
ON employees.emp_no = dept_manager.emp_no
ORDER BY dept_name;


--4. List the department number for each employee along with that employee’s employee number, last name, first name, and department name.

SELECT departments.dept_no, departments.dept_name, employees.emp_no, employees.last_name, employees.frist_name
FROM employees
INNER JOIN dept_emp
ON employees.emp_no = dept_emp.emp_no
INNER JOIN departments 
ON departments.dept_no = dept_emp.dept_no
ORDER BY departments.dept_no;


--5. List first name, last name, and sex of each employee whose first name is Hercules and whose last name begins with the letter B.

SELECT frist_name, last_name, sex
FROM employees 
WHERE frist_name = 'Hercules'
AND last_name LIKE 'B%';


--6. List each employee in the Sales department, including their employee number, last name, and first name.

SELECT departments.dept_name, employees.emp_no, employees.last_name, employees.frist_name
FROM employees
INNER JOIN dept_emp
ON employees.emp_no = dept_emp.emp_no
INNER JOIN departments 
ON departments.dept_no = dept_emp.dept_no
WHERE departments.dept_name = 'Sales'
ORDER BY employees.last_name;


--7. List each employee in the Sales and Development departments, including their employee number, last name, first name, and department name.

SELECT departments.dept_name, employees.emp_no, employees.last_name, employees.frist_name
FROM employees
INNER JOIN dept_emp
ON employees.emp_no = dept_emp.emp_no
INNER JOIN departments 
ON departments.dept_no = dept_emp.dept_no
WHERE departments.dept_name = 'Sales' OR departments.dept_name = 'Development'
ORDER BY employees.last_name;

--8. List the frequency counts, in descending order, of all the employee last names (that is, how many employees share each last name).   

SELECT last_name, COUNT(last_name)
FROM employees
GROUP BY last_name
ORDER BY COUNT(last_name)
DESC;
