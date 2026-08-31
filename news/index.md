# Changelog

## frictionless (development version)

frictionless now supports Data Packages using the
[v2](https://datapackage.org/) specification, while maintaining support
for those using [v1](https://specs.frictionlessdata.io/). When creating
a package, resource or schema, it uses the v2 specification.

- frictionless no longer relies on v1 properties deprecated in v2,
  meaning all functions support both versions. They always return a
  package, resource and schema in the same version as provided.
- [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md),
  [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  and
  [`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
  create a v2 package, resource and schema respectively. **This can be a
  breaking change for some workflows!** Also note that this can lead to
  mixed versions (e.g. a v1 package with a v2 resource).
- `upgrade_package()` can be used to upgrade a package, its resources
  and verbose schemas from v1 to v2 (harmonizing mixed versions)
  ([\#357](https://github.com/frictionlessdata/frictionless-r/issues/357),
  [\#343](https://github.com/frictionlessdata/frictionless-r/issues/343),
  [\#363](https://github.com/frictionlessdata/frictionless-r/issues/363)).

### Changes for v2

- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  and
  [`schema()`](https://docs.ropensci.org/frictionless/reference/schema.md)
  no longer require `"profile": "tabular-data-resource"` (nor
  `"type": "table"`), which was the main blocker for reading resources
  defined in the v2 specification. They still expects a `schema`
  ([\#343](https://github.com/frictionlessdata/frictionless-r/issues/343)).
- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  forbids reading files from a path containing a [hidden
  directory](https://datapackage.org/overview/changelog/#resourcepath-updated)
  ([\#359](https://github.com/frictionlessdata/frictionless-r/issues/359)).
- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  supports [labelled missing
  values](https://datapackage.org/overview/changelog/#schemamissingvalues-updated)
  ([\#354](https://github.com/frictionlessdata/frictionless-r/issues/354)).
- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  supports integer
  [`groupChar`](https://datapackage.org/overview/changelog/#integer-field-type-updated)
  ([\#364](https://github.com/frictionlessdata/frictionless-r/issues/364)).
- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  supports the new
  [`list`](https://datapackage.org/overview/changelog/#list-field-type-new)
  field type. It interprets this as character
  ([\#368](https://github.com/frictionlessdata/frictionless-r/issues/368)).
- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  supports datetimes with [optional
  milliseconds](https://datapackage.org/overview/changelog/#datetime-field-type-updated)
  when `"format": "default"`
  ([\#347](https://github.com/frictionlessdata/frictionless-r/issues/347)).
- [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md)
  now **creates a v2 package** by setting
  [`$schema`](https://datapackage.org/overview/changelog/#packageschema-new)
  to the recommended v2 value
  (`"https://datapackage.org/profiles/2.0/datapackage.json"`)
  ([\#349](https://github.com/frictionlessdata/frictionless-r/issues/349)).
- [`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
  now **creates a v2 schema** by setting
  [`$schema`](https://datapackage.org/overview/changelog/#schemaschema-new)
  to the recommended v2 value
  (`"https://datapackage.org/profiles/2.0/tableschema.json"`)
  ([\#351](https://github.com/frictionlessdata/frictionless-r/issues/351)).
- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  now **creates a v2 resource** by setting
  [`$schema`](https://datapackage.org/overview/changelog/#resourceschema-new)
  to the recommended v2 value
  (`"https://datapackage.org/profiles/2.0/dataresource.json"`) and
  [`type`](https://datapackage.org/overview/changelog/#resourcetype-new)
  to `"table"`. It no longer sets `profile`. If not provided by the
  user, it will also create a v2 schema. Since it typically does not
  define a dialect, it will **default to a v1 dialect**
  ([\#343](https://github.com/frictionlessdata/frictionless-r/issues/343),
  [\#356](https://github.com/frictionlessdata/frictionless-r/issues/356)).
- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  now allows [any
  string](https://datapackage.org/overview/changelog/#resourcename-updated)
  as `resource_name`. It trims leading and trailing spaces from the name
  ([\#344](https://github.com/frictionlessdata/frictionless-r/issues/344)).
- [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  can read `datapackage.json` defined in the v1 and v2 specification
  ([\#336](https://github.com/frictionlessdata/frictionless-r/issues/336)).
- [`example_package()`](https://docs.ropensci.org/frictionless/reference/example_package.md)
  now uses the v2 specification as default
  ([\#360](https://github.com/frictionlessdata/frictionless-r/issues/360),
  [\#361](https://github.com/frictionlessdata/frictionless-r/issues/361)).
- All documentation now references the v2 specification and has been
  updated to describe
  [frictionless](https://github.com/frictionlessdata/frictionless-r)’
  support for v2 properties. Notably,
  [`schema.fieldsMatch`](https://datapackage.org/overview/changelog/#schemafieldsmatch-new)
  and
  [`field.categories`](https://datapackage.org/overview/changelog/#fieldcategories-new)
  are not yet supported
  ([\#367](https://github.com/frictionlessdata/frictionless-r/issues/367)).

### Other changes

- [`version()`](https://docs.ropensci.org/frictionless/reference/version.md)
  is now generic and can report what version of the Data Package
  standard is used by a Data Package (as before), Data Resource, Table
  Dialect and Table Schema
  ([\#341](https://github.com/frictionlessdata/frictionless-r/issues/341)).

## frictionless 1.3.0

CRAN release: 2026-07-31

### Changes for users

- New
  [`version()`](https://docs.ropensci.org/frictionless/reference/version.md)
  determines what version of the Data Package standard is used by a Data
  Package (e.g. `"1.0"`, `"2.0"`, `">=2.0"`), based on the presence and
  value of the `$schema` property
  ([\#299](https://github.com/frictionlessdata/frictionless-r/issues/299)).
  This information is also returned by
  [`print()`](https://rdrr.io/r/base/print.html)
  ([\#302](https://github.com/frictionlessdata/frictionless-r/issues/302)).
- [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  now warns when reading a `datapackage.json` that uses a version of the
  Data Package standard not supported by frictionless (i.e. anything
  other than version `"1.0"`)
  ([\#309](https://github.com/frictionlessdata/frictionless-r/issues/309)).
- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  no longer guesses the type for fields without a `type`, but sets it to
  character (the default for a CSV). This aligns with a clarification in
  the
  [specification](https://datapackage.org/overview/changelog/#any-field-type-updated)
  ([\#296](https://github.com/frictionlessdata/frictionless-r/issues/296)).
- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  now supports reading from remote zip files, thanks to support in
  [vroom](https://vroom.tidyverse.org) (1.3.0)
  ([\#291](https://github.com/frictionlessdata/frictionless-r/issues/291)).
- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  with `replace = TRUE` adds the resource if there is none to replace,
  rather than throwing an error
  ([\#273](https://github.com/frictionlessdata/frictionless-r/issues/273)).
- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  now retains the URL to a provided schema, rather than including it
  verbosely
  ([\#305](https://github.com/frictionlessdata/frictionless-r/issues/305)).
- [`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)’s
  overwrite behavior is now as intended and documented in the function
  ([\#304](https://github.com/frictionlessdata/frictionless-r/issues/304),
  [\#313](https://github.com/frictionlessdata/frictionless-r/issues/313)).
- [`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
  now prints multiple `resource$path`, `resource$schema$missingValues`
  and `field$constraints$enum` on multiple lines in the
  `datapackage.json`
  ([\#297](https://github.com/frictionlessdata/frictionless-r/issues/297)).
- [`resources()`](https://docs.ropensci.org/frictionless/reference/deprecated.md)
  is soft-deprecated, please use
  [`resource_names()`](https://docs.ropensci.org/frictionless/reference/resource_names.md)
  instead
  ([\#282](https://github.com/frictionlessdata/frictionless-r/issues/282)).
- [`get_schema()`](https://docs.ropensci.org/frictionless/reference/deprecated.md)
  is soft-deprecated, please use
  [`schema()`](https://docs.ropensci.org/frictionless/reference/schema.md)
  instead
  ([\#282](https://github.com/frictionlessdata/frictionless-r/issues/282)).

### Changes for developers

- frictionless now relies on R \>= 4.1.0 (because of an indirect
  [vroom](https://vroom.tidyverse.org) dependency)
  ([\#291](https://github.com/frictionlessdata/frictionless-r/issues/291))
  and uses base pipes (`|>` rather than `%>%`)
  ([\#292](https://github.com/frictionlessdata/frictionless-r/issues/292)).

- [`resource()`](https://docs.ropensci.org/frictionless/reference/resource.md)
  is now a public function, making it possible to get a resource object
  (i.e. a list) by its name. This is especially useful if you want to
  implement reading functionality not supported by frictionless
  ([\#303](https://github.com/frictionlessdata/frictionless-r/issues/303)).

- New `resource()<-` allows to overwrite a resource in place
  ([\#314](https://github.com/frictionlessdata/frictionless-r/issues/314)).

- Internal frictionless properties `package$directory` and
  `resource$read_from` are now *attributes* `attr(package, "directory")`
  and `attr(resource, "data_location")`. This separates them better from
  public Data Package and Resource *properties*
  ([\#289](https://github.com/frictionlessdata/frictionless-r/issues/289)).
  Saved Data Package objects created with previous versions of
  frictionless will show a deprecation warning
  ([\#293](https://github.com/frictionlessdata/frictionless-r/issues/293))
  and can be updated with
  [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md).
  If you use these internal properties in your R package, then change
  them:

  ``` r

  # Before
  package$directory
  resource <- frictionless:::get_resource(package, "resource_name") # Internal function
  resource$read_from

  # After
  attr(package, "directory")
  resource <- frictionless:::resource(package, "resource_name") # Function renamed
  attr(resource, "data_location") # Attribute renamed
  ```

### Bug fixes

- Single element arrays (e.g. `"key": ["value"]`) in a
  `datapackage.json` file are now retained when reading and writing
  ([\#276](https://github.com/frictionlessdata/frictionless-r/issues/276)).

## frictionless 1.2.1

CRAN release: 2025-05-23

- frictionless now relies on R version 3.6.0 or higher. Originally it
  stated version 3.5.0 or higher, but this was not tested and likely not
  true
  ([\#238](https://github.com/frictionlessdata/frictionless-r/issues/238)).
- [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  now returns a warning rather than an error when a `datapackage.json`
  contains no resources. This allows use to create the JSON and then add
  resources with frictionless
  ([\#265](https://github.com/frictionlessdata/frictionless-r/issues/265)).
- [`example_package()`](https://docs.ropensci.org/frictionless/reference/example_package.md)
  now has a `version` parameter, allowing to load the example Data
  Package following the Data Package
  [v1](https://specs.frictionlessdata.io/) or
  [v2](https://datapackage.org/) specification
  ([\#249](https://github.com/frictionlessdata/frictionless-r/issues/249)).

## frictionless 1.2.0

CRAN release: 2024-08-28

### Changes for users

- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  now allows to replace an existing resource
  ([\#227](https://github.com/frictionlessdata/frictionless-r/issues/227)).
- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  now returns an error if both `path` and `data` are provided
  ([\#143](https://github.com/frictionlessdata/frictionless-r/issues/143)).
- [`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
  no longer writes to `"."` by default, since this is not allowed by
  CRAN policies. The user needs to explicitly define a directory
  ([\#205](https://github.com/frictionlessdata/frictionless-r/issues/205)).
- `null` values in a read `datapackage.json` are now retained by
  [`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md),
  rather than being changed to empty lists. Properties assigned by the
  user to `NA` and `NULL` remain being written as `null` and removed
  respectively
  ([\#203](https://github.com/frictionlessdata/frictionless-r/issues/203)).
- New vignettes
  [`vignette("data-package")`](https://docs.ropensci.org/frictionless/articles/data-package.md),
  [`vignette("data-resource")`](https://docs.ropensci.org/frictionless/articles/data-resource.md),
  [`vignette("table-dialect")`](https://docs.ropensci.org/frictionless/articles/table-dialect.md)
  and
  [`vignette("table-schema")`](https://docs.ropensci.org/frictionless/articles/table-schema.md)
  describe how frictionless implements the Data Package standard. The
  (verbose) function documentation of
  [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  and
  [`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
  has been moved to these vignettes, improving readability and
  maintenance
  ([\#208](https://github.com/frictionlessdata/frictionless-r/issues/208),
  [\#246](https://github.com/frictionlessdata/frictionless-r/issues/246)).
- The included dataset `example_package` is removed in favour of
  [`example_package()`](https://docs.ropensci.org/frictionless/reference/example_package.md).
  This function allows to reproducibly provide a local Data Package,
  without the need for an internet connection. The `observations`
  resource was also changed from a remote to a local resource and from
  CSV to TSV. **This change affects the use of `example_package` in
  older versions of frictionless.** We recommend to update frictionless
  to the latest version
  ([\#114](https://github.com/frictionlessdata/frictionless-r/issues/114),
  [\#253](https://github.com/frictionlessdata/frictionless-r/issues/253)).

### Changes for developers

- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  is now more modular under the hood, which should make it easier to
  extend
  ([\#210](https://github.com/frictionlessdata/frictionless-r/issues/210)).
- [checklist](https://github.com/inbo/checklist) tooling was removed, in
  favour of `CITATION.cff` for citation and Zenodo deposit
  ([\#206](https://github.com/frictionlessdata/frictionless-r/issues/206)).

### Other changes

- Add [Sanne Govaert](https://orcid.org/0000-0002-8939-1305) as author.
  Welcome Sanne!

## frictionless 1.1.0

CRAN release: 2024-03-29

### Changes for users

- New [`print()`](https://rdrr.io/r/base/print.html) prints a
  human-readable summary of the Data Package, rather than a (long) list
  ([\#155](https://github.com/frictionlessdata/frictionless-r/issues/155)).
- [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  no longer returns a message regarding rights and credit
  ([\#121](https://github.com/frictionlessdata/frictionless-r/issues/121)).
  If `package$id` is a URL (e.g. a DOI) it will be mentioned in
  [`print()`](https://rdrr.io/r/base/print.html).
- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  accepts additional arguments via `...`. These are added as (custom)
  properties to the resource and are retained in
  [`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
  ([\#195](https://github.com/frictionlessdata/frictionless-r/issues/195)).
- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  now supports column selection via the `col_select` argument from
  [`readr::read_delim()`](https://readr.tidyverse.org/reference/read_delim.html).
  This can vastly improve reading speed
  ([\#123](https://github.com/frictionlessdata/frictionless-r/issues/123)).
  [Tidy
  selection](https://dplyr.tidyverse.org/reference/dplyr_tidy_select.html)
  is not supported.
- [`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
  no longer adds `"profile": "tabular-data-package"` to
  `datapackage.json`. It is also removed from the example dataset
  ([\#188](https://github.com/frictionlessdata/frictionless-r/issues/188)).
- Error and warning messages use semantic colours for variables,
  parameters, fields, etc.
- [`readr::problems()`](https://readr.tidyverse.org/reference/problems.html)
  is included in NAMESPACE so you don’t have to load readr to inspect
  parsing issues. The function is mentioned in the documentation of
  [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  ([\#129](https://github.com/frictionlessdata/frictionless-r/issues/129)).

### Changes for developers

- A Data Package object (`package`) now has a `datapackage` class
  ([\#184](https://github.com/frictionlessdata/frictionless-r/issues/184)).
  This enables a custom [`print()`](https://rdrr.io/r/base/print.html)
  function (see above).
  [`check_package()`](https://docs.ropensci.org/frictionless/reference/check_package.md)
  will warn if the class is missing, **so previously saved Data Package
  objects (without the class) will generate a warning**.
- [`check_package()`](https://docs.ropensci.org/frictionless/reference/check_package.md)
  is now a public function, so it can be used in your package
  ([\#185](https://github.com/frictionlessdata/frictionless-r/issues/185)).
  This and the other `check_` functions return the first argument
  silently (rather than `TRUE`), so they can be chained.
- [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md)
  now accepts a `descriptor` argument so that a Data Package object can
  be created from an existing object
  ([\#184](https://github.com/frictionlessdata/frictionless-r/issues/184)).
  It will always validate the created object with
  [`check_package()`](https://docs.ropensci.org/frictionless/reference/check_package.md).
- [`cli::cli_abort()`](https://cli.r-lib.org/reference/cli_abort.html),
  [`cli::cli_warn()`](https://cli.r-lib.org/reference/cli_abort.html)
  and
  [`cli::cli_inform()`](https://cli.r-lib.org/reference/cli_abort.html)
  are used for all errors, warnings, and messages
  ([\#163](https://github.com/frictionlessdata/frictionless-r/issues/163)).
  This has several advantages:
  - Messages use semantic colours for variables, parameters, fields,
    etc.
  - Messages and warnings can be silenced with a global or local option,
    see [this blog
    post](https://ropensci.org/blog/2024/02/06/verbosity-control-packages/).
  - Each call has an [rlang](https://rlang.r-lib.org) class,
    e.g. `frictionless_error_fields_without_name`, making it easier to
    test for specific errors.
- [glue](https://glue.tidyverse.org/) and `{assertthat}` are removed as
  dependencies
  ([\#163](https://github.com/frictionlessdata/frictionless-r/issues/163)).
  The functionality of glue is replaced by cli, while
  `assertthat::assert()` calls are now `if ()` statements.
- [rlang](https://rlang.r-lib.org) is added as dependency
  ([\#192](https://github.com/frictionlessdata/frictionless-r/issues/192)).
  It is already used by other dependencies.
- frictionless now depends on R \>= 3.5.0.

### Other changes

- The package now adheres to the requirements of
  [checklist](https://github.com/inbo/checklist), so that `.zenodo.json`
  can be created with `checklist::update_citation()`.
- Add [Pieter Huybrechts](https://orcid.org/0000-0002-6658-6062) as
  author and [Kyle Husmann](https://orcid.org/0000-0001-9875-8976) as
  contributor. Welcome both!

## frictionless 1.0.3

CRAN release: 2024-03-07

- Add [stringi](https://stringi.gagolewski.com/) to `Suggests`. It was
  removed as a dependency from
  [rmarkdown](https://github.com/rstudio/rmarkdown) 2.26, resulting in
  “stringi package required for encoding operations” build errors on
  CRAN
  ([\#176](https://github.com/frictionlessdata/frictionless-r/issues/176)).

## frictionless 1.0.2

CRAN release: 2022-11-16

- Add `skip_if_offline()` to selected tests and verbosely include output
  in vignette examples, to avoid CRAN errors caused by timeouts
  ([\#116](https://github.com/frictionlessdata/frictionless-r/issues/116)).

## frictionless 1.0.1

CRAN release: 2022-09-08

- Rebuild documentation for compatibility with HTML5 on request of CRAN.
- Add funder information.

## frictionless 1.0.0

CRAN release: 2022-02-16

- First release on
  [CRAN](https://cran.r-project.org/package=frictionless). 🎉
- Files (`datapackage.json`, resource files, schemas) can now be read
  from `(s)ftp://` URLs
  ([\#102](https://github.com/frictionlessdata/frictionless-r/issues/102)).
- Package website is now served from
  <https://docs.ropensci.org/frictionless/>.

## frictionless 0.11.0

- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  now sets `format`, `mediatype` and `encoding` for added CSV file(s)
  ([\#78](https://github.com/frictionlessdata/frictionless-r/issues/78)).
- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  now supports adding `schema` via path or URL.
- [`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
  now supports added data to be gzip compressed before being written to
  disk
  ([\#98](https://github.com/frictionlessdata/frictionless-r/issues/98)).
- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  will now warn rather than error on unknown encoding
  ([\#86](https://github.com/frictionlessdata/frictionless-r/issues/86)).
- `package` objects no longer have or require the custom attribute
  `resource_names`, use the new
  [`resources()`](https://docs.ropensci.org/frictionless/reference/deprecated.md)
  instead
  ([\#97](https://github.com/frictionlessdata/frictionless-r/issues/97)).
- `package` objects no longer have or require the custom attribute
  `datapackage`, making it easier to edit them as lists (with
  e.g. [`append()`](https://rdrr.io/r/base/append.html)).

## frictionless 0.10.0

- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)
  now supports adding CSV file(s) directly as a resource. This skips
  reading/handling by R and gives users control over `path`
  ([\#74](https://github.com/frictionlessdata/frictionless-r/issues/74)).
- CSV files in a remotely read package (like `example_package`) are now
  downloaded when writing with
  [`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md),
  rather than being skipped. This is more consistent with locally read
  packages. The behaviour for resources with a `path` containing URLs
  (only) and resources with `data` remains the same (no files are
  written). The write behaviour is better explained in the documentation
  ([\#77](https://github.com/frictionlessdata/frictionless-r/issues/77)).
- [`write_package()`](https://docs.ropensci.org/frictionless/reference/write_package.md)
  now silently returns the output rather than input `package`.
- [`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md)
  will set `"profile": "tabular-data-package"` since packages created by
  frictionless meet those requirements
  ([\#81](https://github.com/frictionlessdata/frictionless-r/issues/81)).
- [`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)
  interprets empty columns as `string` not `boolean`
  ([\#79](https://github.com/frictionlessdata/frictionless-r/issues/79)).
- [`read_package()`](https://docs.ropensci.org/frictionless/reference/read_package.md)
  can now read from a `datapackage.yaml` file.
- [`read_resource()`](https://docs.ropensci.org/frictionless/reference/read_resource.md)
  now accepts YAML Table Schemas and CSV dialects.
- [`add_resource()`](https://docs.ropensci.org/frictionless/reference/add_resource.md)/[`create_schema()`](https://docs.ropensci.org/frictionless/reference/create_schema.md)’s
  `df` argument is renamed to `data`.
- `example_package`’s `observations` resource now has URLs as `path` to
  serve as an example for that.

## frictionless 0.9.0

- Add vignette with overview of functionality
  ([\#60](https://github.com/frictionlessdata/frictionless-r/issues/60)).
- Prepare frictionless for rOpenSci submission.
