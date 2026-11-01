# W01 Assignment Notes

### GET

Request:

```http
GET http://localhost:5085/pizza/
Accept: application/json
```

Response:

```http
HTTP/1.1 200 OK
Connection: close
Content-Type: application/json; charset=utf-8
Date: Sun, 01 Nov 2026 22:31:22 GMT
Server: Kestrel
Transfer-Encoding: chunked

[
  {
    "id": 1,
    "name": "Classic Italian",
    "isGlutenFree": false
  },
  {
    "id": 2,
    "name": "Veggie",
    "isGlutenFree": true
  },
  {
    "id": 3,
    "name": "BBQ Chicken",
    "isGlutenFree": false
  }
]
```

### POST

Request:

```http
POST http://localhost:5085/pizza/
Content-Type: application/json

{
    "name": "Hawaii",
    "isGlutenFree": false
}
```

Response:

```http
HTTP/1.1 201 Created
Connection: close
Content-Type: application/json; charset=utf-8
Date: Sun, 01 Nov 2026 22:31:52 GMT
Server: Kestrel
Location: http://localhost:5085/Pizza/4
Transfer-Encoding: chunked

{
  "id": 4,
  "name": "Hawaii",
  "isGlutenFree": false
}
```

### PUT

Request:

```http
PUT http://localhost:5085/pizza/3
Content-Type: application/json

{
    "id": 3,
    "name": "Hawaiian",
    "isGlutenFree": false
}
```

Response:

```http
HTTP/1.1 204 No Content
Connection: close
Date: Sun, 01 Nov 2026 22:32:10 GMT
Server: Kestrel
```

Response body: empty (204 No Content).

### DELETE

Request:

```http
DELETE http://localhost:5085/pizza/4
```

Response:

```http
HTTP/1.1 204 No Content
Connection: close
Date: Sun, 01 Nov 2026 22:32:21 GMT
Server: Kestrel
```

Response body: empty (204 No Content).

## Sales summary function

```csharp
void GenerateSalesSummary(
    IEnumerable<string> salesFiles,
    string storesDirectory,
    string salesTotalDir)
{
    var details = new StringBuilder();
    double totalSales = 0;

    foreach (var file in salesFiles)
    {
        string salesJson = File.ReadAllText(file);
        SalesData? data = JsonConvert.DeserializeObject<SalesData>(salesJson);
        double fileTotal = data?.Total ?? 0;

        totalSales += fileTotal;

        string filename = Path.GetRelativePath(storesDirectory, file);
        details.AppendLine($" {filename}: {fileTotal:C}");
    }

    var report = new StringBuilder();
    report.AppendLine("Sales Summary");
    report.AppendLine("----------------------------");
    report.AppendLine($" Total Sales: {totalSales:C}");
    report.AppendLine();
    report.AppendLine(" Details:");
    report.Append(details);

    File.WriteAllText(
        Path.Combine(salesTotalDir, "sales-summary.txt"),
        report.ToString());
}
```
