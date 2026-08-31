# Table Dialect

[Table Dialect](https://datapackage.org/standard/table-dialect/) is a
simple format to describe the dialect of a tabular data file, including
its delimiter, header rows, escape characters, etc.

In this document we use the terms “package” for Data Package, “resource”
for Data Resource, “dialect” for Table Dialect, and “schema” for Table
Schema.

## General implementation

frictionless supports most dialect properties to read [Tabular Data
Resources](https://datapackage.org/standard/data-resource/#tabular), but
only those designed for [delimited
formats](https://datapackage.org/standard/table-dialect/#delimited) (not
structured, spreadsheet, database formats). Dialect manipulation is
limited to setting a `delimiter`. When writing resources, it (mainly)
makes use of default dialect properties, removing the necessity to
define them.

### Read

[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
uses the `dialect` property of a resource to parse a tabular data file.
Only properties that deviate from the default need to be specified. E.g.
a tab-delimited file without header rows must have the following
dialect:

``` json
"dialect": {
  "delimiter": "\t",
  "header": false
}
```

### Manipulate

frictionless does not support direct manipulation of the dialect.
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
allows to set one property (`delimiter`) when data are provided as a
file, all other properties are assumed to be the default.

### Write

[`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
writes a package to disk as a `datapackage.json` file. This file
includes the metadata of all the resources, including the dialect (if
defined).
[`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
writes resources created from a data frame to CSV files, but no
`dialect` property is set for those, since only defaults are used.

## Properties implementation

### \$schema

[`$schema`](https://datapackage.org/standard/table-dialect/#dollar-schema)
indicates what
[`version()`](https://docs.ropensci.org/frictionless/reference/version.md)
of the Table Dialect standard is used (v1 if undefined).

- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  ignores it and does not upgrade a dialect, since it does not rely on
  v1 properties deprecated in v2.
- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  does *not* set `$schema` to the recommended v2 value
  (`"https://datapackage.org/profiles/2.0/tabledialect.json"`), because
  it typically does not define a dialect and therefore `$schema`. It
  thus creates (default to) a v1 dialect.
- `upgrade_package()` leaves dialect as is for all resources.

### header

[`header`](https://datapackage.org/standard/table-dialect/#header) is
used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and defaults to `true`. It is passed as `skip = 1` (or `0` for `false`)
in
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html),
ignoring the header. Field names in `schema` are used instead.

### headerRows

[`headerRows`](https://datapackage.org/standard/table-dialect/#headerRows)
is **not supported** by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md).

### headerJoin

[`headerJoin`](https://datapackage.org/standard/table-dialect/#headerJoin)
is **not supported** by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md).

### commentRows

[`commentRows`](https://datapackage.org/standard/table-dialect/#commentRows)
is **not supported** by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md).

### commentChar

[`commentChar`](https://datapackage.org/standard/table-dialect/#commentChar)
is used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and defaults to undefined. It is passed to `comment` in
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html).

### delimiter

[`delimiter`](https://datapackage.org/standard/table-dialect/#delimiter)
is used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and defaults to `","`. It is passed to `delim` in
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html).
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
does not set `delimiter`, unless provided in `delim` and different from
the default `","`:

``` r

library(frictionless)
package <- example_package()

path <- system.file("extdata", "v2", "observations_1.tsv", package = "frictionless")
package <- add_resource(package, "observations", data = path, delim = "\t", replace = TRUE)
resource(package, "observations")$dialect$delimiter
#> [1] "\t"
```

### lineTerminator

[`lineTerminator`](https://datapackage.org/standard/table-dialect/#lineTerminator)
is **not supported** by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md).
It relies on
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html)
instead, which interprets line terminator `LF` and `CRLF` automatically
and does not support `CR` (used by Classic Mac OS, final release 2001).

### quoteChar

[`quoteChar`](https://datapackage.org/standard/table-dialect/#quoteChar)
is used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and defaults to `"`. It is passed to `quote` in
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html).

### doubleQuote

[`doubleQuote`](https://datapackage.org/standard/table-dialect/#doubleQuote)
is used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and defaults to `true`, but can be overruled by `escapeChar`. It is
passed to `escape_double` in
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html).

### escapeChar

[`escapeChar`](https://datapackage.org/standard/table-dialect/#escapeChar)
is **not supported** by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
unless it is `"\\"`. It is passed as `escape_backslash = TRUE` and
`escape_double = FALSE` in
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html).

`escapeChar` and `doubleQuote` are mutually exclusive, so you cannot
escape with `\"` and `""` in the same file.

### nullSequence

[`nullSequence`](https://datapackage.org/standard/table-dialect/#nullSequence)
is **not supported** by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md).
Provide as `missingValues` in the schema instead (see
[`vignette("table-schema")`](https://docs.ropensci.org/frictionless/articles/table-schema.md)).

### skipInitialSpace

[`skipInitialSpace`](https://datapackage.org/standard/table-dialect/#skipInitialSpace)
is used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and defaults to `false`. It is passed to `trim_ws` in
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html).
