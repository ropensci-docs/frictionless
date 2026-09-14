# Add a Data Resource

Adds a Data Resource to a Data Package. The resource will be a [Tabular
Data Resource](https://datapackage.org/standard/data-resource/#tabular).

## Usage

``` r
add_resource(
  package,
  resource_name,
  data,
  schema = NULL,
  replace = FALSE,
  delim = ",",
  ...
)
```

## Arguments

- package:

  Data Package object, as returned by
  [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  or
  [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md).

- resource_name:

  Name of the Data Resource.

- data:

  Data to attach, either a data frame or path(s) to CSV file(s):

  - Data frame: attached to the resource as `data` and written to a CSV
    file when using
    [`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md).

  - One or more paths or URLs to CSV files as a character (vector):
    added to the resource as `path`. The last file will be read with
    [`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html)
    to create or compare with `schema` and to set `format`, `mediatype`
    and `encoding`. The other files are ignored, but are expected to
    have the same structure and properties.

- schema:

  Either a list, or path or URL to a JSON file describing a Table Schema
  for the `data`. If not provided, one will be created using
  [`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md).

- replace:

  If `TRUE`, allows an existing resource of the same name to be
  replaced.

- delim:

  Delimiter for the CSV file(s) referenced in `data` (e.g. `\t` for a
  tab-separated file). Will be set as `delimiter` in the resource Table
  Dialect, so read functions know how to read the file(s). Ignored if
  `data` is a data frame.

- ...:

  Additional [metadata
  properties](https://docs.ropensci.org/frictionless/articles/data-resource.html#properties-implementation)
  to add to the resource, e.g. `title = "My title", validated = FALSE`.
  These are not verified against specifications and are ignored by
  [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md).
  The following properties are automatically set and can't be provided
  with `...`: `$schema`, `name`, `path`, `data`, `type`, `format`,
  `mediatype`, `encoding`, `dialect` and `schema`.

## Value

`package` with one additional resource.

## Details

See
[`vignette("data-resource")`](https://docs.ropensci.org/frictionless/articles/data-resource.md)
(and to a lesser extend
[`vignette("table-dialect")`](https://docs.ropensci.org/frictionless/articles/table-dialect.md))
to learn how this function implements the Data Package standard.

## See also

Other edit functions:
[`remove_resource()`](https://docs.ropensci.org/frictionless/reference/remove_resource.md)

## Examples

``` r
# Load the example Data Package
package <- example_package()

# List the resources
resource_names(package)
#> [1] "deployments"  "observations" "media"       

# Create a data frame
df <- data.frame(
  multimedia_id = c(
    "aed5fa71-3ed4-4284-a6ba-3550d1a4de8d",
    "da81a501-8236-4cbd-aa95-4bc4b10a05df"
  ),
  x = c(718, 748),
  y = c(860, 900)
)

# Add the resource "positions" from the data frame
package <- add_resource(package, "positions", data = df)

# Add the resource "positions_with_schema", with a user-defined schema and title
my_schema <- create_schema(df)
package <- add_resource(
  package,
  resource_name = "positions_with_schema",
  data = df,
  schema = my_schema,
  title = "Positions with schema"
)

# Replace the resource "observations" with a file-based resource (2 TSV files)
path_1 <-
  system.file("extdata", "v1", "observations_1.tsv", package = "frictionless")
path_2 <-
  system.file("extdata", "v1", "observations_2.tsv", package = "frictionless")
package <- add_resource(
  package,
  resource_name = "observations",
  data = c(path_1, path_2),
  replace = TRUE,
  delim = "\t"
)

# List the resources ("positions" and "positions_with_schema" added)
resource_names(package)
#> [1] "deployments"           "observations"          "media"                
#> [4] "positions"             "positions_with_schema"
```
