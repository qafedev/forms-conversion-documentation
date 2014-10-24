
<h1><strong>Triggers Overview</strong></h1>
<p>&nbsp;</p>
<table>
<tbody>
<tr>
<th>Trigger</th>
<th colspan="1">Description</th>
<th>Form</th>
<th>Block</th>
<th>Item</th>
<th colspan="1">Comments</th></tr>
<tr>
<td>Post-Block</td>
<td colspan="1">Fires during the Leave the Block process.</td>
<td>N/A</td>
<td>Not Converted</td>
<td>N/A</td>
<td colspan="1">Block: Block is converted as Panel inside QAFE and there is no on-exit event for Panel.</td></tr>
<tr>
<td>Post-Delete</td>
<td colspan="1">
<p>Fires just after a record is deleted from the database. It tires once</p>
<p>for each row that is deleted from the database during the<br />commit process.</p></td>
<td>N/A</td>
<td>Can Be Converted</td>
<td>N/A</td>
<td colspan="1">Block: It can be converted for single record block and for multi-record block we have to refactor the iteration in the event and not in the business-action.</td></tr>
<tr>
<td>Post-Form</td>
<td colspan="1">Fires when a form is exited.</td>
<td>??</td>
<td>N/A</td>
<td>N/A</td>
<td colspan="1">Form: To converted to on-unload event for Window in QAFE?</td></tr>
<tr>
<td colspan="1">Post-Forms-Commit</td>
<td colspan="1">
<p>If there are records in the form that have been marked as inserts, updates, or deletes,</p>
<p>the Post-Forms-Commit trigger fires after these changes have been written to the database</p>
<p>but before Oracle Forms issues the database Commit to finalize the transaction. If the operator</p>
<p>or the application initiates a Commit when there are no records in the form have been marked as inserts, updates, or deletes, Oracle Forms fires the Post-Forms-Commit trigger immediately, without posting changes to the database.</p></td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Form: Two phase commit is not supported by QAFE.</td></tr>
<tr>
<td colspan="1">Post-Insert</td>
<td colspan="1">
<p>Fires just after a record is inserted into the database. It fires once for each record that is inserted into the database during<br />the commit process.</p></td>
<td colspan="1">N/A</td>
<td colspan="1">Can Be Converted</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Block: It can be converted for single record block and for multi-record block we have to refactor the iteration in the event and not in the business-action.</span></td></tr>
<tr>
<td colspan="1">Post-Query</td>
<td colspan="1">Fires each time Oracle Forms fetches a record into a block.</td>
<td colspan="1">N/A</td>
<td colspan="1">Can Be Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Block: It can be converted for single-record block and for multi-record block it should be done manually.</td></tr>
<tr>
<td colspan="1">Post-Record</td>
<td colspan="1">Fires whenever the focus moves from one record to another.</td>
<td colspan="1">N/A</td>
<td colspan="1">Can Be Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Block: For single-record block it can be done in navigation events in QAFE. For multi-record block we can use on-click event for the Datagrid Column.</td></tr>
<tr>
<td colspan="1">Post-Text-Item</td>
<td colspan="1">Fires when the input focus moves from a text item to any other item</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Can Be Converted</span></td>
<td colspan="1">Item: It can be converted to in on-exit event for Text-field.</td></tr>
<tr>
<td colspan="1">Post-Update</td>
<td colspan="1">
<p>Fires just after a record is updated in the database. It fires once for each row that is updated in the database during the<br />commit process.</p></td>
<td colspan="1">N/A</td>
<td colspan="1">Can Be Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Block: It can be converted for single record block and for multi-record block we have to refactor the iteration in the event and not in the business-action.</td></tr>
<tr>
<td colspan="1">Pre-Block</td>
<td colspan="1">Fires during navigation from one block to another</td>
<td colspan="1"><span>NA</span></td>
<td colspan="1">Not Converted</td>
<td colspan="1"><span>NA</span></td>
<td colspan="1">Block: Navigation to other Panel is not supported by QAFE.</td></tr>
<tr>
<td colspan="1">Pre-Commit</td>
<td colspan="1">
<p>Fires after Oracle Forms determines that there are inserts, updates, or deletes in the form to post or commit. The trigger<br />does not fire when there is an attempt to commit, but validation determines that there are no changed records in the form.</p></td>
<td colspan="1">N/A</td>
<td colspan="1">Can Be Converted</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Block: It can be converted for single record block and for multi-record block we have to refactor the iteration in the event and not in the business-action.</span></td></tr>
<tr>
<td colspan="1">Pre-Delete</td>
<td colspan="1">
<p>Fires just before a record is deleted from the database. It fires once for each record that is marked for delete. Note: Oracle<br />Forms creates a Pre-Delete trigger automatically for any master-detail relation that has the Master Deletes property set to<br />Cascading.</p></td>
<td colspan="1">N/A</td>
<td colspan="1">Can Be Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Block: It can be converted for single record block and for multi-record block we have to refactor the iteration in the event and not in the business-action.</td></tr>
<tr>
<td colspan="1">Pre-Form</td>
<td colspan="1">Fires at form startup.</td>
<td colspan="1">Can be Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Form: It can be converted to onload-event for Window.</td></tr>
<tr>
<td colspan="1">Pre-Insert</td>
<td colspan="1">Fires just before a record is inserted into the database. It fires once for each record that is marked for insert</td>
<td colspan="1">N/A</td>
<td colspan="1">Can Be Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Block: It can be converted for single record block and for multi-record block we have to refactor the iteration in the event and not in the business-action.</td></tr>
<tr>
<td colspan="1">Pre-Query</td>
<td colspan="1">Fires just before Oracle Forms constructs and issues the SELECT statement to identify rows that match the query criteria.</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Block: Is not supported by QAFE.</span></td></tr>
<tr>
<td colspan="1">Pre-Record</td>
<td colspan="1">Fires during navigation to a different record.</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Block: Navigation to other Panel is not supported by QAFE.</span></td></tr>
<tr>
<td colspan="1">Pre-Text-Item</td>
<td colspan="1">Fires during navigation from an item to a text item.</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Can be Converted</td>
<td colspan="1">Item: It can be converted to onfocus event.</td></tr>
<tr>
<td colspan="1">Pre-Update</td>
<td colspan="1">Fires just before a record is updated in the database. It fires once for each record that is marked for update</td>
<td colspan="1">N/A</td>
<td colspan="1">Can Be Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Block: It can be converted for single record block and for multi-record block we have to refactor the iteration in the event and not in the business-action.</td></tr>
<tr>
<td colspan="1">When-Button-Pressed</td>
<td colspan="1">Fires when an operator selects a button, either by way of a key, or by clicking with a mouse.</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Can be converted</td>
<td colspan="1">Item: It can be converted to onclick event.</td></tr>
<tr>
<td colspan="1">When-Checkbox-Changed</td>
<td colspan="1">
<p>Fires whenever an operator changes the state of a check box, either by clicking with the mouse, or through keyboard<br />interaction.</p></td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Can be Converted</td>
<td colspan="1">Item: It can be converted to onchange event.</td></tr>
<tr>
<td colspan="1">When-Clear-Block</td>
<td colspan="1">
<p>Fires just before Oracle Forms clears the data from the current block. Note that the When-Clear-Block trigger does not fire<br />when Oracle Forms clears the current block during the CLEAR_FORM event.</p></td>
<td colspan="1">N/A</td>
<td colspan="1">??</td>
<td colspan="1">N/A</td>
<td colspan="1">Block: It can be converted by using a clear record button for single-record block. For multi-record block ??</td></tr>
<tr>
<td colspan="1">When-Create-Record</td>
<td colspan="1">Fires whenever Oracle Forms creates a new record.</td>
<td colspan="1">N/A</td>
<td colspan="1">??</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Block: It can be converted by using a new record button for single-record block. For multi-record block ??</span></td></tr>
<tr>
<td colspan="1">When-List-Activated</td>
<td colspan="1">Fires when an operator double-clicks on an element in a list item that is displayed as a T-list</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">It's used in tree component but we will not convert it.</td></tr>
<tr>
<td colspan="1">When-List-Changed</td>
<td colspan="1">
<p>Fires when an operator selects a different element in a list item or de-selects the currently selected element. In addition, if a<br />When-List-Changed trigger is attached to a combo box style list item, it fires each time the operator enters or modifies<br />entered text.</p></td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Can be Converted</td>
<td colspan="1">Item: It can be converted to onchange event.</td></tr>
<tr>
<td colspan="1">When-New-Block-Instance</td>
<td colspan="1">
<p>Fires after navigation to an item, when Oracle Forms is ready to accept input in a block that is different than the block that<br />previously had input focus.</p></td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Is not supported in QAFE.</td></tr>
<tr>
<td colspan="1">When-New-Form-Instance</td>
<td colspan="1">&nbsp;</td>
<td colspan="1">Can be Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Form: It can be converted to onload-event for Window.</td></tr>
<tr>
<td colspan="1">When-New-Item-Instance</td>
<td colspan="1">Fires when the input focus moves to an item.</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Can be Converted</td>
<td colspan="1">Item: It can be converted to onfocus event.</td></tr>
<tr>
<td colspan="1">When-New-Record-Instance</td>
<td colspan="1">Fires when the input focus moves to an item in a record that is different than the record that previously had input focus.</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">&nbsp;</td></tr>
<tr>
<td colspan="1">When-Radio-Changed</td>
<td colspan="1">
<p>Fires when an operator selects a different radio button in a radio group, or de-selects the currently selected radio button,<br />either by clicking with the mouse, or by way of keyboard selection commands.</p></td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Can be Converted</td>
<td colspan="1">Item: It can be converted to onchange event.</td></tr>
<tr>
<td colspan="1">When-Remove-Record</td>
<td colspan="1">Fires whenever the operator or the application clears or deletes a record.</td>
<td colspan="1">N/A</td>
<td colspan="1">??</td>
<td colspan="1">N/A</td>
<td colspan="1">Block: It can be converted by using a clear record button for single-record block. For multi-record block ??</td></tr>
<tr>
<td colspan="1">When-Validate-Item</td>
<td colspan="1">Fires as the last part of item validation for items with the New or Changed validation status.</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Can be Converted</td>
<td colspan="1">Item:It can be converted to onchange event.</td></tr>
<tr>
<td colspan="1">When-Validate-Record</td>
<td colspan="1">Fires as the last part of record validation for records with the New or Changed validation status.</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Is not supported in QAFE.</td></tr>
<tr>
<td colspan="1">When-Window-Closed</td>
<td colspan="1">Fires when an operator closes a window using a window-manager specific Close command.</td>
<td colspan="1">Can be Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Form: It can be converted to onload-event for Window.</td></tr>
<tr>
<td colspan="1">When-Mouse-DoubleClick</td>
<td colspan="1"><span style="color: rgb(0,0,0);">Fires after the operator clicks the mouse</span></td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Can be Converted</td>
<td colspan="1">Item: It can be converted to dblclick event.</td></tr>
</tr></tbody></table>
<p><strong style="font-size: 20.0px;line-height: 1.5;">Key Triggers</strong></p>
<table>
<tbody>
<tr>
<th>Key Trigger</th>
<th>Description</th>
<th>Form</th>
<th>Block</th>
<th>Item</th>
<th>Comments</th></tr>
<tr>
<td>Key-CLRBLK</td>
<td>Clear Block</td>
<td>N/A</td>
<td>Not Converted</td>
<td>N/A</td>
<td>Not Supported in QAFE.</td></tr>
<tr>
<td>Key-CLRFRM</td>
<td>Clear Form</td>
<td>Not Converted</td>
<td>N/A</td>
<td>N/A</td>
<td><span>Not Supported in QAFE.</span></td></tr>
<tr>
<td>Key-CLRREC</td>
<td>Clear Record</td>
<td>N/A</td>
<td>Not Converted</td>
<td>N/A</td>
<td>Not Supported in QAFE.</td></tr>
<tr>
<td colspan="1">Key-COMMIT</td>
<td colspan="1">
<p>Accept</p></td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Function Not clear.</td></tr>
<tr>
<td colspan="1">Key-CQUERY</td>
<td colspan="1">Count Query Hits</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Not supported in QAFE. Should be done manually.</td></tr>
<tr>
<td colspan="1">Key-CREREC</td>
<td colspan="1">Insert Record</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Not supported in QAFE. Should be done manually.</td></tr>
<tr>
<td colspan="1">Key-DELREC</td>
<td colspan="1">Delete Record</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Not supported in QAFE. Should be done manually.</td></tr>
<tr>
<td colspan="1">Key-DOWN</td>
<td colspan="1">Down</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Function Not clear.</span></td></tr>
<tr>
<td colspan="1">Key-DUP-ITEM</td>
<td colspan="1">Duplicate Item</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Function Not clear.</td></tr>
<tr>
<td colspan="1">Key-DUPREC</td>
<td colspan="1">Duplicate Record</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Function Not clear.</td></tr>
<tr>
<td colspan="1">Key-EDIT</td>
<td colspan="1">Edit</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Function Not clear.</span></td></tr>
<tr>
<td colspan="1">Key-ENTQRY</td>
<td colspan="1">Enter Query</td>
<td colspan="1">N/A</td>
<td colspan="1">Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Block: is already implement</td></tr>
<tr>
<td colspan="1">Key-EXEQRY</td>
<td colspan="1">Execute Query</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Not supported in QAFE. Should be done manually.</td></tr>
<tr>
<td colspan="1">Key-EXIT</td>
<td colspan="1">Exit</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Function Not clear.</td></tr>
<tr>
<td colspan="1">Key-HELP</td>
<td colspan="1">Help</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Not supported in QAFE. Should be done manually.</td></tr>
<tr>
<td colspan="1">Key-LISTVAL</td>
<td colspan="1">List of Values</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Converted</td>
<td colspan="1">Is already implemented as a ondblclick event.</td></tr>
<tr>
<td colspan="1">Key-MENU</td>
<td colspan="1">Block Menu</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Function Not clear.</span></td></tr>
<tr>
<td colspan="1">Key-NXTBLK</td>
<td colspan="1">Next Block</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Block: Navigation through Panels is not supported in QAFE.</td></tr>
<tr>
<td colspan="1">Key-NXT-ITEM</td>
<td colspan="1">Next Item</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">Item: Standard behavior in browser using the TAB key.</td></tr>
<tr>
<td colspan="1">Key-NXTKEY</td>
<td colspan="1">Next Primary</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1"><span>Function Not clear.</span></td></tr>
<tr>
<td colspan="1">Key-NXTREC</td>
<td colspan="1">Next Record</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Function Not clear.</span></td></tr>
<tr>
<td colspan="1">Key-NXTSET</td>
<td colspan="1">Next Set of Records</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Function Not clear.</td></tr>
<tr>
<td colspan="1">Key-PRINT</td>
<td colspan="1">Print</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Function Not clear.</span></td></tr>
<tr>
<td colspan="1">Key-PRVBLK</td>
<td colspan="1">Previous Block</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Block: Navigation through Panels is not supported in QAFE.</span></td></tr>
<tr>
<td colspan="1">Key-PRV-ITEM</td>
<td colspan="1">Previous Item</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">Item:<span>Standard behavior in browser using the SHIFT + TAB key.</span></td></tr>
<tr>
<td colspan="1">Key-PRVREC</td>
<td colspan="1">Previous Record</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Function Not clear.</td></tr>
<tr>
<td colspan="1">Key-SCRDOWN</td>
<td colspan="1">Scroll Down</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1"><span>Function Not clear.</span></td></tr>
<tr>
<td colspan="1">Key-SCRUP</td>
<td colspan="1">Scroll Up</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Function Not clear.</td></tr>
<tr>
<td colspan="1">Key-UP</td>
<td colspan="1">Up</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">N/A</td>
<td colspan="1">Function Not clear.</td></tr>
<tr>
<td colspan="1">Key-UPDREC</td>
<td colspan="1">Equivalent to Record | Lock command on the default menu</td>
<td colspan="1">N/A</td>
<td colspan="1">Not Converted</td>
<td colspan="1">N/A</td>
<td colspan="1">Function Not clear.</td></tr></tbody></table>
<p>&nbsp;</p>
<h1><strong>Handling Multi-Record block Triggers</strong></h1>
<p>In this section we explain the way to handle the multi-record block.</p>
<p>Currently we handle the CRUD operation as follow by iterating the CRUD method in the business-tier as follow.</p>

	<business-tier>
	    <business-action id="MENTCB01_MENTCDC_UPDATE_BASETYPE_Iterator">
	        <transaction managed="no"/>
	        <iteration begin="0" increment="1" items-ref="MENTCDC_UPDATE_BASETYPEIn_List" var="MENTCDC_UPDATE_BASETYPEIn" var-index="index">
	          <service ref="MENTCB01" method-ref="MENTCDC_UPDATE_BASETYPE">
	            <in name="MENTCDC_UPDATE_BASETYPEIn" ref="MENTCDC_UPDATE_BASETYPEIn"/>
	            <out name="result" ref="result"/>
	          </service>
	        </iteration>
	    </business-action>
	</business-tier>

