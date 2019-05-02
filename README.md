Intake-stac
===========

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/pangeo-data/intake-stac/master?filepath=examples?urlpath=lab)
[![Build Status](https://travis-ci.com/pangeo-data/intake-stac.svg?branch=master)](https://travis-ci.com/pangeo-data/intake-stac)
[![Coverage Status](https://coveralls.io/repos/github/pangeo-data/intake-stac/badge.svg?branch=master)](https://coveralls.io/github/pangeo-data/intake-stac?branch=master)

This is an [intake](https://intake.readthedocs.io/en/latest)
data source for [STAC](https://stacspec.org/) catalogs.

The SpatioTemporal Asset Catalog (STAC) specification provides a common language to describe a range of geospatial information, so it can more easily be indexed and discovered. A 'spatiotemporal asset' is any file that represents information about the earth captured in a certain space and time.

Two examples of these catalogs are:

- https://planet.stac.cloud/?t=catalogs
- https://landsat-stac.s3.amazonaws.com/catalog.json

Radient Earth keeps track of a more complete listing of STAC implementations [here](https://github.com/radiantearth/stac-spec/blob/master/implementations.md).

This project provides an opinionated way for users to load datasets from these catalogs into the scientific Python ecosystem.
Currently it uses the intake-xarray pluging and support datatypes including GeoTIFF, netCDF, GRIB, and OpenDAP. Future formats could include plain shapefile data and more.

## Requirements
```
intake >= 0.4.4
intake-xarray >= 0.3.0
sat-stac >= 0.1.2
```

## Installation

`intake-stac` will eventually be published on PyPI. For now, you can point to xarray
You can install it by running the following in your terminal:
```bash
pip install git+https://github.com/pangeo-data/intake-stac
```

You can test the functionality by opening the example notebooks in the `examples/` directory:

### Usage

The package can be imported using
```python
from intake_stac import STACCatalog
```

### Loading a catalog

You can load data from a DCAT catalog by providing the URL to the `data.json` file:
```python
catalog = STACCatalog('https://storage.googleapis.com/pdd-stac/disasters/catalog.json', 'planet-disaster-data')
list(catalog)
```

You can display the items in the catalog
```python
for entry_id, entry in catalog.items():
    display(entry)
```

If the catalog has too many entries to comfortably print all at once,
you can narrow it by searching for a term (e.g. 'district'):
```python
for entry_id, entry in catalog.search('thumbnail').items():
  display(entry)
```

### Loading a dataset
Once you have identified a dataset, you can load it into a `xarray.DataArray` using `to_dask()`:

```python
da = entry.to_dask()
```