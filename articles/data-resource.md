# Data Resource

[Data Resource](https://datapackage.org/standard/data-resource/) is a
simple format to describe a data resource such as an individual table or
file, including its name, format, path, etc.

In this document we use the terms “package” for Data Package, “resource”
for Data Resource, “dialect” for Table Dialect, and “schema” for Table
Schema.

## General implementation

frictionless supports getting, reading, manipulating and writing
resources, but much of its functionality is limited to [Tabular Data
Resources](https://datapackage.org/standard/data-resource/#tabular).

### Access

[`resource_names()`](https://docs.ropensci.org/frictionless/reference/resource_names.md)
lists all resources in a package:

``` r

library(frictionless)
package <- example_package()

# List the resources
resource_names(package)
#> [1] "deployments"  "observations" "media"
```

[`resource()`](https://docs.ropensci.org/frictionless/reference/resource.md)
gets a resource from a package by its name:

``` r

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
```

`resource()<-` overwrites a resource in a package:

``` r

resource(package, "deployments") <- resource
```

Note that
[`resource()`](https://docs.ropensci.org/frictionless/reference/resource.md)
and `resource()<-` are designed for internal use in other packages. For
public manipulation of resources, use
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
and
[`remove_resource()`](https://docs.ropensci.org/frictionless/reference/remove_resource.md)
(see [Manipulate](#manipulate)).

### Read

[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
reads data from a tabular resource to a data frame:

``` r

read_resource(package, "deployments")
#> # A tibble: 3 × 5
#>   deployment_id longitude latitude start      comments                     
#>   <chr>             <dbl> <chr>    <date>     <chr>                        
#> 1 1                  4.62 50.76698 2020-09-25  NA                          
#> 2 2                  4.64 50.82716 2020-10-01 "On \"forêt\" road."         
#> 3 3                  4.65 50.81860 2020-10-05 "Malfunction/no photos, data"
```

frictionless does not support reading data from non-tabular resources.

### Manipulate

[`remove_resource()`](https://docs.ropensci.org/frictionless/reference/remove_resource.md)
removes a resource (of any type):

``` r

remove_resource(package, "deployments")
#> A Data Package (version 2.0) with 2 resources:
#> • observations
#> • media
#> Use `unclass()` to print the Data Package as a list.

# This and many other functions return "package", which you can update with
# package <- remove_resource(package, "deployments")
```

[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
adds or replaces a tabular resource. **It always creates a resource
following the [v2
specification](https://datapackage.org/standard/data-resource/).** The
provided data must be a data frame or a tabular data file (e.g. CSV):

``` r

# Add a resource with data from a data frame
add_resource(package, "iris", data = iris)
#> A Data Package (version 2.0) with 4 resources:
#> • deployments
#> • observations
#> • media
#> • iris
#> Use `unclass()` to print the Data Package as a list.

# Replace a resource with one where data is stored in a tabular file
path <- system.file("extdata", "v2", "deployments.csv", package = "frictionless")
add_resource(package, "deployments", data = path, replace = TRUE)
#> A Data Package (version 2.0) with 3 resources:
#> • deployments
#> • observations
#> • media
#> Use `unclass()` to print the Data Package as a list.
```

You can pipe most functions (see
[`vignette("data-package")`](https://docs.ropensci.org/frictionless/articles/data-package.md)).

### Write

[`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
writes a package and its related resources to disk as a
`datapackage.json` and CSV files. See the function documentation for
details.

## Properties implementation

### \$schema

[`$schema`](https://datapackage.org/standard/data-resource/#dollar-schema)
indicates what
[`version()`](https://docs.ropensci.org/frictionless/reference/version.md)
of the Data Resource standard is used (v1 if undefined).

- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  ignores it and does not upgrade a resource, since it does not rely on
  v1 properties deprecated in v2.
- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  sets `$schema` to the recommended v2 value
  (`"https://datapackage.org/profiles/2.0/dataresource.json"`) and thus
  creates a v2 resource.
- `upgrade_package()` set `$schema` to the recommended v2 value for all
  resources of a package.

### profile

[`profile`](https://specs.frictionlessdata.io/data-resource/#profile) (a
deprecated v1 property) is ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md).
`upgrade_package()` removes `profile` (unless `$schema` is already set),
but converts `"profile" = "tabular-data-resource"` to `"type" = "table"`
(for [backwards
compatibility](https://datapackage.org/standard/data-resource/#type)).

### name

[`name`](https://datapackage.org/standard/data-resource/#name) is
required. It is used to identify a resource in
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md),
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
and
[`remove_resource()`](https://docs.ropensci.org/frictionless/reference/remove_resource.md)
(always as the second argument):

``` r

deployments <- read_resource(package, resource_name = "deployments")
```

[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
sets `name` to the provided `resource_name`:

``` r

add_resource(package, resource_name = "iris", data = iris)
#> A Data Package (version 2.0) with 4 resources:
#> • deployments
#> • observations
#> • media
#> • iris
#> Use `unclass()` to print the Data Package as a list.
```

### path

[`path`](https://datapackage.org/standard/data-resource/#path-or-data)
or `data` (see further) is required. Providing both is not allowed.

`path` is for data in files (e.g. a CSV file). It can be a local path or
URL. Supported protocols are `http`, `https`, `ftp`, `sftp` and `sftp`.
Absolute paths (`/`), relative parent paths (`../`) and paths containing
hidden directories (starting with `.`) are not allowed to avoid security
vulnerabilities.

When multiple paths are provided
(`"path": ["myfile1.csv", "myfile2.csv"]`), the files are expected to
have the same structure.
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
merges these into a single data frame in the order the paths are
provided (using
[`dplyr::bind_rows()`](https://dplyr.tidyverse.org/reference/bind_rows.html)):

``` r

# The "observations" resource has multiple files in path
resource(package, "observations")$path
#> [1] "/github/home/R/x86_64-pc-linux-gnu-library/4.6/frictionless/extdata/v2/observations_1.tsv"
#> [2] "/github/home/R/x86_64-pc-linux-gnu-library/4.6/frictionless/extdata/v2/observations_2.tsv"
# These are combined into a single data frame when reading
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
```

[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
sets `path` to the path(s) provided in `data`:

``` r

path <- system.file("extdata", "v2", "deployments.csv", package = "frictionless")
add_resource(package, "deployments", data = path, replace = TRUE)
#> A Data Package (version 2.0) with 3 resources:
#> • deployments
#> • observations
#> • media
#> Use `unclass()` to print the Data Package as a list.
```

### data

Support for inline `data` is currently limited, e.g. JSON object and
string are **not supported** and `schema`, `mediatype` and `format` are
ignored.

`data` is for inline data (included in the `datapackage.json`).
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
attempts to read `data` if it is provided as a JSON array:

``` r

# The "media" resource has inline data
str(resource(package, "media")$data)
#> List of 3
#>  $ :List of 5
#>   ..$ media_id      : chr "aed5fa71-3ed4-4284-a6ba-3550d1a4de8d"
#>   ..$ deployment_id : chr "1"
#>   ..$ observation_id: chr "1-1"
#>   ..$ timestamp     : chr "2020-09-28 02:14:59+02:00"
#>   ..$ file_path     : chr "https://multimedia.agouti.eu/assets/aed5fa71-3ed4-4284-a6ba-3550d1a4de8d/file"
#>  $ :List of 5
#>   ..$ media_id      : chr "da81a501-8236-4cbd-aa95-4bc4b10a05df"
#>   ..$ deployment_id : chr "1"
#>   ..$ observation_id: chr "1-1"
#>   ..$ timestamp     : chr "2020-09-28 02:15:00+02:00"
#>   ..$ file_path     : chr "https://multimedia.agouti.eu/assets/da81a501-8236-4cbd-aa95-4bc4b10a05df/file"
#>  $ :List of 5
#>   ..$ media_id      : chr "0ba57608-3cf1-49d6-a5a2-fe680851024d"
#>   ..$ deployment_id : chr "1"
#>   ..$ observation_id: chr "1-1"
#>   ..$ timestamp     : chr "2020-09-28 02:15:01+02:00"
#>   ..$ file_path     : chr "https://multimedia.agouti.eu/assets/0ba57608-3cf1-49d6-a5a2-fe680851024d/file"
read_resource(package, "media")
#> # A tibble: 3 × 5
#>   media_id                      deployment_id observation_id timestamp file_path
#>   <chr>                         <chr>         <chr>          <chr>     <chr>    
#> 1 aed5fa71-3ed4-4284-a6ba-3550… 1             1-1            2020-09-… https://…
#> 2 da81a501-8236-4cbd-aa95-4bc4… 1             1-1            2020-09-… https://…
#> 3 0ba57608-3cf1-49d6-a5a2-fe68… 1             1-1            2020-09-… https://…
```

[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
adds the provided data frame to `data`:

``` r

df <- data.frame("col_1" = c(1, 2), "col_2" = c("a", "b"))
package <- add_resource(package, "df", df)
resource(package, "df")$data
#>   col_1 col_2
#> 1     1     a
#> 2     2     b
```

[`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
writes that data frame to a CSV file, adds its path to `path` and
removes `data`.

### type

[`type`](https://datapackage.org/standard/data-resource/#type) is
ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md).
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
sets `type` to `"table"` to indicate that the resource is
[tabular](https://datapackage.org/standard/data-resource/#tabular).

### schema

[`schema`](https://datapackage.org/standard/data-resource/#schema) is
required. It is used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
to parse data types and missing values. It can either be a JSON object
or a path or URL referencing a JSON object. See
[`vignette("table-schema")`](https://docs.ropensci.org/frictionless/articles/table-schema.md)
for details.

### dialect

[`dialect`](https://datapackage.org/standard/data-resource/#dialect) is
used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
to parse a tabular data file. It can either be a JSON object or a path
or URL referencing a JSON object. See
[`vignette("table-dialect")`](https://docs.ropensci.org/frictionless/articles/table-dialect.md)
for details.

### title

[`title`](https://datapackage.org/standard/data-resource/#title) is
ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md),
unless provided:

``` r

package <- add_resource(
  package,
  "iris",
  iris,
  title = "Edgar Anderson's Iris Data",
  replace = TRUE
)
resource(package, "iris")$title
#> [1] "Edgar Anderson's Iris Data"
```

### description

[`description`](https://datapackage.org/standard/data-resource/#description)
is ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
unless provided (cf. `title`).

### format

[`format`](https://datapackage.org/standard/data-resource/#format) is
ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md).
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
sets `format` when data are provided as a file, based on the provided
`delim`:

| delim           | format  |
|-----------------|---------|
| `","` (default) | `"csv"` |
| `"\t"`          | `"tsv"` |
| any other value | `"csv"` |

``` r

path <- system.file("extdata", "v2", "observations_1.tsv", package = "frictionless")
package <- add_resource(package, "observations", data = path, delim = "\t", replace = TRUE)
resource(package, "observations")$format
#> [1] "tsv"
```

[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
leaves `format` undefined when data are provided as a data frame.
[`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
sets it to `"csv"` when writing to disk.

### mediatype

[`mediatype`](https://datapackage.org/standard/data-resource/#mediatype)
is ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md).
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
sets `mediatype` when data are provided as a file, based on the provided
`delim`:

| delim           | mediatype                     |
|-----------------|-------------------------------|
| `","` (default) | `"text/csv"`                  |
| `"\t"`          | `"text/tab-separated-values"` |
| any other value | `"text/csv"`                  |

``` r

path <- system.file("extdata", "v2", "observations_1.tsv", package = "frictionless")
package <- add_resource(package, "observations", data = path, delim = "\t", replace = TRUE)
resource(package, "observations")$mediatype
#> [1] "text/tab-separated-values"
```

[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
leaves `mediatype` undefined when data are provided as a data frame.
[`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
sets it to `"text/csv"` when writing to disk.

### encoding

[`encoding`](https://datapackage.org/standard/data-resource/#encoding)
(e.g. `"windows-1252"`) is used by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
to parse the file. It defaults to UTF-8 if no `encoding` is provided or
if it cannot be recognized. The returned data frame is always UTF-8.

[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
guesses the `encoding` (using
[`readr::guess_encoding()`](https://readr.tidyverse.org/reference/encoding.html))
when data are provided as file. It leaves the `encoding` undefined when
data are provided as a data frame.
[`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
sets it to `"utf-8"` when writing to disk.

``` r

path <- system.file("extdata", "v2", "deployments.csv", package = "frictionless")
package <- add_resource(package, "deployments", data = path, delim = ",", replace = TRUE)
resource(package, "observations")$encoding
#> [1] "UTF-8"
```

### bytes

[`bytes`](https://datapackage.org/standard/data-resource/#bytes) is
ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
unless provided (cf. `title`).

### hash

[`hash`](https://datapackage.org/standard/data-resource/#hash) is
ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
unless provided (cf. `title`).

### sources

[`sources`](https://datapackage.org/standard/data-resource/#sources) is
ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
unless provided (cf. `title`).

### licenses

[`licenses`](https://datapackage.org/standard/data-resource/#licenses)
is ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
unless provided (cf. `title`).

### compression

[`compression`](https://datapackage.org/recipes/compression-of-resources/)
(a recipe) is ignored by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
and not set by
[`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md).

Compression is derived from the provided `path` instead. If the `path`
ends in `.gz`, `.bz2`, `.xz`, or `.zip`, the files are automatically
decompressed by
[`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
(using default
[`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html)
functionality).
