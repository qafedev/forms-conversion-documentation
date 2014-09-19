# Oracle Forms Builint-in Handling in QAFE#


### Oracle Forms Built-Ins which can be used in Database Procedures ###



| Forms Built-in |Oracle Forms Syntax |QAFE Built-in Executed |Remarks |
|----------|-----------------|---------------------|---------------|---------|
|SET_ITEM_PROPERTY | SET_ITEM_PROPERTY('block-name.item-name', visible, property_false); |```<set-property property="visible" value="false">```| Property set to Component with Id format block-name$item-name|
|SET_ITEM_PROPERTY | SET_ITEM_PROPERTY('block-name.item-name', enabled, property_false); |```<set-property property="enabled" value="false">```| |
|SET_ITEM_PROPERTY | SET_ITEM_PROPERTY('block-name.item-name', insert_allowed, property_false); |```<set-property property="editable"  value="false">```| |
|SET_ITEM_PROPERTY | SET_ITEM_PROPERTY('block-name.item-name', update_allowed, property_true); |```<set-property property="editable"  value="false">```| |
|SET_ITEM_PROPERTY | SET_ITEM_PROPERTY('block-name.item-name', delete_allowed, property_true); |```<set-property property="editable"  value="false">```| |
|HIDE_VIEW | HIDE_VIEW('canvas-name'); |```<set-property property="visible"  value="false">```| Panel with name canvas-name is made invisible |
|HIDE_VIEW | SHOW_VIEW('canvas-name'); |```<set-property property="visible"  value="true">```| Panel with name canvas-name is made visible |
|GO_BLOCK | GO_BLOCK('block-name'); |No QAFE Built-ins executed| QAFE_BUILTIN script will store this block name in a variable CURRENT_BLOCK_NAME to be used with clear_block built-in or other built-ins |
|CLEAR_BLOCK | CLEAR_BLOCK; |```<clear ref="blockname$ALL" />```| Based on previously stored CURRENT_BLOCK_NAME clear built-in executed for the group-name corresponding to block |
|CLEAR_BLOCK | Clear_Block(NO_VALIDATE); |```<clear ref="blockname$ALL" />```| Based on previously stored CURRENT_BLOCK_NAME clear built-in executed for the group-name corresponding to block |
|CLEAR_BLOCK | Clear_Block(NO_COMMIT); |```<clear ref="blockname$ALL" />```| Based on previously stored CURRENT_BLOCK_NAME, clear built-in executed for the group-name corresponding to block |
|CLEAR_LIST | Clear_List('list_name'); |```<clear ref="list_name" />```| If there is more than one item with same name in the window you should change the built-in call to use the component-id. |
|GO_ITEM | GO_ITEM('blockname.itemname'); |```<focus ref="blockname$itemname" />```|QAFE_BUILTIN script will store this block name in a variable CURRENT_ITEM to be used with clear_item built-in or other built-ins |
|CLEAR_ITEM | CLEAR_ITEM; |```<clear ref="blockname$itemname" />```|Based on previously stored CURRENT_ITEM, clear built-in executed for the current item in focus|
|SET_RADIO_BUTTON_PROPERTY | SET_RADIO_BUTTON_PROPERTY('BL_CONTROL.THRESHOLDS', 'IGNORE', enabled, property_false); |```<set-property property="enabled"  value="false" ../>```|component reference formed for qafe radio item is BL_CONTROL$THRESHOLDS$IGNORE|


##Converting Oracle Form Triggers to QAFE ##

### Functionality: Canvas Management ###
**Scenario: Changing Canvas Visibility / Position Dynamically**

Form triggers contains code to change different Canvas properties.
Oracle Form's set_view_property built-in is used to set visibility and positioning of the Canvas.  
Oracle Form's show_view, hide_view built-ins are used to show/hide a canvas.

**Sample Code:**
```SQL
PROCEDURE CGLY$RAISE_CANVAS(
   P_CANVAS IN VARCHAR2) IS  /* Current canvas */
/* Raise the current canvas, plus any dependent canvases to the top */
BEGIN
  set_view_property(P_CANVAS, VISIBLE, PROPERTY_ON);
  IF ( P_CANVAS = 'CG$PAGE_1') THEN
  	set_view_property('TAB_TA002', VISIBLE, PROPERTY_ON);
    set_view_property('CG$STACKED_FOOTER_1', DISPLAY_POSITION, 0, 4.999);
    set_view_property('CG$STACKED_FOOTER_1', VISIBLE, PROPERTY_ON);
  END IF;
END;
```

```SQL
hide_VIEW('canvas-one');
SHOW_VIEW('canvas-two');
```

**Conversion to QAFE:**

Even though this code will be generated in script file, better place for handling client side properties is QAFE Events.
As part of conversion Canvas objects are converted as QAFE Panel or Panel-Definition.

