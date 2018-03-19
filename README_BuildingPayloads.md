# Easy-Link V2 - Building Payloads

## Overview

Building the payload may be the most critical step in the API process. This is where you map your desired data into our defined data structure. If this isn't set up properly, we may be interpreting your data in an entirely different manner than you might have expected. In some circumstances, if your payload isn't formatted correctly our service may entirely discard your request and return an error.

## Payload Basics

- The payload must be a JSON string. 
- The payload must be wrapped in a containing object (Objects are represented by '{ }').
- The payload must have a nested Array called LineItems (Arrays are represented by '[ ]') that will house all material specific data.
- The payload must contain all required fields for the desired payload type. (This is only true for *POST* requests. *README_SendingRequests.md* explains more about *PATCH* requests.)
  - A list of all available and required fields may be found in the [***REF_AllFields.md***](REF_AllFields.md) file.
  - Example minimum and full payloads may also be seen in the [***REF_PayloadExamples.md***](REF_PayloadExamples.md) file.

#### Simplified Payload

```json
{
    "Freight_CarrierService" : "RINCHEM",
    "ShipFrom_WarehouseCode" : "16",
    "ShipTo_Street1" : "123 Secure Drive",
    "OwnerCode" : "RINCHEM",
    "SupplierCode" : "EXA",

    "LineItems" : 
    [
        {
            "RecordLine_Name" : "1",
            "Quantity" : "4",
            "Product_RinchemPartNumber" : "101234_EXA",
            "UnitOfMeasure" : "BOTTLE",
            "LotNumber" : "M6G162MP",
        }
    ]
}
```

## Design Considerations

### Object To JSON Serialization Methods

Although most languages will require your final HTTP payload to be a string, it is highly advised that an object representation be built first. Most languages and platforms provide libraries to serialize your object representation into a JSON string. By building an object first rather than jumping straight to the string allows changes to be made much easier, it also allows more checks to be done on your side prior to sending the payload, which means less errors and less risk. Also, it is just a cleaner and much more readable and expandable approach.

#### C##

```C#
//Create User object.  
Payload payload = new Payload();  

//Create a stream to serialize the object to.  
MemoryStream ms = new MemoryStream();  

// Serializer the payload object to the stream.  
DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(Payload));  
ser.WriteObject(ms, payload);  
byte[] json = ms.ToArray();  
ms.Close();  
String payloadString = Encoding.UTF8.GetString(json, 0, json.Length);    

```
More information about C# serialization may be found at https://docs.microsoft.com/en-us/dotnet/framework/wcf/feature-details/how-to-serialize-and-deserialize-json-data

#### Java

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;

public class SimpleExample1 {
    public static void main(String[] args) {
        Payload payload = new Payload();
        
        Gson gson = new Gson();
        String payloadString = gson.toJson(payload);
    }
}
```

More information about the **gson** library can be found at See https://github.com/google/gson 

#### JavaScript

```javascript
var payload = { "Request":{ ... } };
var payloadString = JSON.stringify(payload);
```
