# Post Conversion Steps - Dynamic dropdown using Record Group VO
The example below is for a Record Group VO without any input variables.

##Artifacts Generated
Record groups are converted to View Objects, while retaining the query with minimal changes. The VO also includes input parameters, if available. The record group can be found with file name with format {RecordGroupName}VO.xml.

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

## Post Generation Steps to use a Record Group VO in a dropdown
This step is very easy, when a Record Group VO does not have any input variables. Make sure that the dropdown is visible within a JSF Fragment in designer mode. Open the Data control in the Data control menu and select a View Object. Drag and drop the object itself in the dropdown. 

In the Edit List Binding pop-up, the display attribute has to be set. When clicking on OK, the binding will be set and/or updated. Press OK to confirm all new bindings. All underlying code including the binding, VO iterator and dat control elements are automatically generated. The final step is to just start the application and the dropdown should be filled automatically.
