# Remove a Data Resource

Removes a Data Resource from a Data Package, i.e. it removes one of the
described `resources`.

## Usage

``` r
remove_resource(package, resource_name)
```

## Arguments

- package:

  Data Package object, as returned by
  [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  or
  [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md).

- resource_name:

  Name of the Data Resource.

## Value

`package` with one fewer resource.

## See also

Other edit functions:
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)

## Examples

``` r
# Load the example Data Package
package <- example_package()

# List the resources
resource_names(package)
#> [1] "deployments"  "observations" "media"       

# Remove the resource "observations"
package <- remove_resource(package, "observations")

# List the resources ("observations" removed)
resource_names(package)
#> [1] "deployments" "media"      
```
