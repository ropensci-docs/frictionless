# Write a Data Package to disk

Writes a Data Package and its related Data Resources to disk as a
`datapackage.json` and CSV files.

## Usage

``` r
write_package(package, directory, compress = FALSE)
```

## Arguments

- package:

  Data Package object, as returned by
  [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  or
  [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md).

- directory:

  Path to local directory to write files to.

- compress:

  If `TRUE`, data of added resources will be gzip compressed before
  being written to disk (e.g. `deployments.csv.gz`).

## Value

`package` invisibly, as written to file.

## Writing data to CSV files

`write_package()` will write data to CSV files depending on how these
are attached to the Data Resource:

- **Data frame**:

      add_resource(package, "media", data = df)

  Data are written to a CSV file in `directory` using
  [`readr::write_csv()`](https://readr.tidyverse.org/reference/write_delim.html).
  The CSV file will have the same name as the resource, overwriting any
  existing file with the same name. Use `compress = TRUE` to gzip the
  CSV file.

- One or more **paths** to local CSV files in a **different directory**:

      add_resource(package, "media", data = "other-directory/media.csv")

  CSV files are copied to `directory`, overwriting existing files with
  the same name.

- One or more **paths** to local CSV files in the **same directory**:

      add_resource(package, "media", data = "directory/media.csv")

  CSV files are left as is (no overwrite). This allows you to read and
  write a `datapackage.json` to the same directory, without altering the
  CSV files of resources you did not manipulate.

- One or more **URLs** to CSV files:

      add_resource(package, "media", data = "https://example.org/media.csv")

  Files are not downloaded.

- Mix of **URLs and paths** to CSV files:

      add_resource(
        package, "media",
        data = c("https://example.org/media.csv", "media.csv")
      )

  Remote CSV files are downloaded to `directory`, overwriting existing
  files with the same name. Local CSV files are handled as described
  above.

- **Inline data**: No files are written.

In all above cases `path` is added or updated to the new file
location(s) when appropriate.

## Examples

``` r
# Load the example Data Package from disk
package <- read_package(
  system.file("extdata", "v1", "datapackage.json", package = "frictionless")
)

package
#> A Data Package (version 1.0) with 3 resources:
#> • deployments
#> • observations
#> • media
#> Use `unclass()` to print the Data Package as a list.

# Write the (unchanged) Data Package to disk
write_package(package, directory = "my_directory")

# Check files
list.files("my_directory")
#> [1] "datapackage.json"   "deployments.csv"    "observations_1.tsv"
#> [4] "observations_2.tsv"

# No files written for the "observations" resource, since those are all URLs.
# No files written for the "media" resource, since it has inline data.

# Clean up (don't do this if you want to keep your files)
unlink("my_directory", recursive = TRUE)
```
