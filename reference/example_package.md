# Read the example Data Package

Reads the example Data Package included in `frictionless`. This dataset
is used in examples, vignettes, and tests and contains dummy camera trap
data organized in 3 Data Resources:

1.  `deployments`: one local data file referenced in
    `"path": "deployments.csv"`.

2.  `observations`: two local data files referenced in
    `"path": ["observations_1.tsv", "observations_2.tsv"]`.

3.  `media`: inline data stored in `data`.

## Usage

``` r
example_package(version = "2.0")
```

## Arguments

- version:

  Data Package standard version number.

## Value

A Data Package object, see
[`create_package()`](https://docs.ropensci.org/frictionless/reference/create_package.md).

## Details

The example Data Package is available in two versions:

- `1.0`: specified in the [v1](https://specs.frictionlessdata.io/)
  specification.

- `2.0`: specified in the [v2](https://datapackage.org/) specification.

## Examples

``` r
# Version 2 (default)
example_package()
#> A Data Package (version 2.0) with 3 resources:
#> • deployments
#> • observations
#> • media
#> Use `unclass()` to print the Data Package as a list.

# Version 1
example_package(version = "1.0")
#> A Data Package (version 1.0) with 3 resources:
#> • deployments
#> • observations
#> • media
#> Use `unclass()` to print the Data Package as a list.
```
