# OSM Fieldwork Project

Osm-Fieldwork is a project for processing data collection using
OpenDataKit into OpenStreetMap format. It includes several utility
programs that automate part of the data flow like creating satellite
imagery basemaps and data extracts from
[OpenStreetMap](https://www.openstreetmap.org) so they can be
used with [ODK Collect](https://www.getodk.org). Many of these steps
are currently a manual process.

All of the programs in osm-fieldwork are designed to function as the
backend of a webpage, but to also work standalone and offline. The
standalone functionality are simple command line programs run in a
terminal. They were originally created for producing emergency
response maps in the Western United States, which is explained in this
talk from SOTM-US 2022 titled [OSM For
Firefighting](https://www.youtube.com/watch?v=qgk9al1rluE). Much of
the tech and usage is explained in these [tech
briefs](https://www.senecass.com/projects/Mapping/tech/). Currently
these are now part of the backend for the [Field Mapping Tasking Manager](
https://github.com/hotosm/fmtm/wiki) project at
[HOT](https://www.hotosm.org).

## Installation

To install osm-fieldwork, you can use pip. Here are two options:

- Directly from the main branch:
  `pip install git+https://github.com/hotosm/osm-fieldwork.git`

- Latest on PyPi:
  `pip install Osm-Fieldwork`

### Configure

Osm-Fieldwork can be configured using a simple config
($HOME/.osm-fieldwork)file in your home directory, or using environment
variables.

#### Config file

The config file is uswd to store the credentials to access an ODK
Central server. You must have an account on the Central server of
course for this to work. That file looks like this:

	url=https://foo.org
	user=foo@bar.org
	passwd=arfood

#### Environment Variables

**LOG_LEVEL**
> If present, will change the log level. Defaults to DEBUG.

**ODK_CENTRAL_URL**
> The URL for an ODKCentral server to connect to.

**ODK_CENTRAL_USER**
> The user for ODKCentral.

**ODK_CENTRAL_PASSWD**
> The password for ODKCentral.

**ODK_CENTRAL_SECURE**
> If set to False, will allow insecure connections to the ODKCentral API. Else defaults to True.

# Utility Programs

These programs are more fully documented [in this](docs/programs.md)
file. This is just a short overview.

## CSVDump.py

This program converts the data collected from ODK Collect into
the proper OpenStreetMap tagging schema. The conversion is controled
by an
[YAML](https://github.com/hotosm/osm-fieldwork/blob/main/osm-fieldwork/xforms.yaml)
file, so easy to modify for other projects. The output are two files,
one is suitable for OSM,and is in OSM XML format. The other
No converted data should ever be uploaded to OSM without validating
the conversion in JOSM. To do efficient conversion from ODK to OSM,
it's best to use the XLSForm library as templates, as everything is
designed to work together. 

## basemapper.py

This program creates basemaps of satellite imagery, and produces files
in mbtiles format for [ODK
Collect](https://docs.getodk.org/collect-intro/) and sqlitedb files
for [Osmand](https://osmand.net/). Imagery basemaps are very useful
when the map data is lacking.or in ODK Collect, selecting the corrent
location instead of where you are standing. The basemaps Osmand are
very useful of navigation where the map data is lacking. Imageruy
vanbe downloaded from
[ERSI](https://www.arcgis.com/home/item.html?id=10df2279f9684e4a9f6a7f08febac2a9#!),
[Bing](https://www.arcgis.com/home/webmap/viewer.html?webmap=8651e4d585654f6b955564efe44d04e5#!),
[USGS Topo maps](https://apps.nationalmap.gov/datasets/), or [Open
Aerial Map](https://openaerialmap.org/)

## make_data_extract.py

This program makes data extracts from
[OpenStreetMap](https://www.openstreetmap.org) data. Multiple input
sources are supported, a local postgresql database, the HOT maintained
[Underpass](https://galaxy.hotosm.org/) database, or
[Overpass](https://overpass-turbo.eu). 

## odk2csv.py

This program is used when working offline for extended periods. This
converts the ODK XML format on your mobile device into the same CSV
format used for submissions downloaded from [ODK
Central](https://docs.getodk.org/central-intro/)

## odk_client.py

This program is a simple command line client to an ODK Central
server. This allows you to list projects, appusers, tasks, and
submissions. You can also delete projects, tasks, and appusers, but
this should only be [used by
developers](https://en.wikipedia.org/wiki/With_great_power_comes_great_responsibility)
as it does direct database access, and you could loose all your data.

## filter_data.py

This program is used to support humanitariam data models. It extracts
the tags and values from the [data models
document](osm-fieldwork/data_models/Impact Areas - Data Models V1.1.xlsx)
developed by HOT, and compares those to the taginfo database to help
fine tune what data goes into OSM or the private output data. This is
to not flood OSM with obscure tags that aren't supported by the
community. It also filters data extracts so they work with ODK
Collect.

# Best Practices

To ensure the quality of your converted data, here are some best
practices to follow: 

- Always validate your conversion in JOSM before uploading to OpenStreetMap.

- Use the XLSForm library as templates to ensure that your ODK Collect
data is compatible with the conversion process. 

- If you're having trouble with the conversion process, try using the
utility programs included with Osm-Fieldwork to troubleshoot common
issues. 

By following these best practices and using the utility programs
included with Osm-Fieldwork, you can effectively process data collection
from OpenDataKit into OpenStreetMap format. However, please note that
while Osm-Fieldwork has been tested and used in various projects, it is
still in active development and may have limitations or issues that
need to be resolved.

## XLSForm library

In the XForms directory is a collection of XLSForms that support the
new HOT data models for humanitarian data collection. These cover
many categories like healthcare, waterpoints, waste distribution,
etc... All of these XLSForms are designed to have an efficient mapper
data flow, edit existing OSM data, and support the data models.

The data models specify the preferred tag values for each data item,
with a goal of both tag completeness and tag correctness. Each data item
is broken down into a basic and extended survey questions when
appropriate.

### What is an XLSForm?

An XLSForm is a spreadsheet-based form design tool that allows you to
create complex forms for data collection using a simple and intuitive
user interface. With XLSForms, you can easily design and test forms on
your computer, then deploy them to mobile devices for data collection
using ODK Collect or other data collection tools. XLSForms use a
simple and structured format, making it easy for you to share and
collaborate on form designs with your team or other organizations. 

### Using the XLSForm Library with Osm-Fieldwork
The XLSForms in the XForms directory of the XLSForm Library have been
designed to support the HOT data models and have an efficient mapper
data flow. These forms also allow for editing of existing OSM data and
support the data models, specifying the preferred tag values for each
data item with the goal of both tag completeness and tag correctness.

### Here are some examples of how to use the XLSForm Library with Osm-Fieldwork:

- Download an XLSForm from the XForms directory:

	wget https://github.com/hotosm/xlsform/raw/master/XForms/buildings.xls

- Convert the XForm to OSM XML using CSVDump:

- Use the resulting OSM XML file with JOSM or other OSM editors to
validate and edit the data before uploading it to OpenStreetMap. 

### Conclusion

The XLSForm Library is a valuable resource for organizations involved
in humanitarian data collection, as it provides a collection of
pre-designed forms that are optimized for efficient mapper data flow
and tag completeness/correctness. By using the XLSForm Library with
Osm-Fieldwork, you can streamline your data collection process and ensure
the quality of your data. 

Osm-Fieldwork is a powerful tool for processing data collection from
OpenDataKit into OpenStreetMap format. By following the best practices
outlined in this documentation and using the utility programs included
with Osm-Fieldwork, you can streamline your data collection process and
ensure the quality of your converted data. If you have any questions
or issues with osm-fieldwork, please consult the project's documentation
or seek support from the project's community.
