# Oracle Forms Built-in Handling in QAFE#

This document refers to FMBs with following properties.

Form Name: FORM1 , Block Names : BLOCK1,BLOCK2 , Item Names : ITEM1, ITEM2, Window Names : WINDOW1, Canvas Names : CANVAS1, CANVAS2

Form Name: FORM2 , Block Names : BLOCK1,BLOCK2 , Item Names : ITEM1, ITEM2, Window Names : WINDOW1, Canvas Names : CANVAS1, CANVAS2

### Oracle Forms Built-Ins which can be used in Database Procedures ###

After Conversion you will get a script file containing the trigger code in your FMB.

This can be found in src/main/script/FORM1 folder of your QAFE Project with name MAIN_SCRIPT_Package_Script_FORM1.sql

Apart from this a QAFE provides script file QAFE_BUILT_IN.sql and QAFE_BUILT_IN_BODY.sql which handles the Oracle Forms Built-Ins (src/main/script folder).

MAIN_SCRIPT_Package_Script_FORM1 uses QAFE_BUILT_IN Package to handle some of the Oracle Forms Built-Ins.
To Execute any built-In in your script file you have to prefix the built-in with QAFE_BUILT_IN.

For Example your trigger code use SET_ITEM_PROPERTY('BLOCK1.ITEM1', visible, property_false);

This will be converted in script file as,
SET_ITEM_PROPERTY('BLOCK1.ITEM1', visible, property_false);

You have to change this code as below to execute this built-in,

QAFE_BUILT_IN.SET_ITEM_PROPERTY('BLOCK1.ITEM1', QAFE_BUILT_IN.visible, QAFE_BUILT_IN.property_false);

When this line get executed QAFE_BUILT_IN Package collect this in QAFE_BUILT_IN_LIST variable.

QAFE_BUILT_IN_LIST is a return value for all procedures generated and QAFE Engine will execute the corresponding QAFE built-Ins in the front-end after the execution of this procedure.


## Oracle Forms Built-In : SET_ITEM_PROPERTY

**Oracle Forms Syntax :**
```sql
 SET_ITEM_PROPERTY('BLOCK1.ITEM1', visible, property_false);
 ```

**QAFE Built-In Syntax in Procedure :**
```sql
QAFE_BUILT_IN.SET_ITEM_PROPERTY('BLOCK1.ITEM1', QAFE_BUILT_IN.visible, QAFE_BUILT_IN.property_false);
```

**QAFE Built-In Executed:**

```xml
  <set-property property="visible" value="false">
    <component ref="FORM1_BLOCK1_ITEM1"/>
  </set-property>
  FORM1_BLOCK1_ITEM1 is the component id generated for ITEM1
```

## Oracle Forms Built-In : SET_ITEM_PROPERTY

**Oracle Forms Syntax :**
```sql
 SET_ITEM_PROPERTY('BLOCK1.ITEM1', enabled, property_false)
```

**QAFE Built-In Syntax in Procedure :**
```sql
  QAFE_BUILT_IN.SET_ITEM_PROPERTY('BLOCK1.ITEM1', QAFE_BUILT_IN.enabled, QAFE_BUILT_IN.property_false);

```

**QAFE Built-In Executed:**

```xml
  <set-property property="enabled" value="false">
    <component ref="FORM1_BLOCK1_ITEM1"/>
  </set-property>
  FORM1_BLOCK1_ITEM1 is the component id generated for ITEM1
```
## Oracle Forms Built-In : SET_ITEM_PROPERTY

**Oracle Forms Syntax :**
```sql
 SET_ITEM_PROPERTY('BLOCK1.ITEM1', insert_allowed, property_false)
```

**QAFE Built-In Syntax in Procedure :**
```sql
  QAFE_BUILT_IN.SET_ITEM_PROPERTY('BLOCK1.ITEM1', QAFE_BUILT_IN.editable, QAFE_BUILT_IN.property_false);
```

**QAFE Built-In Executed:**

