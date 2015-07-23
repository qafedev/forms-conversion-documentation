# Post Conversion Steps - Validation Trigger

In Oracle Form, when an item has a validation trigger, QAFE conversion tool converts them to different artifacts.

**Assume the following validation code in a FMB**
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

The SQL code mentioned above will be converted to:
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

#Post steps
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
  
