# TMA for Data Files

This document provides varied examples of how TMA (Text Markup) can represent common data file formats. TMA's simplicity and convention-based approach makes it suitable for many data representation scenarios.

## 1. XML Data (RSS Feed)

XML RSS feed example:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0">
  <channel>
    <title>Example Blog</title>
    <link>https://example.com</link>
    <description>A sample blog</description>
    <item>
      <title>First Post</title>
      <link>https://example.com/first</link>
      <pubDate>Mon, 01 Jan 2024 12:00:00 GMT</pubDate>
      <description>This is the first post.</description>
    </item>
    <item>
      <title>Second Post</title>
      <link>https://example.com/second</link>
      <pubDate>Tue, 02 Jan 2024 12:00:00 GMT</pubDate>
      <description>This is the second post.</description>
    </item>
  </channel>
</rss>
```

TMA representation:
```java
rss(
  version(2.0)
  channel(
    title(Example Blog)
    link(https://example.com)
    description(A sample blog)
    item(
      title(First Post)
      link(https://example.com/first)
      pubDate(Mon, 01 Jan 2024 12:00:00 GMT)
      description(This is the first post.)
    )
    item(
      title(Second Post)
      link(https://example.com/second)
      pubDate(Tue, 02 Jan 2024 12:00:00 GMT)
      description(This is the second post.)
    )
  )
)
```

## 2. YAML Data (OpenAPI Specification)

YAML OpenAPI example:
```yaml
openapi: 3.0.0
info:
  title: Sample API
  version: 1.0.0
paths:
  /users:
    get:
      summary: Returns a list of users
      responses:
        '200':
          description: A JSON array of users
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
```

TMA representation:
```java
openapi(3.0.0)
info(
  title(Sample API)
  version(1.0.0)
)
paths(
  (
    path(/users)
    get(
      summary(Returns a list of users)
      responses(
        200(
          description(A JSON array of users)
          content(
            application/json(
              schema(
                type(array)
                items($ref(#/components/schemas/User))
              )
            )
          )
        )
      )
    )
  )
)
components(
  schemas(
    User(
      type(object)
      properties(
        id(type(integer))
        name(type(string))
      )
    )
  )
)
```

## 3. TSV (Tab-Separated Values)

TSV data:
```tsv
ID	Name	Department	Salary
1	Alice	Engineering	95000
2	Bob	Marketing	85000
3	Charlie	Sales	90000
4	Diana	Engineering	110000
```

TMA representation (multiple conventions):
```java
// Convention 1: Row-based with header names
( ID(1) Name(Alice)   Department(Engineering) Salary(95000) )
( ID(2) Name(Bob)     Department(Marketing)   Salary(85000) )
( ID(3) Name(Charlie) Department(Sales)       Salary(90000) )
( ID(4) Name(Diana)   Department(Engineering) Salary(110000) )

// Convention 2: Column-based (better for large datasets)
ID(    (1) (2)     (3)       (4)     )
Name(  (Alice) (Bob) (Charlie) (Diana) )
Department( (Engineering) (Marketing) (Sales) (Engineering) )
Salary( (95000) (85000) (90000) (110000) )
```

## 4. Database Dump (SQL INSERT Statements)

SQL INSERT statements:
```sql
INSERT INTO users (id, username, email, created_at) VALUES
(1, 'alice', 'alice@example.com', '2024-01-01 10:00:00'),
(2, 'bob', 'bob@example.com', '2024-01-02 11:00:00'),
(3, 'charlie', 'charlie@example.com', '2024-01-03 12:00:00');

INSERT INTO orders (id, user_id, amount, status) VALUES
(100, 1, 49.99, 'completed'),
(101, 2, 29.99, 'pending'),
(102, 1, 99.99, 'shipped');
```

TMA representation:
```java
users(
  (id(1) username(alice)   email(alice@example.com)   created_at(2024-01-01 10:00:00))
  (id(2) username(bob)     email(bob@example.com)     created_at(2024-01-02 11:00:00))
  (id(3) username(charlie) email(charlie@example.com) created_at(2024-01-03 12:00:00))
)

orders(
  (id(100) user_id(1) amount(49.99) status(completed))
  (id(101) user_id(2) amount(29.99) status(pending))
  (id(102) user_id(1) amount(99.99) status(shipped))
)
```

## 5. Structured Log Files

JSON log entries:
```json
[
  {
    "timestamp": "2024-01-15T08:30:00Z",
    "level": "INFO",
    "message": "User logged in",
    "user_id": 123,
    "ip": "192.168.1.1"
  },
  {
    "timestamp": "2024-01-15T08:31:00Z",
    "level": "ERROR",
    "message": "Database connection failed",
    "error_code": "DB_CONN_001",
    "retry_count": 3
  },
  {
    "timestamp": "2024-01-15T08:32:00Z",
    "level": "WARN",
    "message": "High memory usage",
    "memory_usage": 85.5,
    "process": "web_server"
  }
]
```

TMA representation:
```java
(
  (timestamp(2024-01-15T08:30:00Z) level(INFO)  message(User logged in)     user_id(123) ip(192.168.1.1))
  (timestamp(2024-01-15T08:31:00Z) level(ERROR) message(Database connection failed) error_code(DB_CONN_001) retry_count(3))
  (timestamp(2024-01-15T08:32:00Z) level(WARN)  message(High memory usage) memory_usage(85.5) process(web_server))
)
```

## 6. GeoJSON Data

GeoJSON example:
```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [102.0, 0.5]
      },
      "properties": {
        "name": "Location A",
        "population": 1000
      }
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "LineString",
        "coordinates": [
          [102.0, 0.0],
          [103.0, 1.0],
          [104.0, 0.0],
          [105.0, 1.0]
        ]
      },
      "properties": {
        "name": "Route 1",
        "highway": "primary"
      }
    }
  ]
}
```

TMA representation:
```java
type(FeatureCollection)
features(
  (
    type(Feature)
    geometry(
      type(Point)
      coordinates((102.0)(0.5))
    )
    properties(
      name(Location A)
      population(1000)
    )
  )
  (
    type(Feature)
    geometry(
      type(LineString)
      coordinates(
        ((102.0)(0.0))
        ((103.0)(1.0))
        ((104.0)(0.0))
        ((105.0)(1.0))
      )
    )
    properties(
      name(Route 1)
      highway(primary)
    )
  )
)
```

## 7. Spreadsheet/Table Data

Excel-like data with multiple sheets:
```csv
Sheet: Employees
ID,Name,Department,Salary
1,Alice,Engineering,95000
2,Bob,Marketing,85000
3,Charlie,Sales,90000