```xml
  <set-property property="editable" value="false">
    <component ref="FORM1_BLOCK1_ITEM1"/>
  </set-property>
  FORM1_BLOCK1_ITEM1 is the component id generated for ITEM1
```
## Oracle Forms Built-In : SET_ITEM_PROPERTY

**Oracle Forms Syntax :**
```sql
 SET_ITEM_PROPERTY('BLOCK1.ITEM1', update_allowed, property_false)
```

**QAFE Built-In Syntax in Procedure :**
```sql
  QAFE_BUILT_IN.SET_ITEM_PROPERTY('BLOCK1.ITEM1', QAFE_BUILT_IN.editable, QAFE_BUILT_IN.property_false);
```

**QAFE Built-In Executed:**

```xml
<set-property property="editable" value="false">
  <component ref="FORM1_P_BLOCK1_ITEM1"/>
</set-property>
```

## Oracle Forms Built-In : SET_ITEM_PROPERTY

**Oracle Forms Syntax :**
```sql
 SET_ITEM_PROPERTY('BLOCK1.ITEM1', delete_allowed, property_false)
```

**QAFE Built-In Syntax in Procedure :**
```sql
QAFE_BUILT_IN.SET_ITEM_PROPERTY('BLOCK1.ITEM1', QAFE_BUILT_IN.editable, QAFE_BUILT_IN.property_false);
```

**QAFE Built-In Executed:**

```xml
<set-property property="editable" value="false">
  <component ref="FORM1_P_BLOCK1_ITEM1"/>
</set-property>
```

## Oracle Forms Built-In : HIDE_VIEW

**Oracle Forms Syntax :**
```sql
 HIDE_VIEW('CANVAS1')
```

**QAFE Built-In Syntax in Procedure :**
```sql
QAFE_BUILT_IN.HIDE_VIEW('CANVAS1');
```

**QAFE Built-In Executed:**

```xml
<set-property property="visible" value="false">
  <component ref="CANVAS1"/>
</set-property>

```
Panel with name CANVAS1 is made invisible

## Oracle Forms Built-In : SHOW_VIEW

**Oracle Forms Syntax :**
```sql
 SHOW_VIEW('CANVAS1')
```

**QAFE Built-In Syntax in Procedure :**
```sql
QAFE_BUILT_IN.SHOW_VIEW('CANVAS1');
```

**QAFE Built-In Executed:**

```xml
<set-property property="visible" value="true">
  <component ref="CANVAS1"/>
</set-property>

```
Panel with name CANVAS1 is made visible

## Oracle Forms Built-In : GO_BLOCK

**Oracle Forms Syntax :**
```sql
 GO_BLOCK('BLOCK1')
```

**QAFE Built-In Syntax in Procedure :**
```xml
QAFE_BUILT_IN.GO_BLOCK('BLOCK1');
```

**QAFE Built-In Executed:**

  No QAFE Built-Ins executed.

QAFE_BUILTIN script will store this block name in a variable CURRENT_BLOCK_NAME in PACKAGE BODY of QAFE_BUILT_IN.

This value will be used when executing clear_block built-in or other built-ins


## Oracle Forms Built-In : CLEAR_BLOCK

**Oracle Forms Syntax :**
```xml
 GO_BLOCK('BLOCK1')
 CLEAR_BLOCK;
 Clear_Block(NO_VALIDATE);
 Clear_Block(NO_COMMIT);
```

**QAFE Built-In Syntax in Procedure :**
```xml
QAFE_BUILT_IN.GO_BLOCK('BLOCK1');

QAFE_BUILT_IN.CLEAR_BLOCK;
QAFE_BUILT_IN.CLEAR_BLOCK(QAFE_BUILT_IN.NO_VALIDATE);
QAFE_BUILT_IN.CLEAR_BLOCK(QAFE_BUILT_IN.NO_COMMIT);

```

**QAFE Built-In Executed:**

```xml
<clear ref="FORM1_BLOCK1_ALL" />
```

Based on previous GO_BLOCK('BLOCK1'),  clear built-in will be executed for the group-name corresponding to block.

