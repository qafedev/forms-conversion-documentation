# QAFE Enter Query Mode How-to #

This document will help you to properly implement **enter Query Mode** in QAFE.<br />
Basic knowledge of QAFE is needed to understand this tutorial.

## Setting up a database ##

Feel free to use any type of database. For this example we assume an Oracle database.

Form details:

Form name is "HRFORM1"
2 blocks called "DEPARTMENTS" and "EMPLOYEES"
1 relation between "DEPARTMENTS" and "EMPLOYEES" where one department and have many employees and one employee can be part of one department

This example uses the following tables:

	CREATE TABLE EMPLOYEES (
		EMPLOYEE_ID NUMBER(6,0), 
		FIRST_NAME VARCHAR2(20 BYTE), 
		LAST_NAME VARCHAR2(25 BYTE), 
		EMAIL VARCHAR2(25 BYTE), 
		PHONE_NUMBER VARCHAR2(20 BYTE), 
		SALARY NUMBER(8,2), 
		DEPARTMENT_ID NUMBER(4,0)
    );

	CREATE TABLE DEPARTMENTS (	
    	DEPARTMENT_ID NUMBER(4,0), 
    	DEPARTMENT_NAME VARCHAR2(30 BYTE), 
    );

In the employees table the following constraints are applied

* employee_id in the employees table is a primary key
* email in the employees table is unique
* department_id in the employees table is a foreign key based on the primary key in departments
* department_id in the departments table is a primary key

For the next section, we assume you have already converted an Oracle Forms application.

## After conversion ##
For the remaining of the tutorial, we assume the following image is the result after a conversion.

![Layout](https://raw.githubusercontent.com/qafedev/qafedev.github.io/master/assets/images/enter_query_mode/enter_query_mode_app_1.png)

Here is a short overview of what you see in the image

- A grid, filled with data based on the departments table
- A grid, filled with data based on the employees table
- A master-detail relation between the two tables. 

To verify that the master-detail relation is working as expected, select a record on the master and the detail should get updated. This works out of the box, because the conversion generates these queries based on relations defined in a block.

Here is a short example of what the converted query will look like in case of our example:

    <select id="HRFORM1_DEPARTMENTS_SELECT_BASETYPE"><![CDATA[SELECT * FROM DEPARTMENTS]]></select>
	<select id="HRFORM1_EMPLOYEES_SELECT_DETAIL"><![CDATA[
		SELECT * FROM EMPLOYEES 
		WHERE  EMPLOYEES.DEPARTMENT_ID = :DEPARTMENT_ID
	]]></select>

Note that the generated id for the master is based on the format [formname]\_[blockname]\_SELECT\_BASETYPE and the generated id for the detail is based on [formname]\_[blockname]\_SELECT\_DETAIL












    
    





