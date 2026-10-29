# Read data from a Data Resource into a tibble data frame

Reads data from a Data Resource (in a Data Package) into a tibble (a
Tidyverse data frame). The resource must be a [Tabular Data
Resource](https://datapackage.org/standard/data-resource/#tabular). The
function uses
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html)
to read CSV files, passing the resource properties `path`, CSV dialect,
column names, data types, etc. Column names are taken from the provided
Table Schema (`schema`), not from the header in the CSV file(s).

## Usage

``` r
read_resource(package, resource_name, col_select = NULL)
```

## Arguments

- package:

  Data Package object, as returned by
  [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  or
  [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md).

- resource_name:

  Name of the Data Resource.

- col_select:

  Character vector of the columns to include in the result, in the order
  provided. Selecting columns can improve read speed.

## Value

A
[`tibble::tibble()`](https://tibble.tidyverse.org/reference/tibble.html)
with the Data Resource's tabular data. If there are parsing problems, a
warning will alert you. You can retrieve the full details by calling
[`problems()`](https://docs.ropensci.org/frictionless/reference/problems.md)
on your data frame.

## Details

See
[`vignette("data-resource")`](https://docs.ropensci.org/frictionless/articles/data-resource.md),
[`vignette("table-dialect")`](https://docs.ropensci.org/frictionless/articles/table-dialect.md)
and
[`vignette("table-schema")`](https://docs.ropensci.org/frictionless/articles/table-schema.md)
to learn how this function implements the Data Package standard.

## See also

Other read functions:
[`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)

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

# Read data from the resource "observations"
read_resource(package, "observations")
#> # A tibble: 8 × 7
#>   observation_id deployment_id timestamp           scientific_name     count
#>   <chr>          <chr>         <dttm>              <chr>               <dbl>
#> 1 1-1            1             2020-09-28 00:13:07 Capreolus capreolus     1
#> 2 1-2            1             2020-09-28 15:59:17 Capreolus capreolus     1
#> 3 1-3            1             2020-09-28 16:35:23 Lepus europaeus         1
#> 4 1-4            1             2020-09-28 17:04:04 Lepus europaeus         1
#> 5 1-5            1             2020-09-28 19:19:54 Sus scrofa              2
#> 6 2-1            2             2021-10-01 01:25:06 Sus scrofa              1
#> 7 2-2            2             2021-10-01 01:25:06 Sus scrofa              1
#> 8 2-3            2             2021-10-01 04:47:30 Sus scrofa              1
#> # ℹ 2 more variables: life_stage <fct>, comments <chr>

# The above tibble is merged from 2 files listed in the resource path
package$resources[[2]]$path
#> [[1]]
#> [1] "observations_1.tsv"
#> 
#> [[2]]
#> [1] "observations_2.tsv"
#> 

# The column names and types are derived from the resource schema
purrr::map_chr(package$resources[[2]]$schema$fields, "name")
#> [1] "observation_id"  "deployment_id"   "timestamp"       "scientific_name"
#> [5] "count"           "life_stage"      "comments"       
purrr::map_chr(package$resources[[2]]$schema$fields, "type")
#> [1] "string"   "string"   "datetime" "string"   "integer"  "string"   "string"  

# Read data from the resource "deployments" with column selection
read_resource(package, "deployments", col_select = c("latitude", "longitude"))
#> # A tibble: 3 × 2
#>   latitude longitude
#>   <chr>        <dbl>
#> 1 50.76698      4.62
#> 2 50.82716      4.64
#> 3 50.81860      4.65
```
