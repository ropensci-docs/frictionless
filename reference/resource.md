# Get or overwrite a Data Resource

Gets or overwrites a Data Resource from/in a Data Package. These
functions are **designed for internal use in other packages**. For
public manipulation of resources, use
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
and
[`remove_resource()`](https://docs.ropensci.org/frictionless/reference/remove_resource.md).

`resource()` gets a Data Resource from a Data Package by its name. The
returned value will be a list describing a Data Resource, with a new
attribute `data_location` to indicate how the data are attached. If
present, `path` will be updated to the full path(s).

`resource<-` overwrites a Data Resource in Data Package with a new
value. The assigned value will typically be a resource obtained with
`resource()` and then manipulated. The assignment function will
therefore revert internal changes made by `resource()` (i.e. removing
the attribute `data_location` and updating paths to the original
values). The assigned value is otherwise **not validated**, so use with
care.

## Usage

``` r
resource(package, resource_name)

resource(package, resource_name) <- value
```

## Arguments

- package:

  Data Package object, as returned by
  [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  or
  [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md).

- resource_name:

  Name of the Data Resource.

- value:

  Value to assign.

## Value

List describing a Data Resource.

`package` with overwritten resource.

## Details

See
[`vignette("data-resource")`](https://docs.ropensci.org/frictionless/articles/data-resource.md)
to learn more about Data Resource.

## See also

Other accessor functions:
[`resource_names()`](https://docs.ropensci.org/frictionless/reference/resource_names.md),
[`schema()`](https://docs.ropensci.org/frictionless/reference/schema.md)

## Examples

``` r
# Load the example Data Package
package <- example_package()

# Get the resource "deployments"
resource <- resource(package, "deployments")
str(resource)
#> List of 9
#>  $ $schema  : chr "https://datapackage.org/profiles/2.0/dataresource.json"
#>  $ name     : chr "deployments"
#>  $ path     : chr "/github/home/R/x86_64-pc-linux-gnu-library/4.6/frictionless/extdata/v2/deployments.csv"
#>  $ type     : chr "table"
#>  $ title    : chr "Camera trap deployments"
#>  $ format   : chr "csv"
#>  $ mediatype: chr "text/csv"
#>  $ encoding : chr "utf-8"
#>  $ schema   :List of 4
#>   ..$ fields       :List of 5
#>   .. ..$ :List of 3
#>   .. .. ..$ name       : chr "deployment_id"
#>   .. .. ..$ type       : chr "string"
#>   .. .. ..$ constraints:List of 2
#>   .. .. .. ..$ required: logi TRUE
#>   .. .. .. ..$ unique  : logi TRUE
#>   .. ..$ :List of 3
#>   .. .. ..$ name       : chr "longitude"
#>   .. .. ..$ type       : chr "number"
#>   .. .. ..$ constraints:List of 3
#>   .. .. .. ..$ required: logi TRUE
#>   .. .. .. ..$ minimum : int -180
#>   .. .. .. ..$ maximum : int 180
#>   .. ..$ :List of 2
#>   .. .. ..$ name       : chr "latitude"
#>   .. .. ..$ constraints:List of 1
#>   .. .. .. ..$ required: logi TRUE
#>   .. ..$ :List of 4
#>   .. .. ..$ name       : chr "start"
#>   .. .. ..$ type       : chr "date"
#>   .. .. ..$ format     : chr "%x"
#>   .. .. ..$ constraints:List of 1
#>   .. .. .. ..$ required: logi TRUE
#>   .. ..$ :List of 3
#>   .. .. ..$ name       : chr "comments"
#>   .. .. ..$ type       : chr "string"
#>   .. .. ..$ constraints:List of 1
#>   .. .. .. ..$ required: logi FALSE
#>   ..$ $schema      : chr "https://datapackage.org/profiles/2.0/tableschema.json"
#>   ..$ missingValues:List of 3
#>   .. ..$ :List of 2
#>   .. .. ..$ value: chr ""
#>   .. .. ..$ label: chr "missing"
#>   .. ..$ :List of 2
#>   .. .. ..$ value: chr "NA"
#>   .. .. ..$ label: chr "not applicable"
#>   .. ..$ :List of 2
#>   .. .. ..$ value: chr "NaN"
#>   .. .. ..$ label: chr "not a number"
#>   ..$ primaryKey   :List of 1
#>   .. ..$ : chr "deployment_id"
#>  - attr(*, "data_location")= chr "path"

# Update the resource
resource$description <- "Table with deployments."

# Overwrite the resource
resource(package, "deployments") <- resource

# Updating a resource property can also be done in one step
resource(package, "deployments")$description <- "Table with deployments."
```
