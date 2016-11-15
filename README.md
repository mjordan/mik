# MIK, the Move to Islandora Kit. [![Build Status](https://travis-ci.org/MarcusBarnes/mik.png?branch=master)](https://travis-ci.org/MarcusBarnes/mik)

> Important: As of commit  72b5d88 (Oct. 21, 2016), MIK's command syntax has changed slightly. Previously, MIK's command-line options used an equal sign (`=`) to separate the option name from its value; after commit  72b5d88, options use a space instead of an equals sign.

> * Before: `./mik --config=foo.ini --checkconfig=all --limit=10`
> * After: `./mik --config foo.ini --checkconfig all --limit 10`

> This change also introduces short forms of the options, e.g., `./mik -c foo.ini -l 10`

## Overview

The Move to Islandora Kit (MIK) converts source content files and accompanying metadata into ingest packages used by existing Islandora batch ingest modules, [Islandora Batch](https://github.com/Islandora/islandora_batch), [Islandora Newspaper Batch](https://github.com/discoverygarden/islandora_newspaper_batch), [Islandora Book Batch](https://github.com/Islandora/islandora_book_batch), and [Islandora Compound Batch](https://github.com/MarcusBarnes/islandora_compound_batch). In other words, it doesn’t import objects into Islandora, it prepares content for importing into Islandora:

![MIK overview](https://dl.dropboxusercontent.com/u/1015702/linked_to/MIK_overview_general.png)

MIK is designed to be extensible. The base classes that convert the source metadata to XML files for importing into Islandora, and that convert the source content files into the required directory structure for importing, can be subclassed easily. MIK also uses plugins (known as "manipulators") and a set of "hook" scripts that allow functionality that can be turned off or on for specific jobs.

MIK was originally developed by staff at Simon Fraser University Library in support of their migration from CONTENTdm to Islandora, but its longer-term purpose is as a general toolkit for preparing content for importing content into Islandora. So MIK should really stand for "Move [content into] Islandora Kit."

## Documentation

We are continuing to improve our documentation, which is on the [MIK wiki](https://github.com/MarcusBarnes/mik/wiki). Please let us know if you have any questions, suggestions or if you would like to assist.

## Troubleshooting and support

If you have a question, please [open an issue](https://github.com/MarcusBarnes/mik/issues).

## Islandora content that has been prepared using MIK

* Some of Emily Carr University of Art and Design's collections in [Arca](http://arcabc.ca/)
  * [Academic Calendars](http://arcabc.ca/islandora/object/ecuad:cals)
  * [Wosk Masterworks Print Collection](arcabc.ca/islandora/object/ecuad:wosk)
* All of the newspapers in Simon Fraser University Library's [Digitized Newspapers](http://newspapers.lib.sfu.ca/) and [Chinese Times](http://chinesetimes.lib.sfu.ca/) sites
* All of the collections in Simon Fraser University Library's [Digitized Collections](http://digital.lib.sfu.ca/)

## Installation

Instructions are [available on the wiki](https://github.com/MarcusBarnes/mik/wiki/Installation).

## Usage

Typical workflow is to 1) configure your toolchain (defined below) by creating an .ini file, 2) check your configuration options and then 3) run MIK to perform the conversion of your source content into Islandora ingest packages. When MIK finishes running, you can import your content into Islandora using [Islandora Batch](https://github.com/Islandora/islandora_batch), [Islandora Newspaper Batch](https://github.com/discoverygarden/islandora_newspaper_batch), [Islandora Book Batch](https://github.com/Islandora/islandora_book_batch), or [Islandora Compound Batch](https://github.com/MarcusBarnes/islandora_compound_batch).

### 1. Configure your toolchain

In a nutshell, this means create an .ini file for MIK. Details for available toolchaines are provided on the [wiki](https://github.com/MarcusBarnes/mik/wiki/Toolchains).

### 2. Check your configuration

To check your configuration options, run MIK and include the `--checkconfig` (or `-cc`) option with a value 'all':

```./mik --config foo.ini --checkconfig all```

You can also check specific types of configuration values as described in this [Cookbook entry](https://github.com/MarcusBarnes/mik/wiki/Cookbook:-Check-your-MIK-configuration-values).

### 3. Convert your source content into Islandora ingest packages

Once you have checked your configuration options, you can run MIK to perform the data conversion:

```./mik --config foo.ini```

On Windows, you'll need to run:

```php mik --config foo.ini```

The `--config` option is required, but you can also add a `--limit` option if you only want to create a specific number of import packages. This option is useful for testing. For example:

```./mik --config foo.ini --limit 10```

Once MIK starts running, it will display its progress:

```
./mik --config foo.ini
Creating 10 Islandora ingest packages. Please be patient.
===================================================>                          56%
```

and when finished will tell you where your ingest packages have been saved and where your log file is.

### 4. Load your content into Islandora

And you're done. In practice, you probably want to do some quality assurance on the Islandora ingest packages before you import them (and MIK provides some helper scripts to do that). If you're not happy with what MIK produced, you can always modify your configuration settings or your metadata mappings file and run MIK again.

## Current status

Until the end of 2016, we will be working on what we call the 0.9 release of MIK. We aim for a 1.0 release of MIK in January 2017. Please note that the only differences between version 0.9 and 1.0 will be the addition of more features, automated tests, and code cleanup. Version 0.9 is already being used in production.

So far, we have "toolchains" (complete sets of MIK fetchers, metadata parsers, file getters, etc.) for creating Islandora import packages from the following:

* CONTENTdm
  * single-file objects (images, audio, etc.)
  * multi-file PDFs
  * books
  * newspapers
  * non-book and non-newspaper compound objects
* CSV
  * metadata and content files from a local filesystem for single-file objects (images, audio, etc.)
  * metadata and content files from a local filesystem for compound objects
  * metadata and content files from a local filesystem for books
  * metadata and content files from a local filesystem for newspaper issues
* OAI-PMH
  * metadata and one PDF per article from an Open Journal Systems journal
  * metadata and one file per resource described in each OAI-PMH record if the record includes the URL to the file

## Roadmap

* Version 1.0 (January 2017)
  * developer documentation
  * code cleanup
  * more tests

## Development

We welcome community development partners. Some features that would be really great to see include:

* a graphical user interface on top of MIK
* tools for creating mappings files
* toolchains to migrate from DSpace and other repository platforms to Islandora (the OAI-PMH toolchain may already cover DSpace - testers welcome)
* a toolchain to generate Hydra import packages (yes, it's called Move to Islandora Kit but it's flexible enough to create other types of ingest packages and we'd love to collaborate with some Hydra friends)
  * we have a sample CsvToJson toolchain that demonstrates that it's possible to write out packages that differ from those Islandora uses

MIK is designed to be extensible. If you have an idea for a useful manipulator or post-write hook script, please let us know.

README_DEV.md contains some information on coding standards, etc.

