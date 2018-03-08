# Easy-Link V2 - Fields

#### Table Of Contents

> [Overview](#overview)
>
> [Header Fields](#header-fields)
> > [Record Fields](#record-fields)<br/>
> > [General Fields](#general-fields)<br/>
> > [Contact Fields](#contact-fields)<br/>
> > [Freight Fields](#freight-fields)<br/>
> > [Ship To Fields](#ship-to-fields)<br/>
> > [Ship From Fields](#ship-from-fields)<br/>
>
> [Line Item Fields](#line-item-fields)
> > [Record Fields](#record-fields-line)<br/>
> > [Product Number Fields](#product-number-fields-line)<br/>
> > [General Fields](#general-fields-line)<br/>
> > [Status and Attribute Fields](#status-and-attribute-fields-line)<br/>

## Overview

To ensure ease of use for all our integrating customers, we have created a set of generic fields with a consistent and intuitive naming structure, to collect your data. To simplify the process of both sending and receiving the payload, we have opted to use the same payload for both Facility to Warehouse (F2W) and Warehouse to Facility (W2F) requests. 

The available fields are listed below in the given tables. They are separated into related groups in order to make their meaning and relationship more apparent. Items that aren't entirely self apparent are given descriptions in subsequent text. If you have any issues understanding what data you might need to map in to a specific field, please don't hesitate to ask your Rinchem contact.

The ***Format*** field shows the data type that will be accepted by the API.
​	**Text(Z)** signifies that any ascii character string with *Z* characters or less will be accepted.
​	**Integer(Z)** signifies that any whole numerical value with *Z* digits or less will be accepted.
​	**Date** signifies that any date in the format of '**mm-dd-yyyy**' will be accepted.
​	**Boolean** signifies that either 'true' or 'false' should be provided.
​	**Picklist** signifies that this is not a free-form field and certain Rinchem approved values must be provided. Look to the description text to find more information about the acceptable values.

The ***F2W*** and ***W2F*** columns show the availability of the field for the desired request type. 
​	'**+**' signifies that the field is available for use, but not required. 
​	'*****' signifies that the field is both available and required. 
​	'**x**' signifies that the field is not available and will be ignored by the API.



## Header Fields

### Record Fields

| F2W  | W2F  | Field Name          | Format   | Example   |
| ---- | ---- | ------------------- | -------- | --------- |
| +    | +    | Record_Name         | Text(25) | IMP000015 |
| +    | +    | Record_ExternalName | Text(25) | 1234EXA   |

The **Record_Name** is a Salesforce generated value that will be returned upon successful order creation. It may be stored and used to reference the order for any *patch* or *get* requests, however, **should not be included in any post requests**.

The **Record_ExternalName** on the other hand is a user created unique id/name. It may be passed in on post requests, and may be used to reference the order for any *patch* or *get* requests. The *Record_ExternalName* must be unique amongst all orders amongst all Chem-Star users. If a *post* request is sent with a non-unique value, the order will be discarded and an error response will be sent.

### General Fields

| F2W  | W2F  | Field Name                 | Format    | Example        |
| ---- | ---- | -------------------------- | --------- | -------------- |
| *    | \*   | OwnerCode                  | Picklist  | RIN            |
| \*   | \*   | SupplierCode               | Picklist  | RIN            |
| x    | \*   | DesiredDeliveryDate        | Date      | 10-22-2018     |
| \*   | x    | EstimatedShipDate          | Date      | 08-26-2018     |
| +    | +    | PurchaseOrderNumber        | Text(50)  | PO1234         |
| +    | +    | AdditionalOrderNumber      | Text(50)  | ADDITIONAL1234 |
| +    | +    | AdditionalShipmentComments | Text(255) |                |

The **OwnerCode** specifies the owner of the material at the time the request is sent. If ownership changes after sending, an update request should be sent. The **SupplierCode** specifies who distributes or manufactures the material. Both *OwnerCode* and *SupplierCode* must match a specific value in Rinchem's system. Please ask your Rinchem contact for a list of relevant owner and supplier codes to use.

The **DesiredDeliverDate** describes when you would like the material to arrive at your facility; it should **only be used for Outbound Orders**. 

The **EstimatedShipDate** similarly describes when you expect the material to leave your facility; it should **only be used for Inbound Orders**.

The **PurchaseOrderNumber** is the purchase order number that pertains to the request. It will travel with the material on the BOL. Alternatively, the *PurchaseOrderNumber* may be provided at the line item level if necessary. The **AdditionalOrderNumber** may be used to store any secondary order numbers that you would like associated with the request. It will live entirely on Salesforce and will not be passed on to our WMS or appear on any forms.

**AdditionalShipmentComments** may be used for any header level information that you would like stored that we haven't provided a specific field for.  This lives only on Salesforce and will not be passed on to our WMS or appear on any forms.

### Contact Fields

| F2W  | W2F  | Field Name                 | Format   | Example                                             |
| ---- | ---- | -------------------------- | -------- | --------------------------------------------------- |
| +    | +    | Requester_Email            | Text(70) | [requester@email.com](mailto:requester@email.com)   |
| +    | +    | Requester_Phone            | Text(70) | 1-555-123-4567                                      |
| +    | +    | SecondaryContact_FirstName | Text(70) | John                                                |
| +    | +    | SecondaryContact_LastName  | Text(70) | Doe                                                 |
| +    | +    | SecondaryContact_Company   | Text(70) | EXAMPLE CO                                          |
| +    | +    | SecondaryContact_Phone     | Text(70) | 1-555-123-4567                                      |
| +    | +    | SecondaryContact_Email     | Text(70) | [john.doe@example.com](mailto:john.doe@example.com) |

The **Requester** and **SecondaryContact** fields are entirely optional. They simply provide Rinchem with contacts to reach out to if there happens to be an issue with the order. There is no guarantee that they will be notified of the order or any order updates. The *Requester* fields should be used to describe the person sending the request payload, if these fields are left blank, the values will be populated with information pulled from your Salesforce profile. The *SecondaryContact* should be used to describe anybody else of concern, this may potentially be an approver, or a facility engineer, or perhaps somebody that you are submitting the order on behalf of. These fields live on Salesforce only and will not be passed to our WMS or appear on any forms.

### Freight Fields

| F2W  | W2F  | Field Name                           | Format    | Example |
| ---- | ---- | ------------------------------------ | --------- | ------- |
| *    | *    | Freight_CarrierService               | Picklist  | RINCHEM |
| +    | +    | Freight_CarrierService_AccountNumber | Text(100) | 12345   |

The **Freight_CarrierService** specifies the company that will be transporting your material. You will need to use Rinchem values for this field, please ask your Rinchem contact for a list of relevant carrier service codes. **Freight_CarrierService_AccountNumber** is going to be the account number that you have open with the specified *Freight_CarrierService*.

| F2W  | W2F  | Field Name                | Format   | Example            |
| ---- | ---- | ------------------------- | -------- | ------------------ |
| *    | *    | Freight_BillTo_Type       | Picklist | Recipient          |
| +    | +    | Freight_BillTo_Name       | Text(40) | John Doe           |
| +    | +    | Freight_BillTo_Company    | Text(40) | EXAMPLE CO         |
| +    | +    | Freight_BillTo_Street1    | Text(30) | 123 Example Street |
| +    | +    | Freight_BillTo_Street2    | Text(30) |                    |
| +    | +    | Freight_BillTo_Street3    | Text(30) |                    |
| +    | +    | Freight_BillTo_City       | Text(20) | Albuquerque        |
| +    | +    | Freight_BillTo_State      | Text(20) | NM                 |
| +    | +    | Freight_BillTo_PostalCode | Text(10) | 87109              |
| +    | +    | Freight_BillTo_Country    | Text(20) | USA                |

The **Freight_BillTo** fields describe who is responsible for the freight charges. **Freight_BillTo_Type** is a picklist that will accept 1 of the 3 following values; **Requester**, **Recipient**, or **Third Party**. If *Requester* is selected, this indicates that, you, the person sending the payload is responsible for payments. If *Recipient* is selected, this indicates that the *ShipTo* recipient will be responsible for the payment. If *Third Party* is selected then address described in the subsequent *Freight_BillTo* address fields will be responsible for payment.

| F2W | W2F | Field Name | Format | Example |
| -- | -- | -------------------------------------- | -------- | ------------------------------|
| *    | *    | Freight_IsInternational                | Boolean  | TRUE          |
| +    | +    | Freight_International_ImporterOfRecord | Text(40) | EXAMPLE OWNER |
| +    | +    | Freight_MethodOfTransport              | Picklist | OCEAN         |

**Freight_IsInternational** lets us know if the shipment will be crossing any international borders. If it is an international shipment, **Freight_International_ImporterOfRecord** then tells us who will be responsible for the material once it reaches customs.

**Freight_MethodOfTransport** specifies how the material will be transported. The field is available for both domestic and international shipments, though it's primary purpose is to specify whether an international shipment will be sent by *OCEAN* or *AIR*. Domestic shipments currently may only specify *GROUND* or leave it blank.

### Ship To Fields

| F2W | W2F | Field Name | Format | Example |
| ---- | ---- | -------------------------------------- | -------- | ------------------------------|
| *    | x    | ShipTo_WarehouseCode | Picklist | 16 |
| x    | *    | ShipTo_Name       | Text(40) | John Doe           |
| x    | +    | ShipTo_Company    | Text(40) | EXAMPLE CO         |
| x    | *    | ShipTo_Street1    | Text(30) | 123 Example Street |
| x    | +    | ShipTo_Street2    | Text(30) |                    |
| x    | +    | ShipTo_Street3    | Text(30) |                    |
| x    | *    | ShipTo_City       | Text(20) | Albuquerque        |
| x    | *    | ShipTo_State      | Text(20) | NM                 |
| x    | *    | ShipTo_PostalCode | Text(10) | 87109              |
| x    | *    | ShipTo_Country    | Text(20) | USA                |

The **ShipTo** fields let us know the final destination for your requested material. The **ShipTo_WarehouseCode** should only be used in the case of a F2W request and must use a Rinchem recognized warehouse code, please ask your Rinchem contact for a list of relevant warehouse codes.  The other *ShipTo* fields should only be used in the case of a W2F request.

### Ship From Fields

| F2W | W2F | Field Name | Format | Example |
| ---- | ---- | -------------------------------------- | -------- | ------------------------------|
| x    | *    | ShipFrom_WarehouseCode | Picklist | 11 |
| *    | x    | ShipFrom_Name       | Text(40) | John Doe           |
| +    | x    | ShipFrom_Company    | Text(40) | EXAMPLE CO         |
| *    | x    | ShipFrom_Street1    | Text(30) | 123 Example Street |
| +    | x    | ShipFrom_Street2    | Text(30) |                    |
| +    | x    | ShipFrom_Street3    | Text(30) |                    |
| *    | x    | ShipFrom_City       | Text(20) | Albuquerque        |
| *    | x    | ShipFrom_State      | Text(20) | NM                 |
| *    | x    | ShipFrom_PostalCode | Text(10) | 87109              |
| *    | x    | ShipFrom_Country    | Text(20) | USA                |

The **ShipFrom** fields let us know the material's origin of shipment. The **ShipTo_WarehouseCode** should only be used in the case of a F2W request and must use a Rinchem recognized warehouse code, please ask your Rinchem contact for a list of relevant warehouse codes.  The other *ShipFrom* fields should only be used in the case of a F2W request.



## Line Item Fields

### Record Fields (line)

| F2W  | W2F  | Field Name        | Format     | Example |
| ---- | ---- | ----------------- | ---------- | ------- |
| x    | *    | Record_LineNumber | Integer(5) | 1       |

The **RecordLine_Number** is a required field that allows you to reference the line item in *update* and *get* calls. Each value must be unique to the order, meaning two lines could not both have a *RecordLine_Number* of '1'. If this occurs, the payload will be discarded and an error will be returned.

### General Fields (line)

| F2W  | W2F  | Field Name          | Format     | Example |
| ---- | ---- | ------------------- | ---------- | ------- |
| *    | *    | Quantity            | Integer(5) | 6       |
| *    | *    | LotNumber           | Text(20)   | 12345   |
| +    | x    | SerialNumber        | Text(50)   | 54321   |
| *    | *    | UnitOfMeasure       | Picklist   | BOTTLE  |
| +    | +    | PurchaseOrderNumber | Text(25)   | PO1234  |
| +    | +    | AdditionalComments  | Text(50)   |         |

**LotNumber** tells us which lot we will be picking/receiving for this request. 

**UnitOfMeasure** tells us what unit of measure the *Quantity* field is referencing; this must match what is set up in our product table and must be a Rinchem recognized value. Please see *README_UnitOfMeasure.md* for more information. 

**PurchaseOrderNumber** tells us what purchase order number applies specifically to this line of material. It should only be provided if it differs from the header level PurchaseOrderNumber.

**AdditionalComments** is used to store any additional information pertaining to this line of material that we didn't explicitly provide a field for. It currently lives exclusively on Salesforce and will not appear on any forms.

### Product Number Fields (line)

| F2W  | W2F  | Field Name             | Format   | Example       |
| ---- | ---- | ---------------------- | -------- | ------------- |
| +    | +    | ProductNumber_Rinchem  | Text(25) | 12345_EXAMPLE |
| +    | +    | ProductNumber_Owner    | Text(25) | OWN1234       |
| +    | +    | ProductNumber_Supplier | Text(25) | SUP1234       |

The **ProductNumber** fields specify the part number of the product that is being shipped. Any of the three may be sent in, if any of them are found, the other 2 will be populated base on our alias table. If none of them are found, the payload will be discarded and an error will be returned.

### Status and Attribute Fields (line)

| F2W  | W2F  | Field Name                 | Format   | Example            |
| ---- | ---- | -------------------------- | -------- | ------------------ |
| +    | x    | Status                     | Picklist | VH                 |
| +    | x    | Status_Reason              | Text(25) | Needs verification |
| +    | x    | Attributes_Destination     | Text(30) | D12                |
| +    | x    | Attributes_Process         | Text(30) | 1272               |
| +    | x    | Attributes_Other           | Text(30) | IO,PQ              |
| +    | x    | Attributes_ComponentStatus | Text(30) | NT                 |

The **Status** and **Attributes** fields are only available for F2W requests. They let us know what state the material is in when we receive it in our warehouse.

The **Status** field lets us know the hold code that is associated with the order. For example, sending VH, would let us know that the material is on vendor hold. The **Status_Reason** field would then be used to let us know why. The **Attributes** fields on the other hand are allocations applied to the material. 

Both *Status* and *Attributes* must use Rinchem accepted values. Please see README_StatusAndAttributes.md for more information.
