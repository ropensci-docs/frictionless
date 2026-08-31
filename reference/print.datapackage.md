# Print a Data Package

Prints a human-readable summary of a Data Package, including its
resources and a link to more information (if provided in `package$id`).

## Usage

``` r
# S3 method for class 'datapackage'
print(x, ...)
```

## Arguments

- x:

  Data Package object, as returned by
  [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  or
  [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md).

- ...:

  Further arguments, they are ignored by this function.

## Value

[`print()`](https://rdrr.io/r/base/print.html) with a summary of the
Data Package object.

## Examples

``` r
# Load the example Data Package
package <- example_package()

# Print a summary of the Data Package
package # Or print(package)
#> A Data Package (version 2.0) with 3 resources:
#> • deployments
#> • observations
#> • media
#> Use `unclass()` to print the Data Package as a list.
```
