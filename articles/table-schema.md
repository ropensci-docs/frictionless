# Table Schema

[Table Schema](https://datapackage.org/standard/table-schema/) is a
simple format to describe tabular data, including field names, types,
constraints, missing values, foreign keys, etc.

In this document we use the terms “package” for Data Package, “resource”
for Data Resource, “dialect” for Table Dialect, and “schema” for Table
Schema.

## General implementation

frictionless supports `schema$fields` and `schema$missingValues` to
parse data types and missing values when reading [Tabular Data
Resources](https://datapackage.org/standard/data-resource/#tabular).
Schema manipulation is limited to getting a schema from a resource,
creating one from a data frame, and providing one back to a resource.
Schema metadata is including when writing a package.

### Access

[`schema()`](https://docs.ropensci.org/frictionless/reference/schema.md)
gets the schema from a resource:

``` r

library(frictionless)
package <- example_package()

# Get the Table Schema for the resource "observations"
schema <- schema(package, "observations")
str(schema)
#> List of 5
#>  $ fields       :List of 7
#>   ..$ :List of 3
#>   .. ..$ name       : chr "observation_id"
#>   .. ..$ type       : chr "string"
#>   .. ..$ constraints:List of 2
#>   .. .. ..$ required: logi TRUE
#>   .. .. ..$ unique  : logi TRUE
#>   ..$ :List of 3
#>   .. ..$ name       : chr "deployment_id"
#>   .. ..$ type       : chr "string"
#>   .. ..$ constraints:List of 1
#>   .. .. ..$ required: logi TRUE
#>   ..$ :List of 4
#>   .. ..$ name       : chr "timestamp"
#>   .. ..$ type       : chr "datetime"
#>   .. ..$ format     : chr "%Y-%m-%dT%H:%M:%S%z"
#>   .. ..$ constraints:List of 1
#>   .. .. ..$ required: logi TRUE
#>   ..$ :List of 3
#>   .. ..$ name       : chr "scientific_name"
#>   .. ..$ type       : chr "string"
#>   .. ..$ constraints:List of 1
#>   .. .. ..$ required: logi FALSE
#>   ..$ :List of 3
#>   .. ..$ name       : chr "count"
#>   .. ..$ type       : chr "integer"
#>   .. ..$ constraints:List of 2
#>   .. .. ..$ required: logi FALSE
#>   .. .. ..$ minimum : int 1
#>   ..$ :List of 5
#>   .. ..$ name             : chr "life_stage"
#>   .. ..$ type             : chr "string"
#>   .. ..$ categories       :List of 5
#>   .. .. ..$ : chr "adult"
#>   .. .. ..$ : chr "subadult"
#>   .. .. ..$ : chr "juvenile"
#>   .. .. ..$ : chr "offspring"
#>   .. .. ..$ : chr "unknown"
#>   .. ..$ categoriesOrdered: logi FALSE
#>   .. ..$ constraints      :List of 2
#>   .. .. ..$ required: logi FALSE
#>   .. .. ..$ enum    :List of 5
#>   .. .. .. ..$ : chr "adult"
#>   .. .. .. ..$ : chr "subadult"
#>   .. .. .. ..$ : chr "juvenile"
#>   .. .. .. ..$ : chr "offspring"
#>   .. .. .. ..$ : chr "unknown"
#>   ..$ :List of 3
#>   .. ..$ name       : chr "comments"
#>   .. ..$ type       : chr "string"
#>   .. ..$ constraints:List of 1
#>   .. .. ..$ required: logi FALSE
#>  $ $schema      : chr "https://datapackage.org/profiles/2.0/tableschema.json"
#>  $ missingValues:List of 1
#>   ..$ : chr "NA"
#>  $ primaryKey   :List of 1
#>   ..$ : chr "observation_id"
#>  $ foreignKeys  :List of 1
#>   ..$ :List of 2
#>   .. ..$ fields   :List of 1
#>   .. .. ..$ : chr "deployment_id"
#>   .. ..$ reference:List of 2
#>   .. .. ..$ resource: chr "deployments"
#>   .. .. ..$ fields  :List of 1
#>   .. .. .. ..$ : chr "deployment_id"
```

### Read

[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
uses the information in `schema$fields` to parse the names and data
types of the columns in a tabular data file. For example, the third
field in the schema above (`timestamp`) is defined as a datetime `type`
with a specific `format`:

``` r

str(schema$fields[[3]])
#> List of 4
#>  $ name       : chr "timestamp"
#>  $ type       : chr "datetime"
#>  $ format     : chr "%Y-%m-%dT%H:%M:%S%z"
#>  $ constraints:List of 1
#>   ..$ required: logi TRUE
```

[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
uses that information to correctly parse the data type and to assign the
name `timestamp` to the column:

``` r

observations <- read_resource(package, "observations")
observations$timestamp
#> [1] "2020-09-28 00:13:07 UTC" "2020-09-28 15:59:17 UTC"
#> [3] "2020-09-28 16:35:23 UTC" "2020-09-28 17:04:04 UTC"
#> [5] "2020-09-28 19:19:54 UTC" "2021-10-01 01:25:06 UTC"
#> [7] "2021-10-01 01:25:06 UTC" "2021-10-01 04:47:30 UTC"
```

The sixth field `life_stage` has an `enum` defined as one of its
`constraints`:

``` r

str(schema$fields[[6]])
#> List of 5
#>  $ name             : chr "life_stage"
#>  $ type             : chr "string"
#>  $ categories       :List of 5
#>   ..$ : chr "adult"
#>   ..$ : chr "subadult"
#>   ..$ : chr "juvenile"
#>   ..$ : chr "offspring"
#>   ..$ : chr "unknown"
#>  $ categoriesOrdered: logi FALSE
#>  $ constraints      :List of 2
#>   ..$ required: logi FALSE
#>   ..$ enum    :List of 5
#>   .. ..$ : chr "adult"
#>   .. ..$ : chr "subadult"
#>   .. ..$ : chr "juvenile"
#>   .. ..$ : chr "offspring"
#>   .. ..$ : chr "unknown"
```

[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
uses that information to parse the column as a factor, using `enum` as
the factor levels:

``` r

class(observations$life_stage)
#> [1] "factor"
levels(observations$life_stage)
#> [1] "adult"     "subadult"  "juvenile"  "offspring" "unknown"
```

### Manipulate

A schema is a list which you can manipulate, but frictionless does not
provide functions to do that. Use [purrr](https://purrr.tidyverse.org/)
or base R instead (see
[`vignette("frictionless")`](https://docs.ropensci.org/frictionless/articles/frictionless.md)).
You do not have to start a schema from scratch though: use
[`schema()`](https://docs.ropensci.org/frictionless/reference/schema.md)
(see above) or
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
instead.

[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
creates a schema from a data frame. **It always creates a schema
following the [v2
specification](https://datapackage.org/standard/table-schema/).** It
defines the `name`, `type` (and if a factor `constraints$enum`) for each
field:

``` r

# Create a schema from the built-in dataset "iris"
iris_schema <- create_schema(iris)
str(iris_schema)
#> List of 2
#>  $ $schema: chr "https://datapackage.org/profiles/2.0/tableschema.json"
#>  $ fields :List of 5
#>   ..$ :List of 2
#>   .. ..$ name: chr "Sepal.Length"
#>   .. ..$ type: chr "number"
#>   ..$ :List of 2
#>   .. ..$ name: chr "Sepal.Width"
#>   .. ..$ type: chr "number"
#>   ..$ :List of 2
#>   .. ..$ name: chr "Petal.Length"
#>   .. ..$ type: chr "number"
#>   ..$ :List of 2
#>   .. ..$ name: chr "Petal.Width"
#>   .. ..$ type: chr "number"
#>   ..$ :List of 3
#>   .. ..$ name       : chr "Species"
#>   .. ..$ type       : chr "string"
#>   .. ..$ constraints:List of 1
#>   .. .. ..$ enum:List of 3
#>   .. .. .. ..$ : chr "setosa"
#>   .. .. .. ..$ : chr "versicolor"
#>   .. .. .. ..$ : chr "virginica"
```

[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
allows to include the schema with a resource. If no schema is provided,
one is created with
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md):

``` r

package <- add_resource(
  package,
  resource_name = "iris",
  data = iris,
  schema = iris_schema
)
```

### Write

[`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
writes a package to disk as a `datapackage.json` file. This file
includes the metadata of all the resources, including the schema. To
directly write a schema to disk, use
[`jsonlite::write_json()`](https://jeroen.r-universe.dev/jsonlite/reference/read_json.html).

## Schema properties implementation

### \$schema

[`$schema`](https://datapackage.org/standard/table-schema/#dollar-schema)
indicates what
[`version()`](https://docs.ropensci.org/frictionless/reference/version.md)
of the Table Schema standard is used (v1 if undefined).

- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  ignores it and does not upgrade a package, since it does not rely on
  v1 properties deprecated in v2.
- `create_scheme()` sets `$schema` to the recommended v2 value (
  (`"https://datapackage.org/profiles/2.0/tableschema.json"`) and thus
  creates a v2 schema.
- `upgrade_package()` sets `$schema` to the recommended v2 value for all
  resources of a package.

### fields

[`fields`](https://datapackage.org/standard/table-schema/#fields) is
required. It is used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
to parse the names and data types of the columns in a tabular data file.
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
sets `fields` based on information in a data frame. See [Field
properties implementation](#field-properties-implementation) for
details.

### fieldsMatch

[`fieldsMatch`](https://datapackage.org/standard/table-schema/#fieldsMatch)
is **not supported** by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md).
It expects the default (`exact`).

### missingValues

[`missingValues`](https://datapackage.org/standard/table-schema/#missingValues)
is used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and defaults to `""`. It is passed to `na` in
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html).
If provided as labelled values, the `value` is extracted and `label`
ignored.
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
does not set `missingValues`.
[`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
converts `NA` values to `""` when writing a data frame to a CSV file.
Since this is the default, no `missingValues` property is set.

### primaryKey

[`primaryKey`](https://datapackage.org/standard/table-schema/#primaryKey)
is ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md).
`upgrade_package()` will convert single values to an array (for
[backwards
compatibility](https://datapackage.org/overview/changelog/#schemaprimarykey-updated)).

### uniqueKeys

[`uniqueKeys`](https://datapackage.org/standard/table-schema/#uniqueKeys)
is ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md).

### foreignKeys

[`foreignKeys`](https://datapackage.org/standard/table-schema/#foreignKeys)
is ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md).
`upgrade_package()` will convert single values in `fields` to an array
and remove `reference.resource` if it is self-referential (for
[backwards
compatibility](https://datapackage.org/overview/changelog/#schemaprimarykey-updated)).

## Field properties implementation

### name

[`name`](https://datapackage.org/standard/table-schema/#name) is used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
to assign a column name. The vector of names is passed as `col_names` to
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html),
ignoring names provided in the header of the data file.
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
uses the data frame column name to set `name`.

### type and format

[`type`](https://datapackage.org/standard/table-schema/#type-and-format)
and (for some types)
[`format`](https://datapackage.org/standard/table-schema/#type-and-format)
is used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
to understand the column type. The vector of types is passed as
`col_types` to
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html),
which warns if there are parsing issues (inspect with
[`problems()`](https://docs.ropensci.org/frictionless/reference/problems.md)).
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
uses the data frame column type to set `type`. See [Field types
implementation](#field-types-implementation) for details.

[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
interprets `type` as follows:

| field type | column type |
|----|----|
| [`string`](#string) | `character` or `factor` |
| [`number`](#number) | `double` or `factor` |
| [`integer`](#integer) | `double` or `factor` |
| [`boolean`](#boolean) | `logical` |
| [`object`](#object) | `character` |
| [`array`](#array) | `character` |
| [`list`](#list) | `character` |
| [`datetime`](#datetime) | `POSIXct` |
| [`date`](#date) | `Date` |
| [`time`](#time) | [`hms::hms()`](https://hms.tidyverse.org/reference/hms.html) |
| [`year`](#year) | `Date` |
| [`yearmonth`](#yearmonth) | `Date` |
| [`duration`](#duration) | `character` |
| [`geopoint`](#geopoint) | `character` |
| [`geojson`](#geojson) | `character` |
| [`any`](#any) | `character` |
| other value | error |
| undefined | `character` |

[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
sets `type` as follows:

| column type | field type |
|----|----|
| `character` | `string` |
| `Date` | `date` |
| `difftime` | `number` |
| `factor` | `string` with factor levels as `constraints$enum` |
| [`hms::hms()`](https://hms.tidyverse.org/reference/hms.html) | `time` |
| `integer` | `integer` |
| `logical` | `boolean` |
| `numeric` | `number` |
| `POSIXct`/`POSIXlt` | `datetime` |
| any other type | `any` |

[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
does not set a `format`, since defaults are used for all types. This is
also the case for datetimes, dates and times, since
[`readr::write_csv()`](https://readr.tidyverse.org/reference/write_delim.html)
used by
[`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
formats those to ISO8601, which is considered the default.

### title

[`title`](https://datapackage.org/standard/table-schema/#title) is
ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md).

### description

[`description`](https://datapackage.org/standard/table-schema/#description)
is ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md).

### example

[`example`](https://datapackage.org/standard/table-schema/#example) is
ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md).

### constraints

[`constraints`](https://datapackage.org/standard/table-schema/#field-constraints)
is ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md),
except for `constraints$enum`.
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
uses it set the column type to `factor`, with `enum` values as factor
levels.
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
does the reverse.

### categories

[`categories`](https://datapackage.org/standard/table-schema/#categories)
is ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md).

### categoriesOrdered

[`categoriesOrdered`](https://datapackage.org/standard/table-schema/#categoriesOrdered)
is ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md).

### missingValues

\[`missingValues](https://datapackage.org/standard/table-schema/#field-missingValues) at field-level is **not supported** by`read_resource()`and not set by`create_schema()`. Provide as`missingValues\`
at schema-level instead.

### rdfType

[`rdfType`](https://datapackage.org/standard/table-schema/#rdfType) is
ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md).

## Field types implementation

### string

[`string`](https://datapackage.org/standard/table-schema/#string) is
interpreted as `character`. Or `factor` when `constraints$enum` is
defined.

- `format` is ignored.

### number

[`number`](https://datapackage.org/standard/table-schema/#number) is
interpreted as `double`. Or `factor` when `constraints$enum` is defined.

- `bareNumber` is supported. If `false`, whitespace and non-numeric
  characters are ignored.
- `decimalChar` (`.` by default) is supported, but as a single value for
  all number fields. If different values are defined, the most occurring
  one is selected.
- `groupChar` (undefined by default) is supported, but as a single value
  for all number and integer fields. If different values are defined
  (across numbers and integers), the most occurring one is selected.

`decimalChar` and `groupChar` cannot have the same value, even if one is
defined for a number and the other for an integer.

### integer

[`integer`](https://datapackage.org/standard/table-schema/#integer) is
interpreted as `double` (to support big integers, + signs and
`bareNumber`). Or `factor` when `constraints$enum` is defined.

- `groupChar` (undefined by default) is supported, but as a single value
  for all number and integer fields. If different values are defined
  (across numbers and integers), the most occurring one is selected.
- `bareNumber` is supported. If `false`, whitespace and non-numeric
  characters are ignored.

### boolean

[`boolean`](https://datapackage.org/standard/table-schema/#boolean) is
interpreted as `logical`.

- `trueValues` is **not supported**, except for the default values
  (`"true"`, `"True"`, `"TRUE"`, `"1"`).
- `falseValues` is **not supported**, except for the default values
  (`"false"`, `"False"`, `"FALSE"`, `"0"`).

### object

[`object`](https://datapackage.org/standard/table-schema/#object) is
interpreted as `character`.

### array

[`array`](https://datapackage.org/standard/table-schema/#array) is
interpreted as `character`.

### list

[`list`](https://datapackage.org/standard/table-schema/#list) is
interpreted as `character`.

- `delimiter` is **not supported**.
- `itemType` is **not supported**.

### datetime

[`datetime`](https://datapackage.org/standard/table-schema/#datetime) is
interpreted as `POSIXct`.

- `format` is supported for the values `default` (ISO datetime, with
  optional milliseconds and timezone), `any` (ISO datetime) and the same
  patterns as for `date` and `time`. The value `%c` is **not
  supported**.

### date

[`date`](https://datapackage.org/standard/table-schema/#date) is
interpreted as `Date`.

- `format` is supported for the values `default` (ISO date), `any`
  (guess `ymd`) and [Python/C
  strptime](https://docs.python.org/2/library/datetime.html#strftime-strptime-behavior)
  patterns, such as `%a, %d %B %Y` for `Sat, 23 November 2013`. `%x` is
  interpreted as `%m/%d/%y`. The values `%j`, `%U`, `%w` and `%W` are
  **not supported**.

### time

[`time`](https://datapackage.org/standard/table-schema/#time) is
interpreted as
[`hms::hms()`](https://hms.tidyverse.org/reference/hms.html).

- `format` is supported for the values `default` (ISO time), `any`
  (guess `hms`) and [Python/C
  strptime](https://docs.python.org/2/library/datetime.html#strftime-strptime-behavior)
  patterns, such as `%I%p%M:%S.%f%z` for `8AM30:00.300+0200`.

### year

[`year`](https://datapackage.org/standard/table-schema/#year) is
interpreted as `Date` with month and day set to `01`.

### yearmonth

[`yearmonth`](https://datapackage.org/standard/table-schema/#yearmonth)
is interpreted as `Date` with day set to `01`.

### duration

[`duration`](https://datapackage.org/standard/table-schema/#duration) is
interpreted as `character`. You can parse these values with
[`lubridate::duration()`](https://lubridate.tidyverse.org/reference/duration.html).

### geopoint

[`geopoint`](https://datapackage.org/standard/table-schema/#geopoint) is
interpreted as `character`.

### geojson

[`geojson`](https://datapackage.org/standard/table-schema/#geojson) is
interpreted as `character`.

### any

[`any`](https://datapackage.org/standard/table-schema/#any) is
interpreted as `character`
