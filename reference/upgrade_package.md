# Upgrade a Data Package to v2

Upgrades a Data Package, its Data Resources and Table Schemas to the
[v2](https://datapackage.org/) specification.

## Usage

``` r
upgrade_package(package)
```

## Arguments

- package:

  Data Package object, as returned by
  [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  or
  [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md).

## Value

Upgraded `package`.

## Upgrade details

### Data Package

`upgrade_package()` upgrades a
[v1](https://specs.frictionlessdata.io/data-package/) descriptor to
[v2](https://datapackage.org/standard/data-package/) as follows:

- Adds `$schema` as first property and sets it to the recommended v2
  value (`"https://datapackage.org/profiles/2.0/datapackage.json"`),
  except for certain `profile` values.

- Removes `profile`, but retains its value in `$schema` if it is a URL
  to a custom profile (see [backwards
  compatibility](https://datapackage.org/standard/data-package/#dollar-schema)).

- Converts `contributors` `"role": "value"` to `"roles": ["value"]` (see
  [changelog](https://datapackage.org/overview/changelog/#packagecontributors-updated)).

### Data Resource

`upgrade_package()` upgrades any
[v1](https://specs.frictionlessdata.io/data-resource/) resource to
[v2](https://datapackage.org/standard/data-resource/) as follows:

- Adds `$schema` as first property and sets it to the recommended v2
  value (`"https://datapackage.org/profiles/2.0/dataresource.json"`).

- Removes `profile`, but converts `"profile" = "tabular-data-resource"`
  to `"type" = "table"` (see [backwards
  compatibility](https://datapackage.org/standard/data-resource/#type)).

### Table Dialect

`upgrade_package()` leaves the dialect as is for all resources.

### Table Schema

`upgrade_package()` upgrades any verbose
[v1](https://specs.frictionlessdata.io/table-schema/) schema to
[v2](https://datapackage.org/standard/table-schema/) as follows:

- Adds `$schema` as first property and sets it to the recommended v2
  value (`"https://datapackage.org/profiles/2.0/tableschema.json"`).

- Converts `primaryKey` single values to an array (see
  [changelog](https://datapackage.org/overview/changelog/#schemaprimarykey-updated)).

- Converts `foreignKeys` single values in `fields` to an array and
  removes `reference$resource` if it is self-referential (see
  [changelog](https://datapackage.org/overview/changelog/#schemaforeignkeys-updated)).

Schemas referenced by path or URL are left as is.

## See also

Other versioning functions:
[`version()`](https://docs.ropensci.org/frictionless/reference/version.md)

## Examples

``` r
# Load the v1 example Data Package
(package <- example_package(version = "1.0"))
#> A Data Package (version 1.0) with 3 resources:
#> • deployments
#> • observations
#> • media
#> Use `unclass()` to print the Data Package as a list.

# Upgrade
(package_upgraded <- upgrade_package(package))
#> A Data Package (version 2.0) with 3 resources:
#> • deployments
#> • observations
#> • media
#> Use `unclass()` to print the Data Package as a list.

# Check version of one of the resources
version(resource(package_upgraded, "deployments"))
#> [1] "2.0"
```
