# Get the specification version number

Determines what version of the Data Package, Data Resource, Table
Dialect or Table Schema standard is used, based on the
[`$schema`](https://datapackage.org/standard/data-package/#dollar-schema)
property.

- `"1.0"`: [v1](https://specs.frictionlessdata.io/) specification.
  Assumed if `$schema` is missing.

- `"2.0`: [v2](https://datapackage.org/) specification.

- `">=2.0"`: assumed if `$schema` is defined by deviating from the
  default (e.g. an
  [extension](https://datapackage.org/standard/extensions/)).

## Usage

``` r
version(x)
```

## Arguments

- x:

  A list describing either a Data Package, Data Resource, Table Dialect
  or Table Schema.

## Value

Data Package standard version number.

## Examples

``` r
# Data Package
package <- example_package()
version(package)
#> [1] "2.0"

# Data Resource
resource <- resource(package, "observations")
version(resource)
#> [1] "2.0"

# Table Dialect
version(resource$dialect)
#> [1] "2.0"

# Table Schema
schema <- schema(package, "observations")
version(schema)
#> [1] "2.0"
```
