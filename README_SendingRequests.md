# Easy-Link V2 - Sending Requests

## Overview

At this point you should have already:

-  Gone through the authentication process and retrieved an *access_token* and *instance_url*. 
  - If you haven't please see the [*README_Authenticating.md*](README_Authenticating.md)
- Created your json body that contains the information you would like sent to Rinchem.
  - If you haven't please see the [*README_BuildingPayloads.md*](README_BuildingPayloads.md)

If you have completed these items, then you are ready to call the EasyLink API. The overall format of the API call is shown below.

#### HTTP Example

```http
<your_verb> <your_api_suffix> HTTP/1.1
Host: <your_instance_url>
Authorization: Bearer <your_access_token>
Accept: application/json
Content-Type: application/json

<your_json_payload>
```

***your_verb*** will let Rinchem know what we should do with your payload.  

>**POST** should be used for any *new* payloads. It will add a record to Rinchem's system.
>**PATCH** should be used to *update* or *cancel* a prior payload. It will modify a record in Rinchem's system.
>**GET** should be used to *retrieve* a record from Rinchem's system.

***your_api_suffix*** will let Rinchem know what type of request you are about to send, either an *Inbound* or an *Outbound* order.

> **/services/apexrest/v2/outbound** - will let us know your request is referencing an Outbound record.
> **/services/apexrest/v2/inbound** - will let us know your request is referencing an Inbound record.

***your_instance_url*** and ***your_access_token*** should have been retrieved and stored during the authentication process. 

***your_json_body*** should have been created during the building payloads process.



## POST (new)

A post call will try to create a new record in Salesforce. It doesn't require any additional parameters to be passed in, it simply requires the payload. For example:

```http
POST <your_api_suffix> HTTP/1.1
Host: <your_instance_url>
Authorization: Bearer <your_access_token>
Accept: application/json
Content-Type: application/json

{
    "OwnerCode" : "OWN",
    "SupplierCode" : "SUP",
    "DesiredDeliveryDate" : "10-22-2018",
    "Freight_CarrierService" : "RINCHEM",
    "Freight_BillTo_Type" : "Requester",
    "Freight_IsInternational" : "FALSE",
    "ShipFrom_WarehouseCode" : "11",
    "ShipTo_Name" : "John Doe",
    "ShipTo_Street1" : "123 Example Street",
    "ShipTo_City" : "Albuquerque",
    "ShipTo_State" : "NM",
    "ShipTo_PostalCode" : "87109",
    "ShipTo_Country" : "USA",
    
    "LineItems" : 
    [
        {
            "Record_LineNumber" : 1,
            "ProductNumber_Owner" : "OWN1234",
            "LotNumber" : "12345",
            "Quantity" : 5,
            "UnitOfMeasure" : "BOTTLE" 
        }
    ]
}
```



If the authentication tokens are still valid and the JSON content body is valid, a response will be returned with the following "success" detail:

```
{
    "status":"Success",
    "message":"Your request has been imported successfully", 
    "record_name" : <your_record_name>
}
```

***your_record_name*** is a unique record identifier that is created by Salesforce. It is highly recommended that this be stored somewhere, as it would be passed in as an HTML parameter if an update/cancel/get operation were needed to be performed on the record. Alternatively, a *Record_ExternalName* could be passed in during post and used for the same purpose.



## PATCH (update and cancel)

A PATCH call will try to find an existing record in Salesforce, either by Record_Name or Record_ExternalName, and then try to update the payload with a new value or cancel the order (depending on what parameter you pass in). Depending on when you sent your request and the status that it currently has in Rinchem's system, we may no longer be able to handle an update/cancel and an error will be returned to you.

Two url parameters are required to send a *PATCH* call:  

- Either **Record_Name** (returned from a post call), or **Record_ExternalName** (passed in to the post call).
- And **Action** which will accept a value of either ***Cancel*** or ***Update***  

### Update

A *PATCH Update* call does not need to send the full payload! If only the *DesiredDeliveryDate* needs to be modified, then only that field needs to be sent. For example:

```http
PATCH <your_api_suffix>?Record_Name=<your_record_name>&amp;Action=Update HTTP/1.1
Host: <your_instance_url>
Authorization: Bearer <your_access_token>
Accept: application/json
Content-Type: application/json

{
    "DesiredDeliverDate": "11-04-2018"
}
```

**Note: Fields may not be updated to *null*! Any field that needs a value removed should be assigned an empty string!!!**

If the authentication tokens are still valid and the JSON content body is valid, a response will be returned with the following "success" detail:

```
{
    "status":"Success",
    "message":"Your request has been updated successfully", 
    "record_name" : <your_record_name>
}
```

### Cancel

A *PATCH Cancel* call does not need to send any payload! It simply requires the 2 parameters; one of the record names and Action=Cancel.  For example:

```http
PATCH <your_api_suffix>?Record_ExternalName=<your_record_external_name>&amp;Action=Cancel HTTP/1.1
Host: <your_instance_url>
Authorization: Bearer <your_access_token>
Accept: application/json
Content-Type: application/json
```

Note: Cancels are not reversible, if for some reason the order shouldn't have been cancelled the full payload will need to be sent again.  

If the authentication tokens are still valid and the request is valid, a response will be returned with the following "success" detail:

```
{
    "status":"Success",
    "message":"Your request has been cancelled successfully", 
    "record_name" : <your_record_name>
}
```

### Cancel and Update Time Frame

Cancels and updates may only be processed while the order is still in the staging status in our warehouse management system. If the order has moved past that status, your *PATCH* request will return a failed response with the following detail:

```
{
    "status":"Fail",
    "message":"We were unable to handle your PATCH request due to the current status of your order. If this is an issue please reach out to your Rinchem contact.", 
    "record_name" : <your_record_name>
}
```



## GET

A *GET* call, similar to *PATCH Cancel* doesn't need to send a payload, rather it will receive a payload in response. It requires only one parameter be passed in, either **Record_Name** or **Record_ExternalName**.

In the future it will accept a second parameter allowing you to retrieve either the full record payload, or the record status.

```http
GET <your_api_suffix>?Record_ExternalName=<your_record_external_name> HTTP/1.1
Host: <your_instance_url>
Authorization: Bearer <your_access_token>
Accept: application/json
Content-Type: application/json
```

A successful *GET* call will return the following "Success" detail:

```
{
    "status":"Success",
    "message":"Your record has been retrieved successfully", 
    "record" : <your_record_payload>
}
```

***your_record_payload*** will be in the same format as the payload you sent in. 
