# Post Conversion Steps - Validation Trigger

## Initial Oracle Form and conversion result
### Example 

The student application (EX10_01) is used to explain the validation scenario. In the application we are considering validation for the zip code text field component within the STUDENT block. Validation checks if the zip code is available in the database or not. If validation fails, it shows proper message to the user.

(screenshot)

### Validation trigger in the example Oracle Form
The ZIP_CODE item has a validation trigger as follows:
```sql
DECLARE
	CURSOR c_get_zip is SELECT city, state
			FROM zipcode
			WHERE zipcode.zip = :STUDENT.ZIP;
	v_invalid BOOLEAN;
	x number;
BEGIN
	OPEN c_get_zip;
	FETCH c_get_zip INTO :STUDENT.CITY, :STUDENT.STATE;
	v_invalid := c_get_zip%NOTFOUND;
	IF v_invalid THEN
		MESSAGE('This Zipcode does not exist in the database.');
		RAISE FORM_TRIGGER_FAILURE;
	END IF;
END;
```
### Artifacts generated to handle validation: 
- **StudentEO.xml**: Entity object representing the student block.
- **StudentEOImpl.java**: Validation implementation class.
- **OBJECTS_MAIN_SCRIPT_EX10_01.sql**: Script file with Database Object types representing the blocks in EX10_01.
- **PURE_MAIN_SCRIPT_EX10_01.sql**: Script file with database procedure to handle validation of ZIP_CODE.

The validation trigger code will be converted and available in PURE_MAIN_SCRIPT_EX10_01.sql:
```sql
procedure STUDENT_ZIP_WVI (P_STUDENT IN OUT ADF_EX10_01_STUDENT)  is 
  begin 		DECLARE
    CURSOR c_get_zip is SELECT city, state
    										FROM zipcode
                        WHERE zipcode.zip = P_STUDENT.ZIP;
    v_invalid BOOLEAN;
    x number;
    BEGIN
    OPEN c_get_zip;
    FETCH c_get_zip INTO P_STUDENT.CITY, P_STUDENT.STATE;
    v_invalid := c_get_zip%NOTFOUND;
    IF v_invalid THEN
      MESSAGE('THIS ZIPCODE DOES NOT EXIST IN THE DATABASE.');
      RAISE FORM_TRIGGER_FAILURE();
    END IF;
  END; 
end;
```
With the corresponding database object, like:
```sql
CREATE OR REPLACE TYPE ADF_EX10_01_STUDENT AS OBJECT
 (STUDENT_ID	NUMBER
 ,SALUTATION	VARCHAR2(5)
 ,FIRST_NAME	VARCHAR2(25)
 ,LAST_NAME	VARCHAR2(25)
 ,PHONE	VARCHAR2(15)
 ,STREET_ADDRESS	VARCHAR2(50)
 ,ZIP	VARCHAR2(5)
 ,CITY	VARCHAR2(30)
 ,STATE	VARCHAR2(30)
 ,EMPLOYER	VARCHAR2(50)
 ,REGISTRATION_DATE	DATE
 ,CONSTRUCTOR FUNCTION  ADF_EX10_01_STUDENT  RETURN SELF AS RESULT
 );
```
### Validation Flow
In StudentEO we generate the attribute ZIP as follows.
```java
<Attribute Name="ZIP" IsNotNull="false" Type="java.lang.String" Precision="5" ColumnType="VARCHAR2" TableName="STUDENT" ColumnName="ZIP" SQLType="VARCHAR">
    <MethodValidationBean xmlns="http://xmlns.oracle.com/bc4j" Name="STUDENT_ZIP_WVI" MethodName="validateZIP" ResId="DUMMY"/>
  </Attribute>
```

The MethodValidationBean will trigger the validateZIP method in StudentEOImpl class:
```java
public boolean validateZIP(java.lang.String zip) {
	return validateAttribute("ZIP", zip, "ADF_EX10_01_TRIGGERS.STUDENT_ZIP_WVI(?)", "ADF_EX10_01_STUDENT");
}
```
The validateZIP method will call the validateAttribute method with the following parameters:
- "ZIP": The attribute name to validate
- zip: Current value for the above attribute
- "ADF_EX10_01_TRIGGERS.STUDENT_ZIP_WVI(?)": The name of the stored procedure, which performs the validation
- "ADF_EX10_01_STUDENT": The database object type representing the student entity/block

As per the above code execution the STUDENT_ZIP_WVI stored procedure will get called. ADF code expects an attribute with name MESSAGE in the output object type. Based on the message value, ADF code determines whether the validation was successful or not. The code can be found in BaseEOImpl.validateAttribute method. BaseEOImpl is the base class for all entities.

##Post steps
In order to run the generated ADF application properly with validation, some post-modifications should be made in the generated sql script, located in the scripts folder in the output:

1. Add the following column to all the generated database objects where you want to use validation:

  ```sql
  MESSAGE VARCHAR2(1000);
  ```
2. Remove the following in the body of all procedures:
```sql
RAISE FORM_TRIGGER_FAILURE()  
```
*NOTE*: FORM_TRIGGER_FAILURE is a forms builtin which cannot be used in the database. Similarly remove/re-write other forms builtins.

3. Modify the Message built-in in the body of the procedure:
  
  ```sql
  MESSAGE('This Zipcode does not exist in the database.');  
  ```
  To: 
  ```sql
  P_STUDENT.MESSAGE := 'This Zipcode does not exist in the database.';
  ```
The validation code in ADF expects the object type (ADF_EX10_01_STUDENT) to have a message attribute to do validation on an object. If the message is empty, the validation is successful and the application will continue. If not, the message is shown to the user.
  
