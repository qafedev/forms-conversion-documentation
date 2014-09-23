# Master-Detail Handling

This section describes how master-detail blocks are handled in QAFE.

Generated QAFE Code for Master-Detail Handling

**Select statements are getting generated for Master block based on the table name, where condition and order by clause mentioned in block properties.**

This can be found in the statements.xml file

```xml
Format of Statement Id for Master Block : <form-name>_<block-name>_SELECT_BASETYPE
<select id="FRET600_TT001_SELECT_BASETYPE"><![CDATA[
  SELECT * FROM TT001_TIP_NOD ORDER BY C_TIP_NOD
  ]]></select>
```

**Select statements are generated for Detail block based on the table name, where condition , order by clause and Relation Join Conditions.**

 This can be found in the statements.xml file

  ```xml
  Format of Statement Id for Detail Block : <form-name>_<block-name>_SELECT_DETAIL
  <select id="FRET600_TA002_SELECT_DETAIL"><![CDATA[
  SELECT * FROM TA002_NOD_RET TA002 WHERE TA002.TT001_C_TIP_NOD = :C_TIP_NOD ORDER BY C_NOD_RET
  ]]></select>
  ```

**Service methods are generated to invoke these statements.**

  This can be found in integration-tier.qaml
  ```xml
  <method id="TT001_SELECT_BASETYPE" name="FRET600_TT001_SELECT_BASETYPE" scrollable="true">
    <out name="TT001_SELECT_BASETYPEOut"/>
  </method>
  <method id="TA002_SELECT_DETAIL" name="FRET600_TA002_SELECT_DETAIL" scrollable="true">
    <in name="C_TIP_NOD"/>
    <out name="TA002_SELECT_DETAILOut"/>
  </method>
```
**Business Actions are generated to invoke the service methods.**

  This can be found in business-tier.qaml.
  ```xml
  <business-action id="FRET600_TT001_SELECT_BASETYPE">
      <transaction managed="no"/>
      <service ref="FRET600" method-ref="TT001_SELECT_BASETYPE">
          <out name="TT001_SELECT_BASETYPEOut" ref="TT001_SELECT_BASETYPEOut"/>
      </service>
  </business-action>

  <business-action id="FRET600_TA002_SELECT_DETAIL">
    <transaction managed="no" />
    <service ref="FRET600" method-ref="TA002_SELECT_DETAIL">
        <in name="C_TIP_NOD" ref="C_TIP_NOD" />
        <out name="TA002_SELECT_DETAILOut" ref="TA002_SELECT_DETAILOut" />
    </service>
  </business-action>
```
**Event generated to handle the detail blocks population in global-events.qaml file.**

  This event call business-actions for all the detail blocks and sets the data to the corresponding detail block.
```xml
  Format of event id : <form-name>_<block-name>_BLOCKTRIGGER_ON-POPULATE-DETAILS
  <event id="FRET600_TT001_BLOCKTRIGGER_ON-POPULATE-DETAILS">
      <listeners/>
      <clear ref="FRET600_TA002_ID"/>
      <clear ref="TA633"/>
      <store name="$OFFSET" value="0"/>
      <store name="$PAGESIZE" value="20"/>
      <business-action ref="FRET600_TA002_SELECT_DETAIL">
        <in name="C_TIP_NOD" ref="FRET600_TT001_C_TIP_NOD"/>
        <out name="TA002_SELECT_DETAILOut" ref="TA002_SELECT_DETAILOut"/>
      </business-action>
      <if>
        <expression expr="${TA002_SELECT_DETAILOut} == None"/>
        <results>
          <result value="true">
            <set component-id="FRET600_TA002_ID" value=""/>
          </result>
          <result value="false">
            <set component-id="FRET600_TA002_ID" ref="TA002_SELECT_DETAILOut"/>
          </result>
        </results>
      </if>
  </event>
```
**MASTER_DETAIL_Package_Script_FRET600.sql is generated to hold all the master detail related trigger code and program unit code mentioned below.**

1. Query_Master_Details Program Unit
2. Check_Package_Failure Program Unit
3. Clear_All_Master_Details Program Unit
4. ON-POPULATE-DETAILS trigger of Master block
5. ON-CHECK-DELETE-MASTER trigger of Master block

This can be found in the src/main/script/<form-name> folder

##Post Conversion Actions
To populate detail blocks when the master record changes, you have to invoke the  event created for ON-POPULATE-DETAILS after the master record is populated. After the business action to populate master is executed, use the event-ref to invoke population of detail blocks. For example onload of the window is populating the master block.
After that we have to invoke the detail block population with proper input values from master block. (Check *LINES ADDED* comment)
```xml
  <event id="onLoadEvent_TT001_FRET600_WINDOW">
    <listeners>
      <listenergroup>
        <component ref="FRET600_WINDOW"/>
        <listener type="onload"/>
      </listenergroup>
    </listeners>
    <store target="user" name="FRET600_WINDOW_TT001_CRUD" value=""/>
    <store target="user" name="FRET600_WINDOW-$OFFSET" value="0"/>
    <store target="user" name="FRET600_WINDOW-$PAGESIZE" value="1"/>
    <store name="$OFFSET" value="0"/>
    <store name="$PAGESIZE" value="1"/>
    <business-action ref="FRET600_TT001_SELECT_BASETYPE">
      <out name="TT001_SELECT_BASETYPEOut" ref="TT001_SELECT_BASETYPEOut"/>
    </business-action>
    <set group-name="FRET600_TT001" ref="TT001_SELECT_BASETYPEOut[0]"/>

    <!-- LINES ADDED START -->
    <store name="FRET600_TT001_C_TIP_NOD" ref="FRET600_TT001_C_TIP_NOD" src="component" />
    <event ref="FRET600_TT001_BLOCKTRIGGER_ON-POPULATE-DETAILS"/>
    <!-- LINES ADDED END-->
    <set-property property="visible" value="false">
      <component ref="saveRecord"/>
      <component ref="deleteRecord"/>
      <component ref="newRecord"/>
    </set-property>
  </event>
```
MASTER_DETAIL_Package_Script_FRET600.sql  file contains all the triggers and program units related to Master-Detail handling in Oracle forms.

This code is specific to Oracle Forms and not required in QAFE Master-Detail handling.

Since these triggers can contain custom logic apart from the generic code , a manual action is required to go through this file and re-implement it in QAFE.

For example if the ON-POPULATE-DETAILS trigger code in the script generated contain some custom code ,
you can add equivalent QAFE Built-In at the end of the ```"<form-name>_<block-name>_BLOCKTRIGGER_ON-POPULATE-DETAILS"```  event to handle this.
