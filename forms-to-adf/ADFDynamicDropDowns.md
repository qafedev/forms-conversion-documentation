# Post Conversion Steps - Dynamic Drop Down



<!-- toc -->

* [Artifacts Generated](#artifacts-generated)
* [Post Generation Steps](#post-generation-steps)

<!-- toc stop -->


All Oracle form List Items are converted to ADF SelectOneChoice. All items mentioned in the Elements List will be converted as static choice items.
If there are no items defined in the  Elements List we generate the ADF artifacts necessary to populate this dropdown dynamically.
Artifacts Generated:

##Artifacts Generated

1. View object is generated with name <ItemName>ListVO with a dummy custom SQL Query.

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
4. Record groups are converted to View Objects, while retaining the query with minimal changes. The VO also includes input parameters, if available.
```xml 
<ViewObject xmlns="http://xmlns.oracle.com/bc4j" Name="MvVO" Version="12.1.2.66.68" InheritPersonalization="merge" BindingStyle="OracleName" CustomQuery="true" PageIterMode="Full" ComponentClass="com.qualogy.qafe.model.view.BaseVOImpl" FetchMode="FETCH_AS_NEEDED" FetchSize="10">
  <DesignTime>
    <Attr Name="_codeGenFlag2" Value="Coll"/>
  </DesignTime>
  <SQLQuery><![CDATA[select mv_name, e.meaning,m.mv_id from meta_versions m, engine_lookup_values e]]></SQLQuery>
  <ViewAttribute IsPersistent="false" Type="java.lang.String" IsUpdateable="false" Name="MVNAME" AliasName="MV_NAME" PrecisionRule="true" Precision="30" ColumnType="VARCHAR2" Expression="MV_NAME" SQLType="VARCHAR"/>
  <ViewAttribute IsPersistent="false" Type="java.lang.String" IsUpdateable="false" Name="MEANING" AliasName="MEANING" PrecisionRule="true" Precision="30" ColumnType="VARCHAR2" Expression="MEANING" SQLType="VARCHAR"/>
  <ViewAttribute IsPersistent="false" Type="java.lang.Integer" IsUpdateable="false" Name="MVID" AliasName="MV_ID" PrecisionRule="true" Precision="23" ColumnType="NUMBER" Expression="MV_ID" SQLType="NUMERIC"/>
  <AttrArray Name="KeyAttributes">
    <Item Value="MVNAME"/>
    <Item Value="MEANING"/>
    <Item Value="MVID"/>
  </AttrArray>
  <Properties>
    <CustomProperties/>
  </Properties>
</ViewObject>
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
### Use a Record Group VO with no input variables to fill a dropdown
This step is very easy, when a Record Group VO does not have any input variables. Make sure that the dropdown is visible within a JSF Fragment in designer mode. Open the Data control in the Data control menu and select a View Object. Drag and drop the object itself in the dropdown. In the Edit List Binding pop-up, the display attribute has to be set. When clicking on OK, the binding will be set and/or updated. Press OK to confirm all new bindings. All underlying code including the binding, VO iterator and dat control elements are automatically generated. The final step is to just start the application and the dropdown should be filled automatically.
