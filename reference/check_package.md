# Check a Data Package object

Check if an object is a Data Package object with the required
properties.

## Usage

``` r
check_package(package)
```

## Arguments

- package:

  Data Package object, as returned by
  [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  or
  [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md).

## Value

`package` invisibly or an error.

## Examples

``` r
# Load the example Data Package
package <- example_package()

# Check if the Data Package is valid (invisible return)
check_package(package)
```
