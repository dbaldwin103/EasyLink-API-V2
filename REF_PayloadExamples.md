## Payload Samples (for *Post* requests)

#### Table of Contents

> [Minimum F2W Payload](#minimum-facility-to-warehouse-f2w-payload)<br/>
> [Minimum W2F Payload](#minimum-warehouse-to-facility-w2f-payload)<br/>
> [Full F2W Payload](#full-f2w-payload)<br/>
> [Full W2F Payload](#full-w2f-payload)<br/>
> [Full Generic Payload](#full-generic-payload)<br/>



## Minimum Facility To Warehouse (F2W) Payload

```json
{
    "Request" : 
    {
        "OwnerCode" : "OWN",
        "SupplierCode" : "SUP",
        "EstimatedShipDate" : "10-22-2018",
        "Freight_CarrierService" : "RINCHEM",
        "Freight_BillTo_Type" : "Requester",
        "Freight_IsInternational" : "FALSE",
        "ShipTo_WarehouseCode" : "11",
        "ShipFrom_Name" : "John Doe",
        "ShipFrom_Street1" : "123 Example Street",
        "ShipFrom_City" : "Albuquerque",
        "ShipFrom_State" : "NM",
        "ShipFrom_PostalCode" : "87109",
        "ShipFrom_Country" : "USA",
    	
        "LineItems" : 
        [
            {
                "Record_LineNumber" : 1,
                "ProductNumber_Owner" : "OWN1234",
                "LotNumber" : "12345",
                "Quantity" : 5,
                "UnitOfMeasure" : "BOTTLE" 
            },
        ]
    }
}
```

## Minimum Warehouse To Facility (W2F) Payload

```json
{
    "Request" : 
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
            },
        ]
    }
}
```

## Full F2W Payload

```json
{
    "Request" : 
    {
        "Record_ExternalName" : "EXA12345",
        "OwnerCode" : "OWN",
        "SupplierCode" : "SUP",
        "EstimatedShipDate" : "10-22-2018",
        "PurchaseOrderNumber" : "PO1234",
        "AdditionalOrderNumber" : "ADDITIONAL1234",
        "AdditionalShipmentComments" : "This is a test payload",
        "Requester_Email" : "requester@email.com",
        "Requester_Phone" : "1-555-123-4567",
        "SecondaryContact_FirstName" : "John",
        "SecondaryContact_LastName" : "Doe",
        "SecondaryContact_Company" : "EXAMPLE CO",
        "SecondaryContact_Phone" : "1-555-765-4321",
        "SecondaryContact_Email" : "john.doe@example.com",
        "Freight_CarrierService" : "RINCHEM",
        "Freight_CarrierService_AccountNumber" : "12345",
        "Freight_BillTo_Type" : "Requester",
        "Freight_BillTo_Name" : "John Doe",
        "Freight_BillTo_Company" : "EXAMPLE CO",
        "Freight_BillTo_Street1" : "123 Example St",
        "Freight_BillTo_Street2" : "",
        "Freight_BillTo_Street3" : "",
        "Freight_BillTo_City" : "Albuquerque",
        "Freight_BillTo_State" : "NM",
        "Freight_BillTo_PostalCode" : "87109",
        "Freight_BillTo_Country" : "USA",
        "Freight_IsInternational" : "FALSE",
        "Freight_International_ImporterOfRecord" : "",
        "Freight_MethodOfTransport" : "GROUND",
        "ShipTo_WarehouseCode" : "11",
        "ShipFrom_Name" : "John Doe",
        "ShipFrom_Company" : "EXAMPLE CO",
        "ShipFrom_Street1" : "123 Example Street",
        "ShipFrom_Street2" : "",
        "ShipFrom_Street3" : "",
        "ShipFrom_City" : "Albuquerque",
        "ShipFrom_State" : "NM",
        "ShipFrom_PostalCode" : "87109",
        "ShipFrom_Country" : "USA",
    	
        "LineItems" : 
        [
            {
                "Record_LineNumber" : 1,
                "ProductNumber_Owner" : "OWN1234",
                "ProductNumber_Supplier" : "SUP1234",
                "ProductNumber_Rinchem" : "1234_EXAMPLE",
                "Quantity" : 5,
                "LotNumber" : "12345A",
                "SerialNumber" : "54321A",
                "UnitOfMeasure" : "BOTTLE",
                "PurchaseOrderNumber" : "PO1234",
                "AdditionalComments" : "This is a test line.",
                "Status" : "VH",
                "Status_Reason" : "Needs verification",
                "Attributes_Destination" : "D12",
                "Attributes_Process" : "1272",
                "Attributes_Other" : "IO,PQ",
                "Attributes_ComponentStatus" : "NT"
            },
        ]
    }
}
```

## Full W2F Payload

```json
{
    "Request" : 
    {
        "Record_ExternalName" : "EXA12345",
        "OwnerCode" : "OWN",
        "SupplierCode" : "SUP",
        "EstimatedShipDate" : "10-22-2018",
        "PurchaseOrderNumber" : "PO1234",
        "AdditionalOrderNumber" : "ADDITIONAL1234",
        "AdditionalShipmentComments" : "This is a test payload",
        "Requester_Email" : "requester@email.com",
        "Requester_Phone" : "1-555-123-4567",
        "SecondaryContact_FirstName" : "John",
        "SecondaryContact_LastName" : "Doe",
        "SecondaryContact_Company" : "EXAMPLE CO",
        "SecondaryContact_Phone" : "1-555-765-4321",
        "SecondaryContact_Email" : "john.doe@example.com",
        "Freight_CarrierService" : "RINCHEM",
        "Freight_CarrierService_AccountNumber" : "12345",
        "Freight_BillTo_Type" : "Requester",
        "Freight_BillTo_Name" : "John Doe",
        "Freight_BillTo_Company" : "EXAMPLE CO",
        "Freight_BillTo_Street1" : "123 Example St",
        "Freight_BillTo_Street2" : "",
        "Freight_BillTo_Street3" : "",
        "Freight_BillTo_City" : "Albuquerque",
        "Freight_BillTo_State" : "NM",
        "Freight_BillTo_PostalCode" : "87109",
        "Freight_BillTo_Country" : "USA",
        "Freight_IsInternational" : "FALSE",
        "Freight_International_ImporterOfRecord" : "",
        "Freight_MethodOfTransport" : "GROUND",
        "ShipTo_WarehouseCode" : "11",
        "ShipFrom_Name" : "John Doe",
        "ShipFrom_Company" : "EXAMPLE CO",
        "ShipFrom_Street1" : "123 Example Street",
        "ShipFrom_Street2" : "",
        "ShipFrom_Street3" : "",
        "ShipFrom_City" : "Albuquerque",
        "ShipFrom_State" : "NM",
        "ShipFrom_PostalCode" : "87109",
        "ShipFrom_Country" : "USA",
    	
        "LineItems" : 
        [
            {
                "Record_LineNumber" : 1,
                "ProductNumber_Owner" : "OWN1234",
                "ProductNumber_Supplier" : "SUP1234",
                "ProductNumber_Rinchem" : "1234_EXAMPLE",
                "Quantity" : 5,
                "LotNumber" : "12345A",
                "UnitOfMeasure" : "BOTTLE",
                "PurchaseOrderNumber" : "PO1234",
                "AdditionalComments" : "This is a test line."
            },
        ]
    }
}
```

## Full Generic Payload

```json
{
    "Request" : 
    {
        "Record_ExternalName" : "",
        "OwnerCode" : "",
        "SupplierCode" : "",
        "EstimatedShipDate" : "",
        "PurchaseOrderNumber" : "",
        "AdditionalOrderNumber" : "",
        "AdditionalShipmentComments" : "",
        "Requester_Email" : "",
        "Requester_Phone" : "",
        "SecondaryContact_FirstName" : "",
        "SecondaryContact_LastName" : "",
        "SecondaryContact_Company" : "",
        "SecondaryContact_Phone" : "",
        "SecondaryContact_Email" : "",
        "Freight_CarrierService" : "",
        "Freight_CarrierService_AccountNumber" : "",
        "Freight_BillTo_Type" : "",
        "Freight_BillTo_Name" : "",
        "Freight_BillTo_Company" : "",
        "Freight_BillTo_Street1" : "",
        "Freight_BillTo_Street2" : "",
        "Freight_BillTo_Street3" : "",
        "Freight_BillTo_City" : "",
        "Freight_BillTo_State" : "",
        "Freight_BillTo_PostalCode" : "",
        "Freight_BillTo_Country" : "",
        "Freight_IsInternational" : "",
        "Freight_International_ImporterOfRecord" : "",
        "Freight_MethodOfTransport" : "",
        "ShipFrom_WarehouseCode" : "",
        "ShipTo_WarehouseCode" : "",
        "ShipFrom_Name" : "",
        "ShipFrom_Company" : "",
        "ShipFrom_Street1" : "",
        "ShipFrom_Street2" : "",
        "ShipFrom_Street3" : "",
        "ShipFrom_City" : "",
        "ShipFrom_State" : "",
        "ShipFrom_PostalCode" : "",
        "ShipFrom_Country" : "",
        "ShipTo_Name" : "",
        "ShipTo_Company" : "",
        "ShipTo_Street1" : "",
        "ShipTo_Street2" : "",
        "ShipTo_Street3" : "",
        "ShipTo_City" : "",
        "ShipTo_State" : "",
        "ShipTo_PostalCode" : "",
        "ShipTo_Country" : "",

        "LineItems" : 
        [
            {
                "Record_LineNumber" : ,
                "ProductNumber_Owner" : "",
                "ProductNumber_Supplier" : "",
                "ProductNumber_Rinchem" : "",
                "Quantity" : ,
                "LotNumber" : "",
                "SerialNumber" : "",
                "UnitOfMeasure" : "",
                "PurchaseOrderNumber" : "",
                "AdditionalComments" : "",
                "Status" : "",
                "Status_Reason" : "",
                "Attributes_Destination" : "",
                "Attributes_Process" : "",
                "Attributes_Other" : "",
                "Attributes_ComponentStatus" : ""
            },
        ]
    }
}
```
