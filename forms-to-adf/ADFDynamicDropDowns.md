# Post Conversion Steps - Dynamic Drop Down



<!-- toc -->

* [Artifacts Generated](#artifacts-generated)
* [Post Generation Steps](#post-generation-steps)

<!-- toc stop -->


All Oracle form List Items are converted to ADF SelectOneChoice. All items mentioned in the Elements List will be converted as static choice items.
If there are no items defined in the  Elements List we generate the ADF artifacts necessary to populate this dropdown dynamically.
Artifacts Generated:

##Artifacts Generated

1. View object is generated with name {ItemName}ListVO with a dummy custom SQL Query.

  ```sql
    select sysdate ValueField, sysdate DisplayNameField from dual;
  ```
2. List Binding is created in the corresponding page definition file.
```xml
  <list id="Hsd0004f_dropdown_civil_stateVO" StaticList="false" IterBinding="CIVIL_STATEListIterator"
     SelectItemValueMode="ListObject" ListIter="CIVIL_STATEListIterator">
    <AttrNames>
      <Item Value="ValueField"/>
    </AttrNames>
    <ListAttrNames>
      <Item Value="ValueField"/>
    </ListAttrNames>
    <ListDisplayAttrNames>
      <Item Value="DisplayNameField"/>
    </ListDisplayAttrNames>
  </list>
```
3. SelectOneChoice is created

  ```xml
    <af:selectOneChoice id="HSD0004F_DROPDOWN_EMP_CIVIL_STATE"
        value="#{bindings.EMP_CIVILSTATE.inputValue}" valuePassThru="true">
      <f:selectItems value="#{bindings.Hsd0004f_dropdown_civil_stateVO.items}"/>
    </af:se1lectOneChoice>
  ```

##Post Generation Steps

The only change you have to make after conversion is to change the SQLQuery based on the FMB.

Make sure ValueField and DisplayNameField are used as alias for the columns.

For example; if actual select statement for populating the drop down is ,

```sql
    SELECT distinct content_name , to_char(mentc_id) mentc_id
    FROM mentc order by content_name
```

Modify the  SQL query of <ItemName>ListVO as below and re-start the application.  
The new data will be populated in the corresponding SelectOneChoice.

```sql
    SELECT distinct content_name DisplayNameField   , to_char(mentc_id) ValueField
    FROM mentc order by content_name
```

