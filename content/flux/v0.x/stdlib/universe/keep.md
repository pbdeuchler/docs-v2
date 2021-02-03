---
title: keep() function
description: The `keep()` function returns a table containing only the specified columns.
aliases:
  - /influxdb/v2.0/reference/flux/functions/transformations/keep
  - /influxdb/v2.0/reference/flux/functions/built-in/transformations/keep/
  - /influxdb/v2.0/reference/flux/stdlib/built-in/transformations/keep/
  - /influxdb/cloud/reference/flux/stdlib/built-in/transformations/keep/
menu:
  flux_0_x_ref:
    name: keep
    parent: universe
weight: 102
introduced: 0.7.0
---

The `keep()` function returns a table containing only the specified columns, ignoring all others.
Only columns in the group key that are also specified in the `keep()` function will be kept in the resulting group key.
_It is the inverse of [`drop`](/flux/v0.x/stdlib/universe/drop)._

_**Function type:** Transformation_  
_**Output data type:** Record_

```js
keep(columns: ["col1", "col2"])

// OR

keep(fn: (column) => column =~ /inodes*/)
```

## Parameters

{{% note %}}
Make sure `fn` parameter names match each specified parameter. To learn why, see [Match parameter names](/flux/v0.x/spec/data-model/#match-parameter-names).
{{% /note %}}

### columns

Columns that should be included in the resulting table.
Cannot be used with `fn`.

_**Data type:** Array of strings_

### fn

A predicate function which takes a column name as a parameter (`column`) and returns
a boolean indicating whether or not the column should be included in the resulting table.
Cannot be used with `columns`.

_**Data type:** Function_

## Examples

##### Keep a list of columns

```js
from(bucket: "example-bucket")
    |> range(start: -5m)
    |> keep(columns: ["_time", "_value"])
```

##### Keep all columns matching a predicate

```js
from(bucket: "example-bucket")
    |> range(start: -5m)
    |> keep(fn: (column) => column =~ /inodes*/)
```