QAFE Builints ```<set-property>, <show-panel>, <close-panel> ```can be used to convert this functionality.
```
    <set-property property="visible" value="false">
        <component ref="panelDefId or panelId"/>
    </set-property>
    <show-panel ref="panel DefinitionId" />
    <close-panel ref="panel DefinitionId" />
```
### Functionality: Block Management ###
**Scenario: Populate the Block with new set of data **

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
go_item('myblock.myitem');
execute_query;
```

```
go_block('myblock');
execute_query;
```


**Conversion to QAFE:**

Even though this code will be generated in script file, better place for handling this code is QAFE Events.
As part of conversion based on the block Query Data Source, SQL statements are created for fetching the data.
Also corresponding business-actions are generated to invoke these statements.
Items in block are converted to QAFE components with group-name of all items in that block.

QAFE Built-ins ```<business-action ref=..>, <set group-name ..>, <set component-id ..> ```can be used to convert this functionality.
```
    <business-action ref="MYFORM_MYBLOCK_SELECT_BASETYPE">
      <out name="MYFORM_MYBLOCK_SELECT_BASETYPEOut" ref="MYFORM_MYBLOCK_SELECT_BASETYPEOut"/>
    </business-action>

    For single record block use group-name to set result value to user components
    <set group-name="MYFORM_MYBLOCK" ref="MYFORM_MYBLOCK_SELECT_BASETYPEOut"/>

    For multi record block use the corresponding component id of datagrid to set result value to user components
    <set component-id="MYFORM_MYBLOCK_ID" ref="MYFORM_MYBLOCK_SELECT_BASETYPEOut"/>
```


### Functionality: Item Management ###
**Scenario: Change the Visibility / Enabled property of items **

Form triggers contains code to dynamically change the visible/enabled properties of items.
Oracle Form's SET_ITEM_PROPERTY built-in is used to set these properties.

**Sample Code:**
```SQL
  SET_ITEM_PROPERTY('blobkName.item1Name',VISIBLE,PROPERTY_FALSE);
  SET_ITEM_PROPERTY('blobkName.item2Name',',ENABLED,PROPERTY_FALSE);
```

**Conversion to QAFE:**

QAFE Built-in ```<set-property> ```can be used to convert this functionality.
```
  <set-property property="visible" value="false">
    <component ref="formName_blockName_item1Name"/> <!-- use corresponding component-id -->
  </set-property>
  <set-property property="enabled" value="false">
    <component ref="formName_blockName_item2Name"/> <!-- use corresponding component-id -->
  </set-property>
```


**Scenario: Change the Update allowed property of item **

Form triggers contains code to dynamically change the update/insert allowed properties of items.
Oracle Form's SET_ITEM_PROPERTY built-in is used to set these properties.

**Sample Code:**
```SQL
  IF (:GLOBAL.USER_NAME <> 'USER1') THEN
    SET_ITEM_PROPERTY  ('blockName.item1',UPDATE_ALLOWED , PROPERTY_FALSE);
  ELSE
    SET_ITEM_PROPERTY  ('blockName.item2',UPDATE_ALLOWED , PROPERTY_TRUE);
  END IF;
```

```SQL
  SET_ITEM_INSTANCE_PROPERTY('TA002.F_STR_RPV', CURRENT_RECORD, INSERT_ALLOWED, PROPERTY_FALSE);
  SET_ITEM_INSTANCE_PROPERTY('TA002.F_STR_RPV', CURRENT_RECORD, UPDATE_ALLOWED, PROPERTY_FALSE);
  SET_ITEM_INSTANCE_PROPERTY('TA002.F_STR_RPV', CURRENT_RECORD, NAVIGABLE, PROPERTY_FALSE);
```

**Conversion to QAFE:**

There is no attribute same as insert-allowed or update-allowed in QAFE.
We can enable/disable/change visibility of the corresponding corresponding components to convert this code based on conditions.

QAFE Built-ins ```<if>, <set-property ..> ```can be used to convert this functionality.
```
    <if>
      <expression expr="${USER_NAME} != 'USER1'">
        <placeholder name="USER_NAME" ref="USER_NAME" src="global"/>
      </expression>
        <results>
          <result value="true">
            <set-property property="enabled" value="false">
              <component ref="formName_blockName_item1"/> <!-- use corresponding component-id -->
            </set-property>
          </result>
          <result value="false">
            <set-property property="enabled" value="false">
              <component ref="formName_blockName_item1"/> <!-- use corresponding component-id -->
            </set-property>
          </result>
        </results>
    </if>
```

If the item is in a multi-record block we can use the $SELECTED_INDEX property to change the component property of the current record.

```
  <set-property property="enabled" value="false">
    <component ref="dataGridId[$SELECTED_INDEX].columnIdofItem1"/>
  </set-property>
```
dataGridId is the id of the Datagrid equivalant to the block
columnId is the id of the column in this Datagrid representing the item1 component


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
    add_parameter(pl_id, 'PARAM1', TEXT_PARAMETER, :blockName.itemName1);
    add_parameter(pl_id, 'PARAM2', TEXT_PARAMETER, :blockName.itemName2);
    call_form('FORM2', no_hide, no_replace, no_query_only, pl_id);
  END;

```

**Conversion to QAFE:**
QAFE converts all the PARAMETERS in Oracle forms to textfield conponents with groupname of all the PARAMETERS set as <formName>_PARAMETER. This can be found in the main Window of the form.
```
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

```
  <show-panel src="panelDefinitionIdCorrespondingToFORM2" position="center" modal="true" auto-hide="false" />
  <set component-id="FORM2_PARAMETER_PARAM1" value="PARAM1 Static Value" />
  <set component-id="FORM2_PARAMETER_PARAM2" ref="componentIdWithParam2Vale" src="component" />

```

Now PARAM1 and PARAM2 are available to use in FORM2. Any QAFE event can use these values.


### Functionality: Exception Handling ###
**Scenario: Handling run time exceptions **

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
```
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

```
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
