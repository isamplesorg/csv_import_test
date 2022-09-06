# iSamples Data Import using Flat Files and a GitHub Repository

## iSamples Flat File Data Format

The iSamples flat file data format is backed by a [frictionlessdata.io](https://frictionlessdata.io) schema.  The schema is human-readable and is located in the [iSamples GitHub](https://github.com/isamplesorg/isamples_inabox/blob/develop/isb_lib/data_import/isamples_simple_schema.json).  From an end-user perspective, the workflow for adding records to iSamples is quite simple:

1. Clone the template repository from the [iSamples GitHub](https://github.com/isamplesorg/csv_import_test).
2. Add rows to the `simple_isamples.csv` file. (Note that if you are using one of the other file formats, you'll commit that file instead).
3. Push the changes to GitHub.  As part of this push, a workflow is triggered that will generate a harvestable sitemap from the repository.
4. Once the github workflow is complete, you can look at the [gh-pages](https://github.com/isamplesorg/csv_import_test/tree/gh-pages) branch of the repository to see the sitemap.

## Supported file formats

Since we are built on top of frictionlessdata, technically we support any file format that they support.  However, the formats that we have tested in the iSamples repository are:

- csv
- tsv
- xlsx
- xls

It ought to be a relatively trivial process to add new formats, so just ask if the one you want isn't already done.

## Implementation Details

The template repository has a GitHub workflow that:

1. Checks out the [iSamples in a Box repository](https://github.com/isamplesorg/isamples_inabox)
2. Installs the python environment
3. Runs the validate command on `isb_things.py`
4. If the file format passes validation, runs the sitemap command on `isb_things.py`, and publishes output to a new `sitemaps` directory.
5. Takes that `sitemaps` directory and publishes that to the `gh-pages` branch on the target repository.

## Validation Errors

If a file is pushed that doesn't pass validation, the errors will be available on github for inspection.  The workflow is aborted and the sitemap isn't generated.  The errors will have a code and specify a line number, for example:

```
code             message
16
---------------  ---------------------------------------------------------------------------------------------------------------------
17
missing-label    There is a missing label in the header's field "label" at position "2"
```