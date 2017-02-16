# pdftabextract - A set of tools for data mining (OCR-processed) PDFs

July 2016 / Feb. 2017, Markus Konrad <markus.konrad@wzb.eu> / [Berlin Social Science Center](https://www.wzb.eu/en)

## Introduction

This repository contains a set of tools written in Python 3 with the aim to extract tabular data from (OCR-processed)
PDF files. Before these files can be processed they need to be converted to XML files in
[pdf2xml format](http://www.mobipocket.com/dev/pdf2xml/). This is very simple -- see section below for instructions.

### Module overview

After that you can view the extracted text boxes with the
[pdf2xml-viewer](https://github.com/WZBSocialScienceCenter/pdf2xml-viewer) tool if you like. The pdf2xml format can be loaded and parsed with functions in the `common` submodule. Lines can be detected in the scanned images using the `imgproc` module. If the pages are skewed or rotated, this can be detected and fixed with methods from `imgproc` and functions in `textboxes`. Lines or text box positions can be clustered in order to detect table columns and rows using the `clustering` module. When columns and rows were successfully detected, they can be converted to a *page grid* with the `extract` module and their contents can be extracted using `fit_texts_into_grid` in the same module. `extract` also allows you to export the data as [pandas](http://pandas.pydata.org/) DataFrame.

If your scanned pages are double pages, you will need to pre-process them with `splitpages`.

### Examples and tutorials

An extensive tutorial was [posted here](https://datascience.blog.wzb.eu/2017/02/16/data-mining-ocr-pdfs-using-pdftabextract-to-liberate-tabular-data-from-scanned-documents/) and is derived from the [Jupyter Notebook](https://github.com/WZBSocialScienceCenter/pdftabextract/blob/master/examples/catalogue_30s/catalog_30s_notebook.ipynb) contained in the examples. There are more use-cases and demonstrations in the [examples directory](https://github.com/WZBSocialScienceCenter/pdftabextract/blob/master/examples/).


## Features

* load and parse files in pdf2xml format (`common` module)
* split scanned double pages (`splitpages` module)
* detect lines in scanned pages via image processing (`imgproc` module)
* detect page rotation or skew and fix it (`imgproc` and `textboxes` module)
* detect clusters in detected lines or text box positions in order to find column and row positions (`clustering` module)
* extract tabular data and convert it to pandas DataFrame (which allows export to CSV, Excel, etc.) (`extract` module)

## Installation

This package is available on [PyPI](https://pypi.python.org/pypi/pdftabextract/) and can be installed via pip: `pip install pdftabextract`

## Requirements

The requirements are listed in `requirements.txt` and are installed automatically if you use pip.

**Only Python 3 -- No Python 2 support.**

## Converting PDF files to XML files with pdf2xml format

You need to convert your PDFs using the **poppler-utils**, a package which is part of most Linux distributions
and is also available for OSX via Homebrew or MacPorts. From this package we need the command `pdftohtml` and can create
an XML file in pdf2xml format in the following way using the Terminal:

```
pdftohtml -c -i -hidden -xml input.pdf output.xml
```

The arguments *input.pdf* and *output.xml* are your input PDF file and the created XML file in pdf2xml format
respectively. It is important that you specifiy the *-hidden* parameter when you're dealing with OCR-processed
("sandwich") PDFs. You can furthermore add the parameters *-f n* and *-l n* to set only a range of pages to be
converted.

## Usage and examples

For usage and background information, please read my series of blog posts about
[data mining PDFs](https://datascience.blog.wzb.eu/category/pdfs/).


See the following images of the example input/output:

Original page
![original page](https://datascience.blog.wzb.eu/wp-content/uploads/10/2017/02/ALA1934_RR-excerpt.pdf-3_1.png)

Generated (and skewed) pdf2xml file viewed with [pdf2xml-viewer](https://github.com/WZBSocialScienceCenter/pdf2xml-viewer)
![OCR PDF example in the viewer](https://datascience.blog.wzb.eu/wp-content/uploads/10/2017/02/pdf2xml-viewer-page.png)

Detected lines
![Detected lines](https://datascience.blog.wzb.eu/wp-content/uploads/10/2017/02/ALA1934_RR-excerpt.pdf-3_1-lines-orig.png)

Detected clusters of vertical lines (columns)
![Detected clusters of vertical lines (columns)](https://datascience.blog.wzb.eu/wp-content/uploads/10/2017/02/ALA1934_RR-excerpt.pdf-3_1-vertical-clusters.png)

Generated page grid viewed in pdf2xml-viewer
![Generated page grid viewed in pdf2xml-viewer](https://datascience.blog.wzb.eu/wp-content/uploads/10/2017/02/pdf2xml-viewer-pagegrid.png)

Excerpt of the extracted data
![Excerpt of the extracted data](http://datascience.blog.wzb.eu/wp-content/uploads/10/2017/02/pdftabextract-example-extracted-data.png)

## License

Apache License 2.0. See LICENSE file.
