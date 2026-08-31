# Read a Data Package descriptor file (`datapackage.json`)

Reads information from a `datapackage.json` file, i.e. the
[descriptor](https://datapackage.org/standard/data-package/#descriptor)
file that describes the Data Package metadata and its Data Resources.

## Usage

``` r
read_package(file = "datapackage.json")
```

## Arguments

- file:

  Path or URL to a `datapackage.json` file.

## Value

A Data Package object, see
[`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md).

## Details

See
[`vignette("data-package")`](https://docs.ropensci.org/frictionless/articles/data-package.md)
to learn how this function implements the Data Package standard.

## See also

Other read functions:
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)

## Examples

``` r
# Read a datapackage.json file
package <- read_package(
  system.file("extdata", "v1", "datapackage.json", package = "frictionless")
)

package
#> A Data Package (version 1.0) with 3 resources:
#> • deployments
#> • observations
#> • media
#> Use `unclass()` to print the Data Package as a list.

# Access the Data Package properties
package$name
#> [1] "example_package"
package$created
#> [1] "2021-03-02T17:22:33Z"
```
