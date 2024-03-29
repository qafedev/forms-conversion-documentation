#QAFE Forms Conversion


<!-- toc -->

* [1. Automatic conversion using the wizard](#1-automatic-conversion-using-the-wizard)
  * [1.1 Setting up the QAFE Forms Conversion Environment](#11-setting-up-the-qafe-forms-conversion-environment)
  * [1.2 Files generated after conversion](#12-files-generated-after-conversion)
* [2. What to do with converted files](#2-what-to-do-with-converted-files)
  * [2.2 Overview of the Windows, Events, BusinessActions etc Generated](#22-overview-of-the-windows-events-businessactions-etc-generated)
  * [2.2 Overview of generated SQL scripts](#22-overview-of-generated-sql-scripts)
    * [2.2.1 Run the generated script file containing the Object Types creation.](#221-run-the-generated-script-file-containing-the-object-types-creation)
    * [2.2.1 Check the converted script file.](#221-check-the-converted-script-file)
* [3. UI Changes](#3-ui-changes)
* [4. Form Trigger Conversion](#4-form-trigger-conversion)
  * [Trigger: WHEN-NEW-FORM-INSTANCE](#trigger-when-new-form-instance)
    * [Scenario : When opening the Form data needs to be populated to a block](#scenario-when-opening-the-form-data-needs-to-be-populated-to-a-block)
  * [Trigger: WHEN-NEW-FORM-INSTANCE](#trigger-when-new-form-instance)
    * [Scenario : When opening the Form data is populated to List Items(drop-down) using a Program Unit](#scenario-when-opening-the-form-data-is-populated-to-list-itemsdrop-down-using-a-program-unit)
* [5. Database Block Conversion (For each Block)](#5-database-block-conversion-for-each-block)
  * [5.1 Populate Data to Block.](#51-populate-data-to-block)
  * [5.2 Implement Block Level Triggers](#52-implement-block-level-triggers)
    * [Trigger: POST-QUERY](#trigger-post-query)
  * [5.3 Implement INSERT / UPDATE / DELETE operations.](#53-implement-insert-update-delete-operations)
    * [Scenario : Save modified records of multi-record block](#scenario-save-modified-records-of-multi-record-block)
    * [5.3.1 PRE-UPDATE trigger](#531-pre-update-trigger)
    * [Scenario : Update additional data other than the block definition](#scenario-update-additional-data-other-than-the-block-definition)
  * [5.4 Handle Master-Detail Relations](#54-handle-master-detail-relations)
* [6. Item Trigger Handling](#6-item-trigger-handling)
  * [6.1 Opening other windows](#61-opening-other-windows)
* [7.Styling](#7styling)
  * [7.1 Direct styling using the Style-tag](#71-direct-styling-using-the-style-tag)
  * [7.2 Styling using a CSS stylesheet](#72-styling-using-a-css-stylesheet)
* [8. General Tips](#8-general-tips)
  * [8.1 Items to Ignore:](#81-items-to-ignore)
  * [8.2  How to find a SQL Query used in an Event](#82-how-to-find-a-sql-query-used-in-an-event)
  * [8.3 How QAFE detects the Main Window in FMB](#83-how-qafe-detects-the-main-window-in-fmb)
  * [8.4 Changing the length of a column in database](#84-changing-the-length-of-a-column-in-database)

<!-- toc stop -->


This document explains steps to follow to convert an Oracle Forms to a fully functional QAFE application.

This document is meant for QAFE Forms Conversion users with knowledge of Oracle Forms and QAFE.

## 1. Automatic conversion using the wizard

### 1.1 Setting up the QAFE Forms Conversion Environment
QAFE Forms conversion can be done using QAFE Conversion Wizard.

Check [QAFE site](http://www.qafe.com) for details about how to download and set up the QAML Builder environment for Forms Conversion.

Check [here](http://www.qafe.com/developer-docs/oracle-forms/qafe-forms-wizard/) for details about how to use the Forms Conversion Wizard.
### 1.2 Files generated after conversion
**Prerequisite**

There should be clear understanding of files generated after forms conversion and how the conversion works.

Please refer these documents.

[Structure of generated files](http://www.qafe.com/developer-docs/oracle-forms/code-walkthrough/).

[Terminology Oracle Forms to QAFE](http://www.qafe.com/developer-docs/oracle-forms/terminology-oracle-forms-to-qafe-forms/)

[Abbreviations used in conversion process](FormsTriggersAbbreviations.md).

**Other Reference Documents:**

[Re-using Forms Built-ins](FormsReusableBuiltins.md).

[Converting Forms Triggers to QAFE](FormsTriggersToQAFEConversion.md).

[Forms Triggers to QAFE Listeners](FormsTriggersToQAFEListner.md).

## 2. What to do with converted files

### 2.2 Overview of the Windows, Events, BusinessActions etc Generated
Before you start, have a clear overview of where the Windows, Events, BusinessActions, etc. are located in the QAML output.
Check the conversion output of Windows in QAML Builder and compare with the original application.
Run the converted application and check the UI.

### 2.2 Overview of generated SQL scripts
Check the generated sql scripts in the database, located in a folder with name of the FMB converted under src/main/script folder.

Note that in the scrips folder, a MAIN and STUBS script is generated. 
The main difference is that STUBS only has empty bodies of the triggers and programming units of your form,
while the main script has most of its logic converted. 

The reason we supply both scripts is that in some cases, logic can not be converted, due to pl/sql not having a way to implement some functionality, or any other such reason. This might give problems when trying to compile the script in the database. In this case, you could consider to use the stubs script, and use the main script as reference to implement the bodies.

#### 2.2.1 Run the generated script file containing the Object Types creation.

#### 2.2.1 Check the converted script file.
Check the converted script file and see how much logic is in the triggers and how much we can reuse.
Individual procedures in this script can be executed in database as needed.

Since master-detail is handled differently in QAFE, all its related trigger code is placed in MASTER_DETAIL_Package_Script_FORMNAME.txt. Running this script file is thus not nescesary. For more information, please refer to the following document: [Master-Detail database script](FormsMasterDetailBlocksToQAFEConversion.md).

## 3. UI Changes
Open the FMB in the Forms Builder.
Open the QAML file in QAML Builder and Re-position the UI components using QAML Builder.

## 4. Form Trigger Conversion
Check the Form level trigger(PRE_FORM, WHEN-NEW-FORM-INSTANCE ) code and implement the same in QAFE.

Check code in this triggers to understand the functionality. This trigger code can call other Program-Unit(s). Check all code involved.
Re-use the logic using database procedures or split and implement it in QAFE.

### Trigger: WHEN-NEW-FORM-INSTANCE
#### Scenario : When opening the Form data needs to be populated to a block

**Trigger Code:**
```
go_block('BLOCK1');
execute_query;
```
**Functionality Handled:**

Populate the data to BLOCK1 based on the Query Data Source Name, WHERE Clause and ORDER BY Clause of the block.

**QAFE Conversion:**

QAFE equivalent of WHEN-NEW-FORM-INSTANCE trigger is the onload listener of the Main Window in the Form.

Open the QAFE Window equivalent of the main Window in FMB.

[*How QAFE detects the Main Window in FMB:*](#main_window)

- QAFE conversion checks the "Console Window" property of the Form. This is considered as main window.
- If "Console Window" property is not mentioned the first Window in the conversion process is considered as main window.

Assume for the form Console Window is mentioned as WINDOW1 for the Form FORM1

Oracle Form WINDOW1 will be converted to QAFE Window with id FORM1_WINDOW1

FORM1_WINDOW1 can be found in the file FORM1-presentation-tier-FORM1_WINDOW1.qaml

Check onload event of FORM1_WINDOW1.

When trigger conversion enabled option is selected in the Conversion Wizard, onload event will be calling an event that handles the WHEN-NEW-FORM-INSTANCE using database package. Database package is useful when you have lot of logic or database interaction in the trigger.
In this case the functionality is just to populate the block. We can do this using QAFE artifacts without using database packages. This can be handled by using QAFE Built-ins ```<business-action>``` and ```<set>```.

Comment or remove the following line from the onload event if exist.
```<event ref="FORM1_FORMTRIGGER_WHEN_NEW_FORM_INSTANCE"/>```

We can also remove the event with id FORM1_FORMTRIGGER_WHEN_NEW_FORM_INSTANCE and all the other layers(business-action, integration-tier method, call statement entry) after implementing same functionality in QAFE.

As part of conversion the code to populate data to the main block (First database block in the Window) is generated in onload event.
In that case you will see a ```<business-action>``` call in the event and after that ```<set>``` built-in is used to set data to user interface.

During the conversion when it detects that BLOCK1 is the main block of the window the following code will be generated to populate the data to the QAFE components equivalent of the BLOCK1.

(*Add this code in onload event if it is not generated as part of conversion*)

```xml
  <business-action ref="FORM1_BLOCK1_SELECT_BASETYPE">
      <out name="BLOCK1_SELECT_BASETYPEOut" ref="BLOCK1_SELECT_BASETYPEOut"/>
  </business-action>
  <set component-id="FORM1_BLOCK1_ID" ref="BLOCK1_SELECT_BASETYPEOut"/>  
```
FORM1_BLOCK1_ID is the ID of the QAFE data-grid equivalent to BLOCK1.  
FORM1_BLOCK1_SELECT_BASETYPE is the business-action to select data from database for BLOCK1.  
FORM1_BLOCK1_SELECT_BASETYPE will execute SQL statement created based on BLOCK1's Query Data Source Name, WHERE Clause and ORDER BY Clause.  

- When BLOCK1 is multi-record block, the equivalent QAFE component generated is a data-grid. We can use ID of the datagrid to set data to it using ```<set component-id="FORM1_BLOCK1_ID"```.  
- When BLOCK1 is single-record block, each item in block will be converted to QAFE UI components with same  
group-name (FORM1_BLOCK1_ALL) corresponding to the block. So we can use ```<set group-name="FORM1_BLOCK1_ALL">``` to set data to user interface.


Also check the document [Forms Builit-In Handling](FormsTriggersToQAFEConversion.md#trigger_block_management) for more details.


### Trigger: WHEN-NEW-FORM-INSTANCE
#### Scenario : When opening the Form data is populated to List Items(drop-down) using a Program Unit

**Trigger Code:**
```
  populate_modifiers_list;
```
populate_modifiers_list is a Program Unit.

```sql
  PROCEDURE populate_modifiers_list
    IS
      CURSOR c1_user
      IS
        SELECT DISTINCT u.user_name ,
          TO_CHAR(u.user_id) user_id
        FROM fr_users u,
          mentc c
        WHERE u.user_id = c.last_updated_by
        ORDER BY user_name,
          user_id;
    BEGIN
      clear_list('bl_control.pl_select_modifier');
      add_list_element('bl_control.pl_select_modifier', 1, 'All Users','%' );
      FOR c1_rec IN c1_user
      LOOP
        add_list_element('bl_control.pl_select_modifier', c1_user%rowcount, c1_rec.user_name,c1_rec.user_id );
      END LOOP;
    EXCEPTION
    WHEN OTHERS THEN
      :bl_control.messages := ('Unable to Populate Modifier List, '||sqlerrm||', Please contact Technical Support');
  END;
```

**Functionality Handled:**

When the form is loaded data needs to be loaded to a List-Item(drop-down) component.
Here the item "**pl_select_modifier**" inside block "**bl_control**" is the component to populate data.


**QAFE Conversion:**

In this case "FORM1" is the form name.

The Oracle form item pl_select_modifier will be converted as a QAFE dropdown component as follows.

```xml
  <dropdown id="FORM1_BL_CONTROL_PL_SELECT_MODIFIER" name="PL_SELECT_MODIFIER"
     group-name="FORM1_BL_CONTROL_ALL" />
```

All the Program Units are converted as database procedures(in script files generated) and corresponding call statement, integration-tier method, business-actions are generated.
Database procedures are created with in/out variables based on the blocks used the code.

Statements.xml file entry for invoking database procedures.

```xml
  <call id="FORM1_POPULATE_MODIFIERS_LIST" call-name="FORM1_TRIGGERS.POPULATE_MODIFIERS_LIST"
      sql="{call FORM1_TRIGGERS.POPULATE_MODIFIERS_LIST(?,?)}"/>
```
Integration-tier method

```xml
  <method id="POPULATE_MODIFIERS_LIST" name="FORM1_POPULATE_MODIFIERS_LIST">
    <in name="P_BL_CONTROL" ref="P_BL_CONTROL"/>
    <out name="P_BL_CONTROL" ref="P_BL_CONTROL"/>
    <out name="QAFE_BUILT_IN_LIST" ref="QAFE_BUILT_IN_LIST"/>
  </method>
```
Business-Action

```xml
  <business-action id="FORM1_POPULATE_MODIFIERS_LIST">
    <transaction managed="no"/>
    <service ref="FORM1" method-ref="POPULATE_MODIFIERS_LIST">
      <in name="P_BL_CONTROL" ref="P_BL_CONTROL"/>
      <out name="P_BL_CONTROL" ref="P_BL_CONTROL"/>
      <out name="QAFE_BUILT_IN_LIST" ref="QAFE_BUILT_IN_LIST"/>
    </service>
  </business-action>
```

Database procedures are useful when we have to handle lot of code in back-end.
In this case we case we can move the select statement in the Program Unit code as a QAFE SQL Statement and set the result to the QAFE drop-down component.

For that change the call statement as a select statement using the query used in "populate_modifiers_list" Program Unit.

```xml
  <select id="MENTCB01_POPULATE_MODIFIERS_LIST"><![CDATA[
  	SELECT  distinct  u.user_name ,
    				to_char(u.user_id) user_id
      FROM fr_users u, mentc c
      where u.user_id = c.last_updated_by
      order by  user_name, user_id
  	]]></select>
```
Change integration-tier method , remove the inputs and add an output variable to get result of select statement.
```xml
  <method id="POPULATE_MODIFIERS_LIST" name="MENTCB01_POPULATE_MODIFIERS_LIST">
    <out name="POPULATE_MODIFIERS_LISTOut" />
  </method>
```

Change business-action method based on modified integration-tier method.

```xml
  <business-action id="FORM1_POPULATE_MODIFIERS_LIST">
    <transaction managed="no"/>
    <service ref="FORM1" method-ref="POPULATE_MODIFIERS_LIST">
      <out name="POPULATE_MODIFIERS_LISTOut" ref="POPULATE_MODIFIERS_LISTOut"/>
    </service>
  </business-action>
```

Now we can invoke this business action from the "onload" event of the main window and set the result to the drop-down component.

```xml
  <business-action ref="FORM1_POPULATE_MODIFIERS_LIST">
    <out name="POPULATE_MODIFIERS_LISTOut" ref="POPULATE_MODIFIERS_LISTOut"/>
  </business-action>

  <set component-id="FORM1_BL_CONTROL_PL_SELECT_MODIFIER" src="pipe" ref="POPULATE_MODIFIERS_LISTOut">
    <mapping key="id" value="USER_ID"/>
    <mapping key="value" value="USER_NAME"/>
  </set>
```


## 5. Database Block Conversion (For each Block)

<a name="block_sql_statements"></a>
### 5.1 Populate Data to Block.
Check the select statement involved and use that to load the data to block.

**Block definition:**  

*Form Name* **:** FORM1  
*Block Name* **:** PRODUCTS  
*Query Data Source Name* **:** PRODUCTS  
*WHERE Clause* **:** product_id >= 0  
*ORDER BY Clause* **:** product_id

**Functionality Handled:**

Based on block's Query Data Source Name, WHERE Clause and ORDER BY Clause the data will be populated to block.

Data is populated when the trigger code calls execute_query built-in, navigation of this block or when a new master-block record block of this block is selected.

**QAFE Conversion:**

Based on properties of the block select SQL statement and corresponding business-action and integration-tier methods will be generated.

Select statement generated can be found in FORM1-statements.xml
```sql
  <select id="FORM1_PRODUCTS_SELECT_BASETYPE"><![CDATA[
    SELECT * FROM PRODUCTS WHERE PRODUCT_ID >=0 ORDER BY PRODUCT_ID
  ]]></select>
```
Integration-tier method generated can be found in FORM1-integration-tier.xml

```xml
  <method id="PRODUCTS_SELECT_BASETYPE" name="FORM1_PRODUCTS_SELECT_BASETYPE" scrollable="true">
    <out name="PRODUCTS_SELECT_BASETYPEOut"/>
  </method>
```

Business Action generated to invoke this method can be found in FORM1-business-tier.xml
```xml
  <business-action id="FORM1_PRODUCTS_SELECT_BASETYPE">
    <transaction managed="no"/>
    <service ref="FORM1" method-ref="PRODUCTS_SELECT_BASETYPE">
      <out name="PRODUCTS_SELECT_BASETYPEOut" ref="PRODUCTS_SELECT_BASETYPEOut"/>
    </service>
  </business-action>
```

To populate data to the block PRODUCTS we have to invoke this business-action and set the result to the user interface.
For example we can invoke this code from onload event of the main window.

```xml
  <business-action ref="FORM1_PRODUCTS_SELECT_BASETYPE">
    <out name="PRODUCTS_SELECT_BASETYPEOut" ref="PRODUCTS_SELECT_BASETYPEOut"/>
  </business-action>
  <set component-id="FORM1_PRODUCTS_ID" ref="PRODUCTS_SELECT_BASETYPEOut"/>  
```
FORM1_PRODUCTS_ID is the ID of the QAFE data-grid equivalent to PRODUCTS.  
FORM1_PRODUCTS_SELECT_BASETYPE is the business-action to select data from database for PRODUCTS.  

- When PRODUCTS is multi-record block, the equivalent QAFE component generated is a data-grid. We can use ID of the datagrid to set data to it using ```<set component-id="FORM1_PRODUCTS_ID" ref="PRODUCTS_SELECT_BASETYPEOut"```.  
- When PRODUCTS is single-record block, each item in block will be converted to QAFE UI components with same  
group-name (FORM1_PRODUCTS_ALL) corresponding to the block. So we can use ```<set group-name="FORM1_PRODUCTS_ALL" ref="PRODUCTS_SELECT_BASETYPEOut">``` to set data to user interface.

### 5.2 Implement Block Level Triggers
Check the block triggers(PRE/POST-QUERY) and implement the same in QAFE using script file or modifying the select statement.

#### Trigger: POST-QUERY
##### Scenario : Populate look up data to multi record block

Block Definition:

*Form Name* **:** FORM1  
*Block Name* **:** PRODUCTS  
*Query Data Source Name* **:** PRODUCTS  
*WHERE Clause* **:** product_id >= 0  
*ORDER BY Clause* **:** product_id  
*Database Items* **:**  PRODUCT_NAME, PRODUCT_ID, PRODUCT_DIS, CREATED_BY_USER_ID, UPDATED_BY_USER_ID  
*Non-Database Items* **:**  CREATED_BY_USER_ID_DIS, UPDATED_BY_USER_ID_DIS


**POST-QUERY Trigger Code:**
```sql  
  select user_name into :CREATED_BY_USER_ID_DIS
  from users where user_id = :PRODUCTS.CREATED_BY_USER_ID;

  select user_name into :UPDATED_BY_USER_ID_DIS
  from users where user_id = :PRODUCTS.CREATED_BY_USER_ID_DIS;
```
**Functionality Handled:**

Based on block's Query Data Source Name, WHERE Clause and ORDER BY Clause the data will be populated to block. In this case we need some lookup data from another table to be filled in some items in the block. So POST-QUERY will be executed for all rows in the block to populate the additional information.  

Here PRODUCT_NAME, PRODUCT_ID, PRODUCT_DIS, CREATED_BY_USER_ID and UPDATED_BY_USER_ID are populated based on the table PRODUCTS. Non database fields CREATED_BY_USER_ID_DIS and UPDATED_BY_USER_ID_DIS are populated using the POST-QUERY code.

**QAFE Conversion:**

Based on properties of the block select SQL statement and corresponding business-action and integration-tier methods will be generated.

Select statement generated can be found in FORM1-statements.xml
```sql
  <select id="FORM1_PRODUCTS_SELECT_BASETYPE"><![CDATA[
    SELECT * FROM PRODUCTS WHERE PRODUCT_ID >=0 ORDER BY PRODUCT_ID
  ]]></select>
```
Integration-tier method generated to invoke this statement can be found in FORM1-integration-tier.xml

```xml
  <method id="PRODUCTS_SELECT_BASETYPE" name="FORM1_PRODUCTS_SELECT_BASETYPE" scrollable="true">
    <out name="PRODUCTS_SELECT_BASETYPEOut"/>
  </method>
```

Business Action generated to invoke method can be found in FORM1-business-tier.xml
```xml
  <business-action id="FORM1_PRODUCTS_SELECT_BASETYPE">
    <transaction managed="no"/>
    <service ref="FORM1" method-ref="PRODUCTS_SELECT_BASETYPE">
      <out name="PRODUCTS_SELECT_BASETYPEOut" ref="PRODUCTS_SELECT_BASETYPEOut"/>
    </service>
  </business-action>
```

To populate data to the block PRODUCTS we have to invoke this business-action and set the result to the user interface.

```xml
  <business-action ref="FORM1_PRODUCTS_SELECT_BASETYPE">
    <out name="PRODUCTS_SELECT_BASETYPEOut" ref="PRODUCTS_SELECT_BASETYPEOut"/>
  </business-action>
  <set component-id="FORM1_PRODUCTS_ID" ref="PRODUCTS_SELECT_BASETYPEOut"/>  
```
FORM1_PRODUCTS_ID is the ID of the QAFE data-grid equivalent to PRODUCTS.  
FORM1_PRODUCTS_SELECT_BASETYPE is the business-action to select data from database for PRODUCTS.  

- When PRODUCTS is multi-record block, the equivalent QAFE component generated is a data-grid. We can use ID of the datagrid to set data to it using ```<set component-id="FORM1_PRODUCTS_ID" ref="PRODUCTS_SELECT_BASETYPEOut"```.  
- When PRODUCTS is single-record block, each item in block will be converted to QAFE UI components with same  
group-name (FORM1_PRODUCTS_ALL) corresponding to the block. So we can use ```<set group-name="FORM1_PRODUCTS_ALL" ref="PRODUCTS_SELECT_BASETYPEOut">``` to set data to user interface.

Now the data will be populated only to the database items which corresponds to the columns in table PRODUCTS.

As part of conversion POST-QUERY trigger code will be created as a database procedure and is available at src/main/script/FORM1/MAIN_SCRIPT_COMPLEX_Package_Script_FORM1.sql or MAIN_SCRIPT_PURE_Package_Script_FORM1.sql file. Also corresponding call statements, integration-method and business action are generated.  
Apart from this one event is generated in FORM1-global-events.qaml file with id ```<event id="FORM1_RODUCTS_BLOCKTRIGGER_POST-QUERY"> ``` which is invoking the corresponding business action with proper inputs and outputs.

**Change the select statement to retrieve data including the lookup data**

When the post query code is simple select statements, we can make a JOIN statement which can also retrieve the look up data.

In this case we can modify the SQL statement as,
```sql
  <select id="FORM1_PRODUCTS_SELECT_BASETYPE"><![CDATA[
    SELECT PRODUCTS.*, a.user_name CREATED_BY_USER_ID_DIS , b.user_name UPDATED_BY_USER_ID_DIS
    FROM PRODUCTS
    LEFT OUTER JOIN USERS a ON a.user_id = created_by_user_id
    LEFT OUTER JOIN USERS b ON b.user_id = last_updated_by_user_id
    WHERE PRODUCT_ID >=0 ORDER BY PRODUCT_ID
  ]]></select>
```
This statement will retrieve the normal database items and the lookup information from other tables.
Here we have to make sure the data retrieved matches the lookup item's name attribute , for this we use alias for the values same as the Item's name.
(Data binding to QAFE component is using the name attribute of the component. This name attribute is same as the Oracle Forms item's name.)

Only change we have to make for converting the POST-QUERY code is the statements file, other layers remain the same.

In one database call we get all information needed to populate the block which will improve performance. So writing JOIN statement is the recommended way to convert POST-QUERY code.


Check the document [Forms Builit-In Handling](FormsTriggersToQAFEConversion.md)

Check the document [Re-usable Forms Builit-Ins](FormsReusableBuiltins.md)

### 5.3 Implement INSERT / UPDATE / DELETE operations.
Check the block triggers related(PRE/POST- INSERT/UPDATE/DELETE) and implement the same in QAFE using script file or modifying the select statement.

<a name="save_records_multi_record"></a>
#### Scenario : Save modified records of multi-record block

Block Definition:

*Form Name* **:** FORM1  
*Block Name* **:** PRODUCTS  
*Query Data Source Name* **:** PRODUCTS  
*WHERE Clause* **:** product_id >= 0  
*ORDER BY Clause* **:** product_id  
*Database Items* **:**  PRODUCT_NAME, PRODUCT_ID, PRODUCT_DIS, CREATED_BY_USER_ID, UPDATED_BY_USER_ID, LAST_UPDATED_DATE  
*Non-Database Items* **:**  CREATED_BY_USER_ID_DIS, UPDATED_BY_USER_ID_DIS

**Functionality Handled:**

User is allowed to update records in a multi-record block and the data gets saved to database on save action.

**QAFE Conversion:**
As part of conversion statement to update the table is generated with corresponding integration method and business-action.

Update statement generated can be found in FORM1-statements.xml
```sql
  <update id="FORM1_PRODUCTS_UPDATE_BASETYPE"><![CDATA[
    UPDATE PRODUCTS
    SET CREATED_BY_USER_ID = :CREATED_BY_USER_ID , CREATED_DATE = :CREATED_DATE ,
    LAST_UPDATED_BY_USER_ID = :LAST_UPDATED_BY_USER_ID ,
    LAST_UPDATED_DATE = :LAST_UPDATED_DATE , PRODUCT_ID = :PRODUCT_ID
    WHERE ID = :ID
  ]]></update>

```
Integration-tier method generated to invoke this statement can be found in FORM1-integration-tier.xml

```xml
  <method id="PRODUCTS_UPDATE_BASETYPE" name="FORM1_PRODUCTS_UPDATE_BASETYPE" scrollable="true">
    <in name="PRODUCTS_UPDATE_BASETYPEIn"/>
    <out name="result"/>
  </method>
```
PRODUCTS_UPDATE_BASETYPEIn is a map(key-value pairs) containing the binding variables like :PRODUCT_ID as key.

Business Action generated to invoke method can be found in FORM1-business-tier.xml

When PRODUCTS is a multi record block the business-action generated is expecting a list of records updated and for all records it calls the service method to update the record.
```xml
  <business-action id="FORM1_PRODUCTS_UPDATE_BASETYPE_Iterator">
    <transaction managed="no"/>
    <iteration begin="0" increment="1" items-ref="PRODUCTS_UPDATE_BASETYPEIn_List"
        var="PRODUCTS_UPDATE_BASETYPEIn" var-index="index">
      <service ref="FORM1" method-ref="PRODUCTS_UPDATE_BASETYPE">
        <in name="PRODUCTS_UPDATE_BASETYPEIn" ref="PRODUCTS_UPDATE_BASETYPEIn"/>
        <out name="result" ref="result"/>
      </service>
    </iteration>
  </business-action>
```

In case of Multi-record block, the block will be converted as a QAFE data-grid and an event is generated on save action of the data-grid as follows.

In QAML Builder,
- Click on the data-grid (Properties view shows "datagrid")
- Click on Events section below Properties.
- There you can see the event with Component "datagridId.save"

```xml
  <event id="FORM1_PRODUCT_WIN_FORM1_PRODUCTS_ID.savePRODUCT_NAME_onclick">
    <listeners>
      <listenergroup>
        <component ref="FORM1_PRODUCTS_ID.save"/>
        <listener type="onclick"/>
      </listenergroup>
    </listeners>
    <store name="modifiedRecords" ref="FORM1_PRODUCTS_ID.$$MODIFIED" src="component"/>
    <store name="deletedRecords" ref="FORM1_PRODUCTS_ID.$$DELETED" src="component"/>
    <store name="newRecords" ref="FORM1_PRODUCTS_ID.$$NEW" src="component"/>
    <if>
      <expression expr="len(${modifiedRecords}) > 0"/>
      <results>
        <result value="true">
          <business-action ref="FORM1_PRODUCTS_UPDATE_BASETYPE_Iterator">
            <in name="PRODUCTS_UPDATE_BASETYPEIn_List" ref="modifiedRecords"/>
            <out name="result" ref="result"/>
          </business-action>
        </result>
      </results>
    </if>
    ..
  </event>
```
FORM1_PRODUCTS_ID is the ID of the QAFE data-grid equivalent to PRODUCTS block.  
FORM1_PRODUCTS_UPDATE_BASETYPE_Iterator is the business-action to update the modified records.  
$$MODIFIED is the QAFE keyword to retrieve the modified records in a data-grid. FORM1_PRODUCTS_ID.$$MODIFIED returns list of map (key-value pairs) representing the modified records.

(Same way you can use $$DELETED and $$NEW to get deleted and new records in data-grid and use for insert or delete operations.)

This code will make sure the all the modified records in the data-grid is updated to database.

#### 5.3.1 PRE-UPDATE trigger

#### Scenario : Update additional data other than the block definition

**Trigger Code**
```
  :products.last_updated_by_user_id := :global.user_id;
  :products.last_updated_date  := sysdate;
```

**Functionality Handled:**

When any record in the block is updated by a user, the logged-in user_id and time of update should be stored.

Here user_id is available in global scope and the value for it is set when the user logs-in. Also last_updated_date is getting updated as the current date.

**QAFE Conversion:**

Check [How the Update action is handled in QAFE](#save_records_multi_record)

Since the trigger code is not a complex logic we can try to incorporate that in the update statement itself. This will make sure in place we handle the update operation.

For this we can modify the corresponding update statement to also update last_updated_by_user_id and last_updated_date.

Generated Update statement
```sql
  <update id="FORM1_PRODUCTS_UPDATE_BASETYPE"><![CDATA[
    UPDATE PRODUCTS
    SET CREATED_BY_USER_ID = :CREATED_BY_USER_ID , CREATED_DATE = :CREATED_DATE ,
    LAST_UPDATED_BY_USER_ID = :LAST_UPDATED_BY_USER_ID ,
    LAST_UPDATED_DATE = :LAST_UPDATED_DATE , PRODUCT_ID = :PRODUCT_ID
    WHERE ID = :ID
  ]]></update>
```

Modified Update statement to include PRE-UPDATE trigger code
```sql
  <update id="FORM1_PRODUCTS_UPDATE_BASETYPE"><![CDATA[
    UPDATE PRODUCTS
    SET CREATED_BY_USER_ID = :CREATED_BY_USER_ID , CREATED_DATE = :CREATED_DATE ,
    LAST_UPDATED_BY_USER_ID = :LAST_UPDATED_BY_USER_ID ,
    LAST_UPDATED_DATE = :LAST_UPDATED_DATE , PRODUCT_ID = :PRODUCT_ID ,
    LAST_UPDATED_BY_USER_ID = :USER_ID, LAST_UPDATED_DATE  = sysdate
    WHERE ID = :ID
  ]]></update>
```
- **:USER_ID** is the new binding variable added, which needs to be passed while invoking this statement.
- **LAST_UPDATED_DATE** is set directly to use sysdate of database

Also include the USER_ID as extra input variable in corresponding integration tier method and business-action.
```xml
  <method id="PRODUCTS_UPDATE_BASETYPE" name="FORM1_PRODUCTS_UPDATE_BASETYPE" scrollable="true">
    <in name="PRODUCTS_UPDATE_BASETYPEIn"/>
    <in name="USER_ID" ref="USER_ID"/>
    <out name="result"/>
  </method>
```
PRODUCTS_UPDATE_BASETYPEIn is a map(key-value pairs) containing the binding variables like :PRODUCT_ID as key.

Business Action generated to invoke method can be found in FORM1-business-tier.xml

When PRODUCTS is a multi record block the business-action generated is expecting a list of records updated and for all records it calls the service method to update the record.
```xml
  <business-action id="FORM1_PRODUCTS_UPDATE_BASETYPE_Iterator">
    <transaction managed="no"/>
    <iteration begin="0" increment="1" items-ref="PRODUCTS_UPDATE_BASETYPEIn_List"
        var="PRODUCTS_UPDATE_BASETYPEIn" var-index="index">
      <service ref="FORM1" method-ref="PRODUCTS_UPDATE_BASETYPE">
        <in name="PRODUCTS_UPDATE_BASETYPEIn" ref="PRODUCTS_UPDATE_BASETYPEIn"/>
        <in name="USER_ID" ref="USER_ID"/>
        <out name="result" ref="result"/>
      </service>
    </iteration>
  </business-action>
```

Make sure in the event when invoking the update action, you pass the variable LAST_UPDATED_BY_USER_ID with proper value.

```xml
  <business-action ref="FORM1_PRODUCTS_UPDATE_BASETYPE_Iterator">
    <in name="PRODUCTS_UPDATE_BASETYPEIn_List" ref="modifiedRecords"/>
    <in name="USER_ID" ref="LOGGED_IN_USER_ID" src="global"/>
    <out name="result" ref="result"/>
  </business-action>
```

### 5.4 Handle Master-Detail Relations
Check Relations of the block and invoke the detail population based on selection in master.

Check the document [Master Detail Handling in QAFE](FormsMasterDetailBlocksToQAFEConversion.md)

## 6. Item Trigger Handling
**TODO**
- Check all buttons functionality (For all Buttons)
- Check the trigger code for button and implement it in QAFE by using generated - database script or re-writing using QAML.
- Implement Validation for components
- Check the trigger code for validation and implement  it in QAFE by using generated database script or re-writing using QAML.
- Check the block triggers(PRE/POST-QUERY) and implement the same in QAFE using script file or modifying the select statement.

### 6.1 Opening other windows
In this section, opening other windows and populating the data afterwards is explained.
#### Trigger: WHEN-BUTTON-PRESSED
##### Scenario : Open other window and populate data

Block Definition:

*Form Name* **:** FORM1  
*Window Name* **:** WINDOW1, WINDOW2  
*Block Name* **:** PRODUCTS, containing a datagrid PRODUCTS_DATAGRID  
*Query Data Source Name* **:** PRODUCTS  
*WHERE Clause* **:** product_id >= 0  
*ORDER BY Clause* **:** product_id  

**Trigger Code**
```sql
go_block('PRODUCTS');
show_window('WINDOW2');
```

**Functionality Handled:**
When the button is pressed, a window is opened with certain data already set to one or more blocks. In this case, the PRODUCTS-block within WINDOW2 is set.

**QAFE Conversion:**
The show window built-in is already supported in QAFE using the openwindow built-in.

```xml
<openwindow external="false">
	<ref value="FORM1_WINDOW2"/>
</openwindow>
```

This will only open the window, but will not fire any other event. This needs to be done manually. Underneath is an example of this. WINDOW2 is opened using the openwindow-tag and an event is fired to populate the data.

```xml
<event id="FORM1_WINDOW1_BLOCK1_ITEM1_TRIGGER_WHEN-BUTTON-PRESSED_onclick">
  <listeners>
    <listenergroup>
      <component ref="FORM1_WINDOW1_BLOCK1_ITEM1"/>
      <listener type="onclick"/>
    </listenergroup>
  </listeners>
  <openwindow external="false">
  	<ref ref="FORM1_WINDOW2"/>
  </openwindow>
  <event ref="FORM1_WINDOW2_POPULATEDATA"/>
</event>
```

The populate data-event fetches products from the database and sets the items to a datagrid. The set built-in sets the data retrieved in the business-action to a component. The business-action below is the same one used in the Populate Data to Block-section. It is also possible to specify these actions directly in the onclick-event, but it is recommended to specify the actions in a separate event for reuse later on.

```xml
<event id="FORM1_WINDOW2_POPULATEDATA">
  <business-action ref="FORM1_PRODUCTS_SELECT_BASETYPE">
    <out name="PRODUCTS_SELECT_BASETYPEOut" ref="PRODUCTS_SELECT_BASETYPEOut"/>
  </business-action>
  <set component-id="FORM1_WINDOW2_PRODUCTS_DATAGRID" ref="PRODUCTS_SELECT_BASETYPEOut"/>
</event>
```

### 6.2 Further information
Check the document [Forms Built-In Handling](FormsTriggersToQAFEConversion.md)

## 7.Styling
The styling of the application can be done through directly using the style-attribute and using a CSS stylesheet.

### 7.1 Inline styling using the Style-attribute
It is possible to directly apply the styling to a component using the style-attribute of a component. All common CSS styling attributes can be used. The example below shows a label with a specific font size, the text aligned to the right side and a gray color.

```xml
<label id="newLabel" displayname="New Label" style="font-size:12px;text-align:right; color:#ADADAD;" />
```

### 7.2 Styling using a CSS stylesheet
The stylesheet has to be loaded inside the presentation-tier of the application. Inside a style element, the location of the stylesheet on the system and windows using the sheet have to be specified. The asterisk-symbol defines the use of the stylesheet on all available windows.
```xml
<styles>
	<style location="../css/newStyle.css" window-id="*" />
</styles>
```

Example of the content within a CSS file. The text is transformed into all uppercase using a specific font type and size. The color of the text is changed into a dark shade of red.
```css
.labelStyle{
	text-transform: uppercase;
	font-family: Arial, Helvetica, sans-serif;
	font-size: 12px;
	color: #C40606;
}
```

The class selectors defined within the CSS file are now usable. Using the class-attribute the CSS styling of that class selector is taken into account. For example, a label can be styled with this class selector, as shown in the example below.

```xml
<label id="newLabel" displayname="New Label" class="labelStyle" />
```

## 8. General Tips
### 8.1 Items to Ignore:
ignore- DATE* blocks which are specific for Oracle forms Calendar.
QAFE Calendar will be handled using textfield type="date".

### 8.2  How to find a SQL Query used in an Event
- Open the QAML file in the UI Tab in QAML Builder.  
- Select a component, like a button.  
- Open the Properties View, and go to the Events section.  
- Double-click on an event, this will open the event in the Event Tab in the QAML Builder.  
- Expand a Business-Action, recognized by the cogwheel icon.  
- Double-click on the expanded item, this will open the Business-Action in the Business-Action Tab in the QAML Builder.  
- Double-click on the expanded item, this will bring you to the sql statement, located in the statements.xml file.  
- The sql statements should look the same as defined in the Form.  

<a name="main_window"></a>
### 8.3 How QAFE detects the Main Window in FMB

- QAFE conversion checks the "Console Window" property of the Form. This is considered as main window.
- If "Console Window" property is not mentioned the first Window in the conversion process is considered as main window.

Assume for the form Console Window is mentioned as WINDOW1 for the Form FORM1

Oracle Form WINDOW1 will be converted to QAFE Window with id FORM1_WINDOW1

FORM1_WINDOW1 can be count in the file FORM1-presentation-tier-FORM1_WINDOW1.qaml

### 8.4 Changing the length of a column in database

Suppose we have a field displayed on a QAFE as part of conversion, and the field used to be 200 characters long. Now, we want to increase the length of the field to 2000.

Length of the field is used in QAFE at 2 places.
- In QAML client side code to restrict the user entering more characters using "max-length" attribute of the component.
- In equivalent Database Object Types used.

So for changing the length we might have to modify these two places.