Note: FORM1_BLOCK1_ALL is the group-name generated for all the items in a Block.

## Oracle Forms Built-In : CLEAR_LIST

**Oracle Forms Syntax :**
```xml
 Clear_List('list_name');
```

**QAFE Built-In Syntax in Procedure :**
```xml
QAFE_BUILT_IN.Clear_List('list_name');
```

**QAFE Built-In Executed:**

```xml
<clear ref="list_name" />
```
QAFE component with name=list_name will be cleared.
Multiple components can contain the same name, so it is better to change the ref to the component-id of the QAFE component.

## Oracle Forms Built-In : GO_ITEM

**Oracle Forms Syntax :**
```xml
 GO_ITEM('BLOCK1.ITEM1');
```

**QAFE Built-In Syntax in Procedure :**
```xml
QAFE_BUILT_IN.GO_ITEM('BLOCK1.ITEM1');
```

**QAFE Built-In Executed:**

```xml
<focus ref="FORM1_BLOCK1_ITEM1" />
```
QAFE_BUILTIN script will store this block name in a variable CURRENT_ITEM in PACKAGE BODY of QAFE_BUILT_IN.

This value will be used for clear_item built-in or other built-ins executed after this.

## Oracle Forms Built-In : CLEAR_ITEM

**Oracle Forms Syntax :**
```xml
 GO_ITEM('BLOCK1.ITEM1');
 CLEAR_ITEM;
```

**QAFE Built-In Syntax in Procedure :**
```xml
QAFE_BUILT_IN.GO_ITEM('BLOCK1.ITEM1');
QAFE_BUILT_IN.CLEAR_ITEM;
```

**QAFE Built-In Executed:**

```xml
  <clear ref="FORM1_BLOCK1_ITEM1" />
  BLOCK1_ITEM1 is the component-id generated for ITEM1.
```

### Oracle Forms Built-Ins handling in QAFE ###