<p>Handling the triggers for pre/post can be done by calling the method created for trigger before/after calling the CRUD method. So suppose you want to call &quot;pre-insert&quot; before and &quot;post-insert&quot; after calling your CRUD method:</p>

	<business-tier>
	    <business-action id="MENTCB01_MENTCDC_UPDATE_BASETYPE_Iterator">
	        <transaction managed="no"/>
	        <iteration begin="0" increment="1" items-ref="MENTCDC_UPDATE_BASETYPEIn_List" var="MENTCDC_UPDATE_BASETYPEIn" var-index="index">

	          <service ref="MENTCB01" method-ref="MENTCDC_PRE_INSERT_BASETYPE">
	            <in name="MENTCDC_PRE_INSERT_BASETYPEIn" ref="MENTCDC_PRE_INSERT_BASETYPEIn"/>
	            <out name="result" ref="result"/>
	          </service>

	          <service ref="MENTCB01" method-ref="MENTCDC_UPDATE_BASETYPE">
	            <in name="MENTCDC_UPDATE_BASETYPEIn" ref="MENTCDC_UPDATE_BASETYPEIn"/>
	            <out name="result" ref="result"/>
	          </service>

	          <service ref="MENTCB01" method-ref="MENTCDC_POST_INSERT_BASETYPE">
	            <in name="MENTCDC_POST_INSERT_BASETYPEIn" ref="MENTCDC_POST_INSERT_BASETYPEIn"/>
	            <out name="result" ref="result"/>
	          </service>

	        </iteration>
	    </business-action>
	</business-tier>
