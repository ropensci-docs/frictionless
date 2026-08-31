# Get the Table Schema of a Data Resource

Gets the Table Schema of a Data Resource (in a Data Package), i.e. the
content of its `schema` property, describing the resource's fields, data
types, relationships, and missing values. The resource must be a
[Tabular Data
Resource](https://datapackage.org/standard/data-resource/#tabular).

## Usage

``` r
schema(package, resource_name)
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

List describing a Table Schema.

## Details

See
[`vignette("table-schema")`](https://docs.ropensci.org/frictionless/articles/table-schema.md)
to learn more about Table Schema.

## See also

Other accessor functions:
[`resource()`](https://docs.ropensci.org/frictionless/reference/resource.md),
[`resource_names()`](https://docs.ropensci.org/frictionless/reference/resource_names.md)

## Examples

``` r
# Load the example Data Package
package <- example_package()

# Get the Table Schema for the resource "observations"
schema <- schema(package, "observations")
str(schema)
#> List of 5
#>  $ fields       :List of 7
#>   ..$ :List of 3
#>   .. ..$ name       : chr "observation_id"
#>   .. ..$ type       : chr "string"
#>   .. ..$ constraints:List of 2
#>   .. .. ..$ required: logi TRUE
#>   .. .. ..$ unique  : logi TRUE
#>   ..$ :List of 3
#>   .. ..$ name       : chr "deployment_id"
#>   .. ..$ type       : chr "string"
#>   .. ..$ constraints:List of 1
#>   .. .. ..$ required: logi TRUE
#>   ..$ :List of 4
#>   .. ..$ name       : chr "timestamp"
#>   .. ..$ type       : chr "datetime"
#>   .. ..$ format     : chr "%Y-%m-%dT%H:%M:%S%z"
#>   .. ..$ constraints:List of 1
#>   .. .. ..$ required: logi TRUE
#>   ..$ :List of 3
#>   .. ..$ name       : chr "scientific_name"
#>   .. ..$ type       : chr "string"
#>   .. ..$ constraints:List of 1
#>   .. .. ..$ required: logi FALSE
#>   ..$ :List of 3
#>   .. ..$ name       : chr "count"
#>   .. ..$ type       : chr "integer"
#>   .. ..$ constraints:List of 2
#>   .. .. ..$ required: logi FALSE
#>   .. .. ..$ minimum : int 1
#>   ..$ :List of 5
#>   .. ..$ name             : chr "life_stage"
#>   .. ..$ type             : chr "string"
#>   .. ..$ categories       :List of 5
#>   .. .. ..$ : chr "adult"
#>   .. .. ..$ : chr "subadult"
#>   .. .. ..$ : chr "juvenile"
#>   .. .. ..$ : chr "offspring"
#>   .. .. ..$ : chr "unknown"
#>   .. ..$ categoriesOrdered: logi FALSE
#>   .. ..$ constraints      :List of 2
#>   .. .. ..$ required: logi FALSE
#>   .. .. ..$ enum    :List of 5
#>   .. .. .. ..$ : chr "adult"
#>   .. .. .. ..$ : chr "subadult"
#>   .. .. .. ..$ : chr "juvenile"
#>   .. .. .. ..$ : chr "offspring"
#>   .. .. .. ..$ : chr "unknown"
#>   ..$ :List of 3
#>   .. ..$ name       : chr "comments"
#>   .. ..$ type       : chr "string"
#>   .. ..$ constraints:List of 1
#>   .. .. ..$ required: logi FALSE
#>  $ $schema      : chr "https://datapackage.org/profiles/2.0/tableschema.json"
#>  $ missingValues:List of 1
#>   ..$ : chr "NA"
#>  $ primaryKey   :List of 1
#>   ..$ : chr "observation_id"
#>  $ foreignKeys  :List of 1
#>   ..$ :List of 2
#>   .. ..$ fields   :List of 1
#>   .. .. ..$ : chr "deployment_id"
#>   .. ..$ reference:List of 2
#>   .. .. ..$ resource: chr "deployments"
#>   .. .. ..$ fields  :List of 1
#>   .. .. .. ..$ : chr "deployment_id"
```