Sheet: Departments
ID,Name,Budget,Location
1,Engineering,1000000,Floor 5
2,Marketing,500000,Floor 3
3,Sales,750000,Floor 4
```

TMA representation:
```java
workbook(
  Employees(
    (ID(1) Name(Alice)   Department(Engineering) Salary(95000))
    (ID(2) Name(Bob)     Department(Marketing)   Salary(85000))
    (ID(3) Name(Charlie) Department(Sales)       Salary(90000))
  )
  Departments(
    (ID(1) Name(Engineering) Budget(1000000) Location(Floor 5))
    (ID(2) Name(Marketing)   Budget(500000)  Location(Floor 3))
    (ID(3) Name(Sales)       Budget(750000)  Location(Floor 4))
  )
)
```

## 8. Nested JSON with Complex Arrays

Complex JSON data:
```json
{
  "company": "Tech Corp",
  "departments": [
    {
      "name": "Engineering",
      "employees": [
        {
          "id": 1,
          "name": "Alice",
          "skills": ["Java", "Python", "Docker"],
          "projects": [
            {"name": "Project A", "status": "active"},
            {"name": "Project B", "status": "completed"}
          ]
        },
        {
          "id": 2,
          "name": "Bob",
          "skills": ["JavaScript", "React", "Node.js"],
          "projects": [
            {"name": "Project C", "status": "active"}
          ]
        }
      ]
    },
    {
      "name": "Sales",
      "employees": [
        {
          "id": 3,
          "name": "Charlie",
          "skills": ["Communication", "Negotiation"],
          "projects": []
        }
      ]
    }
  ]
}
```

TMA representation:
```java
company(Tech Corp)
departments(
  (
    name(Engineering)
    employees(
      (
        id(1)
        name(Alice)
        skills((Java) (Python) (Docker))
        projects(
          (name(Project A) status(active))
          (name(Project B) status(completed))
        )
      )
      (
        id(2)
        name(Bob)
        skills((JavaScript) (React) (Node.js))
        projects(
          (name(Project C) status(active))
        )
      )
    )
  )
  (
    name(Sales)
    employees(
      (
        id(3)
        name(Charlie)
        skills((Communication) (Negotiation))
        projects()
      )
    )
  )
)
```

## 9. Key-Value Store Dump

Redis-like key-value data:
```
user:1:name = "Alice"
user:1:email = "alice@example.com"
user:1:age = "30"
user:2:name = "Bob"
user:2:email = "bob@example.com"
user:2:age = "25"
session:abc123:user_id = "1"
session:abc123:expires = "2024-01-16T12:00:00Z"
cache:homepage:html = "<html>...</html>"
```

TMA representation:
```java
kvstore(
  (key(user:1:name)    value(Alice))
  (key(user:1:email)   value(alice@example.com))
  (key(user:1:age)     value(30))
  (key(user:2:name)    value(Bob))
  (key(user:2:email)   value(bob@example.com))
  (key(user:2:age)     value(25))
  (key(session:abc123:user_id) value(1))
  (key(session:abc123:expires) value(2024-01-16T12:00:00Z))
  (key(cache:homepage:html) value(<html>...</html>))
)
```

## 10. Time Series Data

Time series measurements:
```
timestamp,metric,value
2024-01-15T00:00:00Z,cpu_usage,45.2
2024-01-15T00:01:00Z,cpu_usage,47.8
2024-01-15T00:02:00Z,cpu_usage,43.1
2024-01-15T00:00:00Z,memory_usage,65.3
2024-01-15T00:01:00Z,memory_usage,66.1
2024-01-15T00:02:00Z,memory_usage,64.8
```

TMA representation:
```java
timeseries(
  (timestamp(2024-01-15T00:00:00Z) metric(cpu_usage)    value(45.2))
  (timestamp(2024-01-15T00:01:00Z) metric(cpu_usage)    value(47.8))
  (timestamp(2024-01-15T00:02:00Z) metric(cpu_usage)    value(43.1))
  (timestamp(2024-01-15T00:00:00Z) metric(memory_usage) value(65.3))
  (timestamp(2024-01-15T00:01:00Z) metric(memory_usage) value(66.1))
  (timestamp(2024-01-15T00:02:00Z) metric(memory_usage) value(64.8))
)
```

