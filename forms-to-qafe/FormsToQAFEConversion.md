#QAFE Forms Conversion
This document explains steps to follow to convert an Oracle Forms to a fully functional QAFE application.

This document is meant for QAFE Forms Conversion users with knowledge of Oracle Forms and QAFE.

## 1. Automatic conversion using the wizard

### 1.1 Setting up the QAFE Forms Conversion Environment
QAFE Forms conversion can be done using QAFE Conversion Wizard.

Check [QAFE site](http://www.qafe.com) for details about how to download and set up the QAML Builder environment for Forms Conversion.

Check [here](http://www.qafe.com/developer-docs/oracle-forms/qafe-forms-wizard/) for details about how to use the Forms Conversion Wizard.
### 1.2 Files generated after conversion

**Reference Documents:**

[Structure of generated files](http://www.qafe.com/developer-docs/oracle-forms/code-walkthrough/).

[Re-using Forms Built-ins](FormsReusableBuiltins.md).

[Re-using Forms Built-ins](FormsReusableBuiltins.md).

[Converting Forms Triggers to QAFE](FormsTriggersToQAFEConversion.md).

[Forms Triggers to QAFE Listeners](FormsTriggersToQAFEListner.md).

[Abbreviations used in conversion process](FormsTriggersAbbreviations.md).

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

### 8.1  How to find a SQL Query used in an Event
Open the QAML file in the UI Tab in QAML Builder.
Select a component, like a button.
Open the Properties View, and go to the Events section.
Double-click on an event, this will open the event in the Event Tab in the QAML Builder.
Expand a Business-Action, recognized by the cogwheel icon.
Double-click on the expanded item, this will open the Business-Action in the Business-Action Tab in the QAML Builder.
Double-click on the expanded item, this will bring you to the sql statement, located in the statements.xml file.
The sql statements should look the same as defined in the Form.
