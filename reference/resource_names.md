# List Data Resource names

Lists the names of the Data Resources included in a Data Package.

## Usage

``` r
resource_names(package)
```

## Arguments

- package:

  Data Package object, as returned by
  [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  or
  [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md).

## Value

Character vector with the Data Resource names.

## See also

Other accessor functions:
[`resource()`](https://docs.ropensci.org/frictionless/reference/resource.md),
[`schema()`](https://docs.ropensci.org/frictionless/reference/schema.md)

## Examples

``` r
# Load the example Data Package
package <- example_package()

# List the resources
resource_names(package)
#> [1] "deployments"  "observations" "media"       
```
