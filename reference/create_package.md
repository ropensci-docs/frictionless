# Create a Data Package

Initiates a Data Package object, either from scratch or from an existing
list. This Data Package object is a list with the following
characteristics:

- All properties of the original `descriptor`.

- A `resources` property, set to an empty list if undefined.

- A `directory` attribute, set to `"."` for the current directory if
  undefined. It is used as the base path to access resources with
  [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md).

- A `datapackage` subclass.

## Usage

``` r
create_package(descriptor = NULL)
```

## Arguments

- descriptor:

  List to be made into a Data Package object. If undefined, an empty
  Data Package will be created from scratch.

## Value

A Data Package object.

## Details

See
[`vignette("data-package")`](https://docs.ropensci.org/frictionless/articles/data-package.md)
to learn how this function implements the Data Package standard.
[`check_package()`](https://docs.ropensci.org/frictionless/reference/check_package.md)
is automatically called on the created package to make sure it is valid.

## See also

Other create functions:
[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)

## Examples

``` r
# Create a Data Package
package <- create_package()

package
#> A Data Package (version 2.0) with 0 resources.
#> Use `unclass()` to print the Data Package as a list.

# See the structure of the (empty) Data Package
str(package)
#> List of 2
#>  $ $schema  : chr "https://datapackage.org/profiles/2.0/datapackage.json"
#>  $ resources: list()
#>  - attr(*, "directory")= chr "."
#>  - attr(*, "class")= chr [1:2] "datapackage" "list"
```
