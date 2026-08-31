# Create a Table Schema from a data frame

Creates a Table Schema for a data frame, listing all column names and
types as field names and (converted) types.

## Usage

``` r
create_schema(data)
```

## Arguments

- data:

  A data frame.

## Value

List describing a Table Schema.

## Details

See
[`vignette("table-schema")`](https://docs.ropensci.org/frictionless/articles/table-schema.md)
to learn how this function implements the Data Package standard.

## See also

Other create functions:
[`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md)

## Examples

``` r
# Create a data frame
df <- data.frame(
  id = c(as.integer(1), as.integer(2)),
  timestamp = c(
    as.POSIXct("2020-03-01 12:00:00", tz = "EET"),
    as.POSIXct("2020-03-01 18:45:00", tz = "EET")
  ),
  life_stage = factor(c("adult", "adult"), levels = c("adult", "juvenile"))
)

# Create a Table Schema from the data frame
schema <- create_schema(df)
str(schema)
#> List of 2
#>  $ $schema: chr "https://datapackage.org/profiles/2.0/tableschema.json"
#>  $ fields :List of 3
#>   ..$ :List of 2
#>   .. ..$ name: chr "id"
#>   .. ..$ type: chr "integer"
#>   ..$ :List of 2
#>   .. ..$ name: chr "timestamp"
#>   .. ..$ type: chr "datetime"
#>   ..$ :List of 3
#>   .. ..$ name       : chr "life_stage"
#>   .. ..$ type       : chr "string"
#>   .. ..$ constraints:List of 1
#>   .. .. ..$ enum:List of 2
#>   .. .. .. ..$ : chr "adult"
#>   .. .. .. ..$ : chr "juvenile"
```
