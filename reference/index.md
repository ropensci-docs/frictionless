# Package index

## Read a Data Package

Read a `datapackage.json` file and its Data Resources from path or URL.

- [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  :

  Read a Data Package descriptor file (`datapackage.json`)

- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  : Read data from a Data Resource into a tibble data frame

## Create and edit a Data Package

Create and edit a Data Package, its Data Resources and Table Schemas.

- [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md)
  : Create a Data Package
- [`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
  : Create a Table Schema from a data frame
- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  : Add a Data Resource
- [`remove_resource()`](https://docs.ropensci.org/frictionless/reference/remove_resource.md)
  : Remove a Data Resource

## Write a Data Package

Write a Data Package and its Data Resources to disk.

- [`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
  : Write a Data Package to disk

## Versioning

Determine what version of the Data Package standard is used
([v1](https://specs.frictionlessdata.io/) or
[v2](https://datapackage.org/)) and/or upgrade. frictionless uses the v2
specification when creating a package, resource or schema.

- [`upgrade_package()`](https://docs.ropensci.org/frictionless/reference/upgrade_package.md)
  : Upgrade a Data Package to v2
- [`version()`](https://docs.ropensci.org/frictionless/reference/version.md)
  : Get the specification version number

## Developing with frictionless

Access, set and check Data Package properties.

- [`resource()`](https://docs.ropensci.org/frictionless/reference/resource.md)
  [`` `resource<-`() ``](https://docs.ropensci.org/frictionless/reference/resource.md)
  : Get or overwrite a Data Resource
- [`resource_names()`](https://docs.ropensci.org/frictionless/reference/resource_names.md)
  : List Data Resource names
- [`schema()`](https://docs.ropensci.org/frictionless/reference/schema.md)
  : Get the Table Schema of a Data Resource
- [`check_package()`](https://docs.ropensci.org/frictionless/reference/check_package.md)
  : Check a Data Package object

## Miscellaneous

- [`print(`*`<datapackage>`*`)`](https://docs.ropensci.org/frictionless/reference/print.datapackage.md)
  : Print a Data Package
- [`example_package()`](https://docs.ropensci.org/frictionless/reference/example_package.md)
  : Read the example Data Package
