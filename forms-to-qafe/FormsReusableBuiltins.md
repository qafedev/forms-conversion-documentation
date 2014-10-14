# Oracle Forms Built-in Handling in QAFE#
*This document refers to FMBs with following properties.*

Form Name: FORM1  
Block Names : BLOCK1, BLOCK2  
Item Names : ITEM1,ITEM2  
Window Names : WINDOW1  
Canvas Names : CANVAS1, CANVAS2

Form Name: FORM2  
Block Names : BLOCK1, BLOCK2  
Item Names : ITEM1, ITEM2  
Window Names : WINDOW1  
Canvas Names : CANVAS1, CANVAS2

### Oracle Forms Built-Ins which can be used in Database Procedures ###

After Conversion (FORM1.fmb) you will get a script file containing the trigger code in your FMB.

This can be found in src/main/script/FORM1 folder of your QAFE Project with name MAIN_SCRIPT_Package_Script_FORM1.sql

Apart from this QAFE provides script file QAFE_BUILT_IN.sql and QAFE_BUILT_IN_BODY.sql which handles the Oracle Forms Built-Ins (src/main/script folder).

MAIN_SCRIPT_Package_Script_FORM1 uses QAFE_BUILT_IN Package to handle some of the Oracle Forms Built-Ins.
To Execute any built-In in your script file you have to prefix the built-in with the `QAFE_BUILT_IN.` string.

For Example your trigger code use SET_ITEM_PROPERTY('BLOCK1.ITEM1', visible, property_false);

This will be converted in script file as,
SET_ITEM_PROPERTY('BLOCK1.ITEM1', visible, property_false);

You have to change this code as below to execute this built-in,

QAFE_BUILT_IN.SET_ITEM_PROPERTY('BLOCK1.ITEM1', QAFE_BUILT_IN.visible, QAFE_BUILT_IN.property_false);

When this line get executed QAFE_BUILT_IN Package collect this in QAFE_BUILT_IN_LIST variable.

QAFE_BUILT_IN_LIST is a return value for all procedures generated and QAFE Engine will execute the corresponding QAFE built-Ins in the front-end after the execution of this procedure.


## Oracle Forms Built-In : SET_ITEM_PROPERTY - visible

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

## Oracle Forms Built-In : SET_ITEM_PROPERTY - enabled

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
## Oracle Forms Built-In : SET_ITEM_PROPERTY - insert_allowed

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
## Oracle Forms Built-In : SET_ITEM_PROPERTY - update_allowed

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

## Oracle Forms Built-In : SET_ITEM_PROPERTY - delete_allowed

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

## Oracle Forms Built-In : SET_VIEW_PROPERTY

**Oracle Forms Syntax :**
```sql
 set_view_property ('CANVAS1', visible, property_true);
```

**QAFE Built-In Syntax in Procedure :**
```sql
QAFE_BUILT_IN.set_view_property ('CANVAS1', QAFE_BUILT_IN.visible, QAFE_BUILT_IN.property_true);
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
