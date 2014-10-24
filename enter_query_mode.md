# QAFE Enter Query Mode How-to #

enter-query-mode which is often used as a mechanism for searching for
master-detail blocks within an Oracle Forms application.

The purpose of this document is to help you achieve a similar functionality within
QAFE.

A basic understanding of QAFE is assumed throughout this tutorial.

## The Oracle Form ##

The Oracle Form used for this example has the following properties;

Form Name: HRFORM1  
Block Names : DEPARTMENTS, EMPLOYEES  
Relation : EMPLOYEES.DEPARTMENT_ID = DEPARTMENTS.DEPARTMENT_ID

Details for the tables corresponding to the blocks are as follows:
```sql
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
```

With this in mind, we can convert out application to QAFE.

## Result after QAFE Conversion ##

After converting our form, both blocks, including their statements
are converted to populate the data.

A UI similar to the example below will be generated.

The blocks EMPLOYEES and DEPARTMENTS are converted to a datagrid with id's
HRFORM1_EMPLOYEES_ID and HRFORM1_DDEPARTMENTS_ID respectively.
```xml
<panel>
	<verticallayout>
		<datagrid id="HRFORM1_DEPARTMENTS_ID" .....></datagrid>
	</verticallayout>
</panel>

<panel>
	<verticallayout>
		<datagrid id="HRFORM1_EMPLOYEES_ID" ...></datagrid>
	</verticallayout>
</panel>
```

After running the application, we can verify that the relation is converted correctly
by clicking on the master (departments). This will result in employees being updated.

The converted statements should look like the following.

```xml
<select id="HRFORM1_DEPARTMENTS_SELECT_BASETYPE">
	<![CDATA[SELECT * FROM DEPARTMENTS]]>
</select>
<select id="HRFORM1_EMPLOYEES_SELECT_DETAIL"><![CDATA[
	SELECT * FROM EMPLOYEES
	WHERE  EMPLOYEES.DEPARTMENT_ID = :DEPARTMENT_ID
]]></select>
```
Note that the generated id for the master is based on the format
[formname]\_[blockname]\_SELECT\_BASETYPE and the generated id for the detail is
based on [formname]\_[blockname]\_SELECT\_DETAIL

These translate to the following business-actions and their method definitions
In the business-tier.

The format of the generated business-tier file is [formname]-business-tier.qaml.
```xml
<business-action id="HRFORM1_DEPARTMENTS_SELECT_BASETYPE">
	<transaction managed="no" />
	<service ref="hrXE" method-ref="HRFORM1_DEPARTMENTS_SELECT_BASETYPE">
		<out name="result" ref="result" />
	</service>
</business-action>
<business-action id="HRFORM1_EMPLOYEES_SELECT_DETAIL">
	<transaction managed="no" />
	<service ref="hrXE" method-ref="HRFORM1_EMPLOYEES_SELECT_DETAIL">
		<in name="DEPARTMENT_ID" ref="DEPARTMENT_ID" />
		<in name="employeeIdSearch" ref="employeeIdSearch" />
		<in name="employeeFirstNameSearch" ref="employeeFirstNameSearch" />
		<out name="result" ref="result" />
	</service>
</business-action>
```

Their corresponding methods can be found in the integration-tier:

The format for this file is [formname]-integration-tier.qaml.
```xml
<method id="HRFORM1_DEPARTMENTS_SELECT_BASETYPE"
			name="HRFORM1_DEPARTMENTS_SELECT_BASETYPE"
			scrollable="true">
	<out name="result" />
</method>
<method id="HRFORM1_EMPLOYEES_SELECT_DETAIL"
			name="HRFORM1_EMPLOYEES_SELECT_DETAIL"
			scrollable="true">
	<in name="DEPARTMENT_ID" ref="DEPARTMENT_ID" />
	<out name="result" />
</method>
```

We can see that the methods don't have any input for handling enter query mode.
When we scan through the code, we can verify that neither search nor
enter-query-mode is handled.

## Implementing enter-query-mode the QAFE way ##
Implementing enter-query-mode in QAFE can be broken down in 3 parts;

* Adding search support to the view
* Modifying the queries
* Sticking it all together: Events

### Adding search support to the view ###

To support enter-search-query like search, we need to add several components.
We prefer to use a separate panel to hold the search fields. Based on keyboard input,
we will show and hide this panel.

