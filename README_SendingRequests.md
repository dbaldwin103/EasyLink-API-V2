# Easy-Link V2 - Easy Link API

## The Rinchem API Call

At this point you should have already already:

-  Gone through the authentication process and retrieved the *access_token* and *instance_url*. 
  - If you haven't please see the *README_Authenticating.md*
- Created your json body that contains the information you would like sent to Rinchem.
  - If you haven't please see the [*README_BuildingPayloads.md*](README_BuildingPayloads.md)

If you have completed these items, then you are ready to call the EasyLink API. The overall format of the API call is shown below.

### HTTP Example

```http
<your_verb> <your_api_suffix> HTTP/1.1
Host: <your_instance_url>
Authorization: Bearer <your_access_token>
Accept: application/json
Content-Type: application/json

<your_json_payload>
```

***your_verb*** will let Rinchem know what we should do with your payload.  

>**POST** should be used for any new payloads. It will add a record to Rinchem's system.
>**PATCH** should be used to update or cancel a prior payload. It will modify a record in Rinchem's system.
>**GET** should be used to retrieve a record from Rinchem's system.

***your_api_suffix*** will let Rinchem know what type of request you are about to send, either a Facility To Warehouse (F2W) or a Warehouse To Facility (W2F).

> **/services/apexrest/v1/W2F** - will let us know your request is referencing a Warehouse To Facility record.
> **/services/apexrest/v1/F2W** - will let us know your request is referencing a Facility To Warehouse record.

***your_instance_url*** and ***your_access_token*** should have been retrieved and stored during the authentication process. ***your_json_body*** should have been created during the building payloads process.

Depending on the Verb request type the response(s) you receive may vary.



## POST (new)

A post call will try to create a new record in Salesforce. If the authentication tokens are still valid and the JSON content body is valid, a response will be returned with the following "success" detail:

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

A *P*ATCH *Update* call does not need to send the full payload! If only the *DesiredDeliveryDate* needs to be modified, then only that field needs to be sent. For example:

```http
PATCH <your_api_suffix>?Record_Name=<your_record_name>&amp;Action=Update HTTP/1.1
Host: <your_instance_url>
Authorization: Bearer <your_access_token>
Accept: application/json
Content-Type: application/json

{
	"Request":
	{
        "DesiredDeliverDate": "11-04-2018"
	}
}
```

Note: **Fields may not be updated to *null*!** Any field that needs a value removed should be assigned an empty string.

### Cancel

A *PATCH Cancel* call does not need to send any payload! Simply one of the record names and action cancel. For example:

```http
PATCH <your_api_suffix>?Record_ExternalName=<your_record_external_name>&amp;Action=Cancel HTTP/1.1
Host: <your_instance_url>
Authorization: Bearer <your_access_token>
Accept: application/json
Content-Type: application/json
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



## Unsuccessful Responses (Error Handling)

