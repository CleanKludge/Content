# TableQueryParser #

TableQueryParser provides extension methods to the Microsoft Azure TableQuery class which allows them to be run against objects.

[![https://github.com/Korthax/TableQueryParser](https://img.shields.io/badge/github-TableQueryParser-brightgreen.svg)](https://github.com/Korthax/TableQueryParser)
[![NuGet Version](http://img.shields.io/nuget/v/TableQueryParser.svg?style=flat)](https://www.nuget.org/packages/TableQueryParser/)


## Dependencies

* WindowsAzure.Storage v8.1.1

## Installation

```
Install-Package TableQueryParser
```

## Usage
### Via extension methods

```csharp
var filter = TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith");
var query = new TableQuery<TableEntity>().Where(filter);
var isMatch = rangeQuery.Validate(new TableEntity { PartitionKey = "Smith", RowKey = "ZZZ" }); // Returns true
```

```csharp
var data = new List<TableEntity>
{
    new TableEntity { PartitionKey = "Smith", RowKey = "Adam" },
    new TableEntity { PartitionKey = "Phillips", RowKey = "Steve" },
    new TableEntity { PartitionKey = "Phillips", RowKey = "Molly" },
    new TableEntity { PartitionKey = "Phillips", RowKey = "E" },
    new TableEntity { PartitionKey = "Jones", RowKey = "Quinton" }
};

var partitionKeyFilter = TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Phillips");
var rowKeyFilter = TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E");

var combineFilters = TableQuery.CombineFilters(partitionKeyFilter, TableOperators.And, rowKeyFilter);
var rangeQuery = new TableQuery<TableEntity>().Where(combineFilters);

var filteredResults = rangeQuery.Filter(data); // Returns the Steve Phillips and Molly Phillips table entities
```

### Directly

```csharp
var filter = TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith");
var query = new TableQuery<TableEntity>().Where(filter);
var isMatch = QueryParser.Validate(query.FilterString, new TableEntity { PartitionKey = "Smith", RowKey = "ZZZ" }); // Returns true
```

```csharp
var data = new List<TableEntity>
{
    new TableEntity { PartitionKey = "Smith", RowKey = "Adam" },
    new TableEntity { PartitionKey = "Phillips", RowKey = "Steve" },
    new TableEntity { PartitionKey = "Phillips", RowKey = "Molly" },
    new TableEntity { PartitionKey = "Phillips", RowKey = "E" },
    new TableEntity { PartitionKey = "Jones", RowKey = "Quinton" }
};

var partitionKeyFilter = TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Phillips");
var rowKeyFilter = TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E");

var combineFilters = TableQuery.CombineFilters(partitionKeyFilter, TableOperators.And, rowKeyFilter);
var rangeQuery = new TableQuery<TableEntity>().Where(combineFilters);

var filteredResults = QueryParser.Filter(rangeQuery.FilterString, data); // Returns the Steve Phillips and Molly Phillips table entities
```

### Further examples

For further example please see the integration tests [here](https://github.com/Korthax/TableQueryParser/tree/master/tests/TableQueryParser.Tests/Integration).


## General Notes
**This is an initial version and not tested thoroughly**.

I've made this library for my own use when mocking out Azure's table storage during testing. User at your own risk :)

## License ##

TableQueryParser is released under the [MIT license](https://github.com/Korthax/TableQueryParser/blob/master/LICENSE.md).