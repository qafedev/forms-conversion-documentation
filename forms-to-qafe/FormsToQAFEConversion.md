#QAFE Forms Conversion
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

#### 2.2.1 Run the generated script file containing the Object Types creation.

#### 2.2.1 Check the converted script file.
Check the converted script file and see how much logic is in the triggers and how much we can reuse.
Individual procedures in this script can be executed in database as needed.


## 3. UI Changes
Open the FMB in the Forms Builder.
Open the QAML file in QAML Builder and Re-position the UI components using QAML Builder.

## 4. Form Trigger Conversion
Check the Form level trigger(PRE_FORM, WHEN-NEW-FORM-INSTANCE ) code and implement the same in QAFE.

Check code in this triggers to understand the functionality. This trigger code can call other Program-Unit(s). Check all code involved.
Re-use the logic using database procedures or split and implement it in QAFE.

### Scenario : When opening the Form data needs to be populated to a block

**Trigger :** WHEN-NEW-FORM-INSTANCE

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

FORM1_WINDOW1 can be count in the file FORM1-presentation-tier-FORM1_WINDOW1.qaml

Check onload event of FORM1_WINDOW1.

When trigger conversion enabled option is selected in the Conversionn Wizard, onload event will be calling an event that handles the WHEN-NEW-FORM-INSTANCE using database package. Database package is useful when you have lot of logic or database interaction in the trigger.
In this case the functionality is just to populate the block. We can do this using QAFE artifacts without using database packages. This can be handled by ```<business-action>``` and ```<set>``` built-ins in QAFE.

Comment or remove the following line from the onload event if exist.
```<event ref="FORM1_FORMTRIGGER_WHEN_NEW_FORM_INSTANCE"/>```

We can also remove the event with id FORM1_FORMTRIGGER_WHEN_NEW_FORM_INSTANCE and all the other layers(business-action, integration-tier method, call statement entry) after implementing same functionality in QAFE.

As part of conversion the code to populate data to the main block (First database block in the Window) is generated in onload event.
In that case you will see a ```<business-action>``` call in the event and after that ```<set>``` built-in is used to set data to user interface.

When QAFE detects that BLOCK1 is the main block of the window the following code will be generated to populate the data to the QAFE components equivalent of the BLOCK1.

When BLOCK1 is multi-record block the equivalent QAFE component generated is a data-grid. We can use ID of the datagrid to set data to it.
```xml
  <business-action id="FORM1_BLOCK1_SELECT_BASETYPE">
    <transaction managed="no"/>
    <service ref="FORM1" method-ref="BLOCK1_SELECT_BASETYPE">
      <out name="BLOCK1_SELECT_BASETYPEOut" ref="BLOCK1_SELECT_BASETYPEOut"/>
    </service>
  </business-action>
  <set component-id="FORM1_BLOCK1_ID" ref="BLOCK1_SELECT_BASETYPEOut"/>
  FORM1_BLOCK1_ID is ID of the QAFE data-grid equivalent to BLOCK1.
  FORM1_BLOCK1_SELECT_BASETYPE is the business-action to select data from database for BLOCK1.
  FORM1_BLOCK1_SELECT_BASETYPE will execute SQL statement created based on BLOCK1's Query Data Source Name, WHERE Clause and ORDER BY Clause.

```

Also check the document [Forms Builit-In Handling](FormsTriggersToQAFEConversion.md#trigger_block_management)


## 5. Database Block Conversion (For each Block)

### 5.1 Populate Data to Block.
Check the select statement involved and use that to load the data to block.

### 5.2 Implement Block Level Triggers
Check the block triggers(PRE/POST-QUERY) and implement the same in QAFE using script file or modifying the select statement.

Check the document [Forms Builit-In Handling](FormsBuiltinsHandling.md)

Check the document [Re-usable Forms Builit-Ins](FormsReusableBuiltins.md)


### 5.3 Implement INSERT / UPDATE / DELETE operations.
Check the block triggers related(PRE/POST- INSERT/UPDATE/DELETE) and implement the same in QAFE using script file or modifying the select statement.

### 5.4 Handle Master-Detail Relations
Check Relations of the block and invoke the detail population based on selection in master.

Check the document [Master Detail Handling in QAFE](FormsMasterDetailBlocksToQAFEConversion.md)

## 6. Item Trigger Handling
Check all buttons functionality (For all Buttons)
Check the trigger code for button and implement it in QAFE by using generated database script or re-writing using QAML.
Implement Validation for components
Check the trigger code for validation and implement  it in QAFE by using generated database script or re-writing using QAML.

Check the document [Forms Builit-In Handling](FormsBuiltinsHandling.md)

## 7.Styling
Styling (CSS).
Adapt to the corporate identity style.

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
