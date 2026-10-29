# Add properties

Adds or inserts provided arguments as elements to a list. Attributes and
classes of the original list are retained.

## Usage

``` r
add_properties(x, ..., after = length(x))
```

## Arguments

- x:

  A list or vector.

- ...:

  Elements to add, as named arguments.

- after:

  Position after which elements should be added. Use `0` to add elements
  at the beginning.

## Value

`x` with the added elements.

## Details

`add_properties()` can be used to add (custom) metadata properties to a
Data Package, Data Resource, Table Dialect or Table Schema. Note that
added properties are not validated. See the vignettes for an overview of
all standard metadata properties.

`add_properties()` has some advantages over:

- [`append()`](https://rdrr.io/r/base/append.html): removes attributes
  and classes, invalidating a Data Package object.

- Direct assignment (`package$title <- "My package"`): no control over
  the position of the added element.

[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
also supports adding metadata properties to a Data Resource, except
those automatically set by that function.

## See also

Other edit functions:
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md),
[`remove_resource()`](https://docs.ropensci.org/frictionless/reference/remove_resource.md)

## Examples

``` r
# Add property to a generic list
list <- list(a = 1, b = 2)
add_properties(list, added = 3)
#> $a
#> [1] 1
#> 
#> $b
#> [1] 2
#> 
#> $added
#> [1] 3
#> 

# Add properties to a Data Package
package <- create_package()
package <- add_properties(
  package,
  title = "Example package",
  keywords = c("camera traps", "frictionlessdata"),
  after = 1 # Add after the first property
)
package
#> A Data Package (version 2.0) with 0 resources.
#> Use `unclass()` to print the Data Package as a list.

str(package)
#> List of 4
#>  $ $schema  : chr "https://datapackage.org/profiles/2.0/datapackage.json"
#>  $ title    : chr "Example package"
#>  $ keywords : chr [1:2] "camera traps" "frictionlessdata"
#>  $ resources: list()
#>  - attr(*, "directory")= chr "."
#>  - attr(*, "class")= chr [1:2] "datapackage" "list"
```