| Forms Built-Ins |QAFE Built-Ins | References |
|----------|-----------------|---------------------|---------------|---------|
|SET_ITEM_PROPERTY | ```<set-property>``` | [Sample code](#set_item_property) |
|HIDE_VIEW | ```<set-property>``` | [Functionality: Canvas Management][] |


##Converting Oracle Form Triggers to QAFE ##

### Functionality: Canvas Management ###
**Scenario: Changing Canvas Visibility / Position Dynamically**

Form triggers contains code to change different Canvas properties.
Oracle Form's set_view_property built-in is used to set visibility and positioning of the Canvas.  
Oracle Form's show_view, hide_view built-ins are used to show/hide a canvas.

**Sample Code:**
```sql
PROCEDURE RAISE_CANVAS(
   P_CANVAS IN VARCHAR2) IS  /* Current canvas */
/* Raise the current canvas, plus any dependent canvases to the top */
BEGIN
  set_view_property(P_CANVAS, VISIBLE, PROPERTY_ON);
  IF ( P_CANVAS = 'CANVAS1') THEN
    set_view_property('CANVAS1', VISIBLE, PROPERTY_ON);
    set_view_property('CANVAS1', DISPLAY_POSITION, 0, 4.999);
    set_view_property('CANVAS2', VISIBLE, PROPERTY_OFF);
  END IF;
END;
```

```sql
  HIDE_VIEW('CANVAS1');
  SHOW_VIEW('CANVAS2');
```

**Conversion to QAFE:**

Even though this code will be generated in script file, better place for handling client side properties is QAFE Events.
As part of conversion Canvas objects are converted as QAFE Panel or Panel-Definition.

QAFE Built-Ins ```<set-property>, <show-panel>, <close-panel> ```can be used to convert this functionality.
```
    <set-property property="visible" value="false">
        <component ref="FORM1_WINDOW1_main_CANVAS1"/>
    </set-property>
    <show-panel ref="FORM1_WINDOW1_main_CANVAS1" />
    <close-panel ref="FORM1_WINDOW1_main_CANVAS1" />
```
FORM1_WINDOW1_main_CANVAS1 and FORM1_WINDOW1_main_CANVAS2 are the Panel-Definition Id corresponding to CANVAS1 and CANVAS2.

### Functionality: Block Management ###
**Scenario: Populate the Block with new set of data**

Form triggers contains code to populate the block.
Oracle Form's get_block_property built-in is used to get different properties of block
and EXECUTE_QUERY built-in or CGBS$.DO_KEYQRY are used to populate block data.  

**Sample Code:**
```SQL
BEGIN
  IF (get_block_property( :SYSTEM.TRIGGER_BLOCK, QUERY_ALLOWED) = 'FALSE') THEN
    MESSAGE('Error: Query not allowed in this block');
    RAISE FORM_TRIGGER_FAILURE;
  END IF;
  CGBS$.DO_KEYQRY( :SYSTEM.TRIGGER_BLOCK, 'EXECUTE_QUERY', :SYSTEM.MODE);
END;
```

```
go_item('BLOCK1.ITEM1');
execute_query;
```

```
go_block('BLOCK1');
execute_query;
```


**Conversion to QAFE:**

Even though this code will be generated in script file, better place for handling this code is QAFE Events.
As part of conversion based on the block Query Data Source, SQL statements are created for fetching the data.
Also corresponding business-actions are generated to invoke these statements.
Items in block are converted to QAFE components with group-name of all items in that block.

QAFE Built-ins ```<business-action>, <set>```can be used to convert this functionality.
```xml
    <business-action ref="FORM1_BLOCK1_SELECT_BASETYPE">
      <out name="FORM1_BLOCK1_SELECT_BASETYPEOut" ref="FORM1_BLOCK1_SELECT_BASETYPEOut"/>
    </business-action>

    For single record block use group-name to set result value to user components
    <set group-name="FORM1_BLOCK1" ref="FORM1_BLOCK1_SELECT_BASETYPEOut"/>

    For multi record block use the corresponding component id of datagrid to set result value to user components
    <set component-id="FORM1_BLOCK1_ID" ref="FORM1_BLOCK1_SELECT_BASETYPEOut"/>
```

<a name="set_item_property"></a>
### Functionality: Item Management ###
**Scenario: Change the Visibility / Enabled property of items**

Form triggers contains code to dynamically change the visible/enabled properties of items.
Oracle Form's SET_ITEM_PROPERTY built-in is used to set these properties.

**Sample Code:**
```SQL
  SET_ITEM_PROPERTY('BLOCK1.ITEM1', VISIBLE,PROPERTY_FALSE);
  SET_ITEM_PROPERTY('BLOCK1.ITEM2', ENABLED,PROPERTY_FALSE);
```

**Conversion to QAFE:**

QAFE Built-in ```<set-property> ```can be used to convert this functionality.
```xml
  <set-property property="visible" value="false">
    <component ref="FORM1_BLOCK1_ITEM1"/> <!-- corresponding component-id -->
  </set-property>
  <set-property property="enabled" value="false">
    <component ref="FORM1_BLOCK1_ITEM2"/> <!-- corresponding component-id -->
  </set-property>
```


**Scenario: Change the Update allowed property of item**

Form triggers contains code to dynamically change the update/insert allowed properties of items.
Oracle Form's SET_ITEM_PROPERTY built-in is used to set these properties.

**Sample Code:**
```SQL
  IF (:GLOBAL.USER_NAME <> 'USER1') THEN
    SET_ITEM_PROPERTY  ('BLOCK1.ITEM1',UPDATE_ALLOWED , PROPERTY_FALSE);
  ELSE
    SET_ITEM_PROPERTY  ('BLOCK1.ITEM2',UPDATE_ALLOWED , PROPERTY_TRUE);
  END IF;
```

```SQL
  SET_ITEM_INSTANCE_PROPERTY('BLOCK1.ITEM1', CURRENT_RECORD, INSERT_ALLOWED, PROPERTY_FALSE);
  SET_ITEM_INSTANCE_PROPERTY('BLOCK1.ITEM2', CURRENT_RECORD, UPDATE_ALLOWED, PROPERTY_FALSE);
  SET_ITEM_INSTANCE_PROPERTY('BLOCK1.ITEM3', CURRENT_RECORD, NAVIGABLE, PROPERTY_FALSE);
```

**Conversion to QAFE:**

There is no attribute same as insert-allowed or update-allowed in QAFE.
We can enable/disable/change visibility of the corresponding corresponding components to convert this code based on conditions.

QAFE Built-ins ```<if>, <set-property> ```can be used to convert this functionality.
```xml
    <if>
      <expression expr="${USER_NAME} != 'USER1'">
        <placeholder name="USER_NAME" ref="USER_NAME" src="global"/>
      </expression>
        <results>
          <result value="true">
            <set-property property="enabled" value="false">
              <component ref="FORM1_BLOCK1_ITEM1"/> <!-- use corresponding component-id -->
            </set-property>
          </result>
          <result value="false">
            <set-property property="enabled" value="false">
              <component ref="FORM1_BLOCK1_ITEM2"/> <!-- use corresponding component-id -->
            </set-property>
          </result>
        </results>
    </if>
```

If the item is in a multi-record block we can use the $SELECTED_INDEX QAFE keyword to change the component property of the current record.

```xml
  <set-property property="enabled" value="false">
    <component ref="FORM1_BLOCK1_ID[$SELECTED_INDEX].FORM1_BLOCK1_ITEM1"/>
  </set-property>
  FORM1_BLOCK1_ID is the id of the Data-grid equivalent to the block.
  FORM1_BLOCK1_ITEM1 is the id of the column in this Data-grid representing the item1 component

```


### Functionality: Opening a form using built-ins ###
**Scenario: Opening a new Oracle form from the current form **

Form triggers contains code to open a new from on specific triggers.
Oracle Form's create_parameter_list,add_parameter are used to create the parameters needed for the new form and call_form, open_form, new_form built-in is used to open a new form.

**Sample Code:**
```SQL
  DECLARE
    pl_id PARAMLIST;
    pl_name VARCHAR2(10);
  BEGIN
    pl_name := 'temp';
    pl_id   := get_parameter_list(pl_name);
    IF NOT ID_NULL(pl_id) THEN
      destroy_parameter_list(pl_id);
    END IF;
    pl_id := create_parameter_list(pl_name);
    add_parameter(pl_id, 'PARAM1', TEXT_PARAMETER, :BLOCK1.ITEM1);
    add_parameter(pl_id, 'PARAM2', TEXT_PARAMETER, :BLOCK1.ITEM2);
    call_form('FORM2', no_hide, no_replace, no_query_only, pl_id);
  END;

```

**Conversion to QAFE:**
QAFE converts all the PARAMETERS in Oracle forms to textfield components with group-name of all the PARAMETERS set as <formName>_PARAMETER. This can be found in the main Window of the form.
```xml
  <panel visible="false">
    <verticallayout>
      <textfield id="FORM2_PARAMETER_PARAM1" name="PARAM1" group-name="FORM2_PARAMETER">
        <value>NORMAL</value>
      </textfield>
      <textfield id="FORM2_PARAMETER_PARAM2" type="date"  name="PARAM2" group-name="FORM2_PARAMETER"/>
    </verticallayout>
  </panel>

```
After opening the new form using <show-panel> we can set the PARAMETERS values using <set component-id ..>

```xml
  <show-panel src="FORM2_WINDOW1_main_CANVAS1" position="center" modal="true" auto-hide="false" />
  <set component-id="FORM2_PARAMETER_PARAM1" value="PARAM1 Static Value" />
  <set component-id="FORM2_PARAMETER_PARAM2" ref="FORM1_BLOCK1_ITEM1" src="component" />

  FORM2_WINDOW1_main_CANVAS1 is the Panel-Definition Id corresponding to main canvas of FORM2.
  FORM1_BLOCK1_ITEM1 is the id of a component in FORM1.

```


Now PARAM1 and PARAM2 are available to use in FORM2. Any QAFE event can use these values.


### Functionality: Exception Handling ###
**Scenario: Handling run time exceptions**

Form triggers contains code to handle exceptions.
Oracle Form's TODO built-in are  used to set these properties.

**Sample Code:**
```SQL
  DECLARE
    err_code        CONSTANT NUMBER         := ERROR_CODE;
    err_type        CONSTANT VARCHAR2(3)    := ERROR_TYPE;
    server_err               NUMBER         := abs(DBMS_ERROR_CODE);
    server_msg               VARCHAR2(2000) := DBMS_ERROR_TEXT;
    constraint_name          VARCHAR2(61);
  BEGIN
    IF (err_type = 'FRM' AND   err_code IN (41105, 41106) ) THEN
      IF (err_code = 41105) THEN
        MESSAGE('Error: Query of Ta002 Nod Ret must be in context of Tt001 Tip Nod');
        RAISE FORM_TRIGGER_FAILURE;
      ELSIF (err_code = 41106) THEN
        MESSAGE('Error: Insert of Ta002 Nod Ret must be in context of Tt001 Tip Nod');
        RAISE FORM_TRIGGER_FAILURE;
        END IF;
    END IF;
  END;

```

**Conversion to QAFE:**
QAFE Exception handling is done in a two step process.
Using Intergation-Tier error tags and <error-hanlder> built-in define the error types.

You can also use  exception="*" to catch all exceptions.
```xml
    <integration-tier>
    <services/>
    <adapters/>
    <errors>
      <error id="valueLargerThanSpecified" exception="ORA-01438">
        <log>You tried to assign a numeric value to a column, but the value was larger than the column will allow. This occurred during either an INSERT or an UPDATE statement</log>
      </error>
      <error id="insertNullValueNotAllowed" exception="ORA-01400">
        <log>You tried to insert a NULL value into a column that does not accept NULL values</log>
      </error>
      <error id="updateNullValueNotAllowed" exception="ORA-01407">
        <log>You tried to update a column to a NULL value but the column will not accept NULL values</log>
      </error>
      <error id="duplicatedValueNotAllowed" exception="ORA-00001">
        <log>You tried to execute an INSERT or UPDATE statement that has created a duplicate value in a field restricted by a unique index</log>
      </error>
      <error id="numberInvalid" exception="ORA-01722">
        <log>You executed an SQL statement that tried to convert a string to a number, but it was unsuccessful</log>
      </error>
      <error id="dataNotFound" exception="ORA-01403">
        <log>You tried one of the following: you executed a SELECT INTO statement and no rows were returned, you referenced an uninitialized row in a table, or you read past the end of file with the UTL_FILE package</log>
      </error>
      <error id="anyExceptionHanlder" exception="*">
        <log>Generic exception handler</log>
      </error>
    </errors>
  </integration-tier>
```

In QAFE Event the defined type of exceptions can be caught using <error-handler>. Body of the error-hanlder can use any QAFE built-ins to handle this exception.

```xml
  <event>
    <listeners>
      <listenergroup>
        <component ref="qafeComponentId" />
        <listener type="onclick" />
      </listenergroup>
    </listeners>  
    <business-action ref="saveDataBusinessActionId">
      <in name="newRecord" ref="FORM1_GroupName" src="component"/>
    </business-action>
    <error-handler id="valueLargerThanSpecifiedHandler" final-action="swallow" error-ref="valueLargerThanSpecified">
      <set component-id="FORM1_BLOCK1_MESSAGES" value="Lookup code or Name length larger than allowed in database." />
    </error-handler>
    <error-handler id="duplicatedValueNotAllowedHanlder" final-action="swallow" error-ref="duplicatedValueNotAllowed">
      <set component-id="FORM1_BLOCK1_MESSAGES" value="Lookup code exist." />
    </error-handler>
    <error-handler id="anyExceptionHanlder" final-action="swallow" error-ref="anyException">
      <set component-id="FORM1_BLOCK1_MESSAGES" value="An exception has occurred. Please contact support team." />
    </error-handler>
  </event>

```
