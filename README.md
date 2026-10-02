

1. En el método testTransformWithAlternativeStrategy (Imagen 6)

```java
    // CAMBIO AQUÍ: Reemplaza la concatenación por Text Block
    String jsonInput = """
        {
         "applicationReferenceNumber": "REF-456",
         "applicationFiles": [
          {
           "fileMetadata": { "documentType": "cbfIdentification", "customerId": "CUST-2", "curp": "CURP123456" },
           "documentInfo": { "documentStoragePath": "/storage/id.pdf", "fileName": "id.pdf" }
          }
         ]
        }
        """;
```

2. En el método testTransformArrayWithStrategies (Imagen 7)

```java
    // CAMBIO AQUÍ: Reemplaza la concatenación por Text Block
    String jsonInput = """
        {
         "applicationReferenceNumber": "REF-999",
         "applicationFiles": [
          {
           "fileMetadata": { "documentType": "cbfVotingCard", "customerId": "CUST-1", "curr": "MXN", "documentExpirationDate": "2030-01-01", "documentStatus": "ACTIVE" },
           "documentInfo": { "documentStoragePath": "/storage/ine.pdf", "fileName": "ine.pdf" }
          }
         ]
        }
        """;
```

3. En el método testTransformConcurrentWithExecutorService (Imagen 11)

```java
    // CAMBIO AQUÍ: Reemplaza la concatenación por Text Block
    String jsonInput = """
        {
         "applicationReferenceNumber": "REF-CONCURRENT",
         "applicationFiles": [
          { "fileMetadata": { "documentType": "cbfPassport", "customerId": "CUST-X", "curp": "CURPX" }, "documentInfo": { "documentStoragePath": "/p.pdf", "fileName": "p.pdf" } }
         ]
        }
        """;