Add a new panel above the data grid definition for both master and detail;
```xml
<panel>
	<verticallayout>
		<panel visible="true" id="departmentSearchPanel">
			<horizontallayout>
				<textfield id="departmentNameSearch" displayname="Department Name" />
			</horizontallayout>
		</panel>
		<datagrid id="HRFORM1_DEPARTMENTS_ID" .....></datagrid>
	</verticallayout>
</panel>

<panel>
	<verticallayout>
		<panel visible="false" id="employeeSearchPanel">
			<horizontallayout>
				<textfield id="employeeIdSearch" displayname="Employee Id" />
				<textfield id="employeeFirstNameSearch" displayname="Employee First Name" />
			</horizontallayout>
		</panel>
		<datagrid id="HRFORM1_EMPLOYEES_ID" ...></datagrid>
	</verticallayout>
</panel>
```
Note: Set the panel's visible attribute to true if you want to see it by default.

When enabled, the application should look something like:

![Layout](https://raw.githubusercontent.com/qafedev/qafedev.github.io/master/assets/images/enter_query_mode_app_2.png)

### Modifying the queries ###

Now that the UI is finished, we can focus on enabling the queries for search.
After that, we can connect the two, using events as the glue.

Adding search criteria to the queries is simple enough.

```sql
<select id="HRFORM1_DEPARTMENTS_SELECT_BASETYPE"><![CDATA[
	select * from DEPARTMENTS
	WHERE upper(DEPARTMENT_NAME) like '%' || upper(:departmentNameSearch) || '%'
	]]></select>
<select id="HRFORM1_EMPLOYEES_SELECT_DETAIL"><![CDATA[
	select * from EMPLOYEES
	where  EMPLOYEES.DEPARTMENT_ID = :DEPARTMENT_ID
	and upper(EMPLOYEE_ID) like '%' || upper(:employeeIdSearch) || '%'
	and upper(FIRST_NAME) like '%' || upper(:employeeFirstNameSearch) || '%'
	]]></select>
```

As we can see, we added a LIKE clause to our statement, and we surround
the left and right criteria with the upper method. This will help us achieve
case-insensitive search.

We add a LIKE clause to all columns we want to search on.

Now that we covered the queries, we need to update the methods and business-actions
to add the new input parameters corresponding to the data entered in the search fields.

business-tier
```xml
<business-action id="HRFORM1_DEPARTMENTS_SELECT_BASETYPE">
	<transaction managed="no" />
	<service ref="hrXE" method-ref="HRFORM1_DEPARTMENTS_SELECT_BASETYPE">
		<in name="departmentNameSearch" ref="departmentNameSearch" />
		<out name="result" ref="result" />
	</service>
</business-action>
<business-action id="HRFORM1_EMPLOYEES_SELECT_DETAIL">
	<transaction managed="no" />
	<service ref="hrXE" method-ref="HRFORM1_EMPLOYEES_SELECT_DETAIL">
		<in name="DEPARTMENT_ID" ref="DEPARTMENT_ID" />
		<in name="employeeIdSearch" ref="employeeIdSearch" />
		<in name="employeeFirstNameSearch" ref="employeeFirstNameSearch" />
		<out name="result" ref="result" />
	</service>
</business-action>
```

intergration-tier
```xml
<method id="HRFORM1_DEPARTMENTS_SELECT_BASETYPE" name="HRFORM1_DEPARTMENTS_SELECT_BASETYPE">
	<in name="departmentNameSearch" ref="departmentNameSearch" />
	<out name="result" />
</method>
<method id="HRFORM1_EMPLOYEES_SELECT_DETAIL" name="HRFORM1_EMPLOYEES_SELECT_DETAIL">
	<in name="DEPARTMENT_ID" ref="DEPARTMENT_ID" />
	<in name="employeeIdSearch" ref="employeeIdSearch" />
	<in name="employeeFirstNameSearch" ref="employeeFirstNameSearch" />
	<out name="result" />
</method>
```

This is all that needs to be done for supporting enter-query-mode based search
on the query.

Next up, the glue that sticks it all together.

### Sticking it all together: Events ###

Before defining the events, here are some handy shortcuts for you to know;

* Pressing F7 when focused on one of the grids opens the corresponding
search panel
* Pressing F8 fires the queries to fetch data based on search criteria
and update both the master and detail
* Pressing Shift + F9 will both hide the panel, and do a select without search
criteria

I'll show how to deal with this based on the master, because the detail is
easier to deal with.

Handling F7:

```xml
<event>
	<listeners>
		<listenergroup>
			<component ref="HRFORM1_DEPARTMENTS_ID.DEPARTMENT_ID" />
			<component ref="HRFORM1_DEPARTMENTS_ID.DEPARTMENT_NAME" />
			<listener type="onkeydown">
				<listener-parameter name="key" value="F7" />
			</listener>
		</listenergroup>
	</listeners>
	<set-property property="visible" value="true">
		<component ref="departmentSearchPanel" />
	</set-property>
	<focus ref="departmentNameSearch" />
</event>
```

In the component ref, the pattern is [datagrid column id].[database column]
As a listener, we define a type "onkeydown" which handles keyboard in-put.
In the listener parameter, we define what key we are talking about. F7 in this case.

You'll notice this pattern will be the same for most coming events.

In the body (coming after the listeners section) we set the visibility of a component to true
The component in this case being a panel (and all its child components).

We then set keyboard focus on the departmentNameSearch textbox.

For the sake of re-usability, we moved the code that handles the actual
loading of the departments and employees to separate events "loadDepartments"
and "loadEmployees".

Handling F8:

```xml
<event>
	<listeners>
		<listenergroup>
			<component ref="HRFORM1_DEPARTMENTS_ID.DEPARTMENT_ID" />
			<component ref="HRFORM1_DEPARTMENTS_ID.DEPARTMENT_NAME" />
			<component ref="departmentNameSearch" />
			<listener type="onkeydown">
				<listener-parameter name="key" value="F8" />
			</listener>
		</listenergroup>
		<listenergroup>
			<component ref="departmentNameSearch" />
			<listener type="onenter" />
		</listenergroup>
	</listeners>
	<store name="departmentNameSearch" ref="departmentNameSearch"
		src="component" />
	<event ref="loadDepartments" />
</event>
```

The logic behind the listeners is the same, the main change is that we now listen
to F8, and only when either the department_id or department_name column is
selected, or departmentNameSearch component has focus.

We then store the value of the departmentNameSearch component in a variable

loadDepartments event:

```xml
<event id="loadDepartments">
	<business-action ref="HRFORM1_DEPARTMENTS_SELECT_BASETYPE">
		<in name="departmentNameSearch" ref="departmentNameSearch" />
		<out name="selectDepartmentsResult" ref="result" />
	</business-action>
	<set component-id="HRFORM1_DEPARTMENTS_ID" ref="selectDepartmentsResult" />

	<store name="selectedDepartmentId" ref="selectDepartmentsResult[0].DEPARTMENT_ID" />
	<store name="employeeIdSearch" ref="employeeIdSearch" src="component" />
	<store name="employeeFirstNameSearch" ref="employeeFirstNameSearch"
		src="component" />
	<event ref="loadEmployees" />
</event>
```

First, we execute a business-action, which retrieves all departments if
departmentNameSearch is empty. If departmentNameSearch is not empty;
it will be used to filter data. Thus, we only get the departments based on the search criteria.

Then, we set the data to data grid using the "set" built-in. Populate the employees,
also taking search criteria entered in that panel into account.

In case there is no data in the employee search panel, the query
still executes, it just fetches everything based on the selected department.

Lastly, we call the loadEmployees event.

```xml
<event id="loadEmployees">
	<business-action ref="HRFORM1_EMPLOYEES_SELECT_DETAIL">
		<in name="DEPARTMENT_ID" ref="selectedDepartmentId" />
		<in name="employeeIdSearch" ref="employeeIdSearch" />
		<in name="employeeFirstNameSearch" ref="employeeFirstNameSearch" />
		<out name="selectEmployeesResult" ref="result" />
	</business-action>
	<set component-id="HRFORM1_EMPLOYEES_ID" ref="result" />
</event>
```

The loadEmployees event is mostly similar to what we have done before.
We execute a business action, passing collected data (remember that we
	stored the selectedDepartmentId before using the store built-in) as input parameters.

We then set the result of the query (stored in selectEmployeesResult) to the datagrid,

Only one thing left. Hiding the panels when we don't need them anymore.

```xml
<event>
	<listeners>
		<listenergroup>
			<component ref="HRFORM1_DEPARTMENTS_ID.DEPARTMENT_ID" />
			<component ref="HRFORM1_DEPARTMENTS_ID.DEPARTMENT_NAME" />
			<component ref="departmentNameSearch" />
			<listener type="onkeydown">
				<listener-parameter name="key" value="KEY_SHIFT + F9" />
			</listener>
		</listenergroup>
	</listeners>
	<clear ref="departmentSearchPanel" />
	<set-property property="visible" value="false">
		<component ref="departmentSearchPanel" />
	</set-property>
	<store name="departmentNameSearch" value="" />
	<event ref="loadDepartments" />
</event>
```

One again, the listener definition is mostly the same, except we change the value to
"KEY_SHIFT + F9".

The "clear" built-in removes the data of all components inside the panel,
the panel is then hidden, by setting its visible property to false.

Clearing is done to reset the search panel; meaning that it clears the value of all components
inside the referred panel

This makes sure that, when calling loadDepartments, all departments are fetched without any filters applied.

With the glue hardened, the views in place and the queries all set up;
we have achieved enter-query-mode like behavior in QAFE.
