
rtika
=====

***Extract text and metadata from over a thousand file types.***

[![Travis-CI Build Status](https://travis-ci.org/predict-r/rtika.svg?branch=master)](https://travis-ci.org/predict-r/rtika)

From Wikipedia:

> Apache Tika is a content detection and analysis framework, written in Java, stewarded at the Apache Software Foundation. It detects and extracts metadata and text from over a thousand different file types, and as well as providing a Java library, has server and command-line editions suitable for use from other programming languages ...

> For most of the more common and popular formats, Tika then provides content extraction, metadata extraction and language identification capabilities. (Accessed Jan 18, 2018. See <https://en.wikipedia.org/wiki/Apache_Tika>.)

This R interface includes the Tika software.

Installation
------------

You need at least `Java 7` or `OpenJDK 1.7`. To check, run the command `java -version` from a terminal. Get Java installation tips at <http://openjdk.java.net/install/> or <https://www.java.com/en/download/help/download_options.xml>.

Next, install the `rtika` package from Github.com.

``` r
# devtools simplifies installation from Github.
if(!requireNamespace('devtools')){ install.packages('devtools', repos='https://cloud.r-project.org') }
# Install rtika from github
if(!requireNamespace('rtika')){ devtools::install_github('predict-r/rtika') } 
library('rtika')
```

Extract Plain Text
------------------

Get a text document, such as `.pdf`, `.doc`, `.docx`, `.rtf`, `.ppt`, or a mix. Then, extract the plain text with the `tika()` function. Relax, it will probably work!

``` r
input = 'https://cran.r-project.org/doc/manuals/r-release/R-data.pdf'
text = tika(input) # magic happens
```

The `text` will be a character vector, in the same order as `input`. Display a snippet using `cat`.

``` r
cat(substr(text[1],45,450)) # sub-string of the text
```


    R Data Import/Export
    Version 3.4.3 (2017-11-30)

    R Core Team



    This manual is for R, version 3.4.3 (2017-11-30).

    Copyright c© 2000–2016 R Core Team
    Permission is granted to make and distribute verbatim copies of this manual provided
    the copyright notice and this permission notice are preserved on all copies.

    Permission is granted to copy and distribute modified versions of this manual under
    the cond

Get Metadata
------------

Metadata comes with the `json`,`xml` and `html` output options. A side effect is that Tika retains more document structure, like table cells.

``` r
library('jsonlite')
json = tika(input,'J') # 'J' is a shortcut for 'jsonRecursive'
metadata = fromJSON(json[1])
```

See the structure of the metadata, or meta-metadata 🤯 .

``` r
str(metadata) #data.frame of metadata
```

    'data.frame':   1 obs. of  41 variables:
     $ Content-Length                             : chr "309939"
     $ Content-Type                               : chr "application/pdf"
     $ Creation-Date                              : chr "2017-11-30T13:39:02Z"
     $ Last-Modified                              : chr "2017-11-30T13:39:02Z"
     $ Last-Save-Date                             : chr "2017-11-30T13:39:02Z"
     $ PTEX.Fullbanner                            : chr "This is pdfTeX, Version 3.14159265-2.6-1.40.18 (TeX Live 2017/Debian) kpathsea version 6.2.3"
     $ X-Parsed-By                                :List of 1
      ..$ : chr  "org.apache.tika.parser.DefaultParser" "org.apache.tika.parser.pdf.PDFParser"
     $ X-TIKA:content                             : chr "<html xmlns=\"http://www.w3.org/1999/xhtml\">\n<head>\n<meta name=\"date\" content=\"2017-11-30T13:39:02Z\" />\"| __truncated__
     $ X-TIKA:digest:MD5                          : chr "3f1b649a4ec70aaa4c2dad4eade8b430"
     $ X-TIKA:parse_time_millis                   : chr "1002"
     $ access_permission:assemble_document        : chr "true"
     $ access_permission:can_modify               : chr "true"
     $ access_permission:can_print                : chr "true"
     $ access_permission:can_print_degraded       : chr "true"
     $ access_permission:extract_content          : chr "true"
     $ access_permission:extract_for_accessibility: chr "true"
     $ access_permission:fill_in_form             : chr "true"
     $ access_permission:modify_annotations       : chr "true"
     $ created                                    : chr "Thu Nov 30 05:39:02 PST 2017"
     $ date                                       : chr "2017-11-30T13:39:02Z"
     $ dc:format                                  : chr "application/pdf; version=1.5"
     $ dcterms:created                            : chr "2017-11-30T13:39:02Z"
     $ dcterms:modified                           : chr "2017-11-30T13:39:02Z"
     $ meta:creation-date                         : chr "2017-11-30T13:39:02Z"
     $ meta:save-date                             : chr "2017-11-30T13:39:02Z"
     $ modified                                   : chr "2017-11-30T13:39:02Z"
     $ pdf:PDFVersion                             : chr "1.5"
     $ pdf:docinfo:created                        : chr "2017-11-30T13:39:02Z"
     $ pdf:docinfo:creator_tool                   : chr "TeX"
     $ pdf:docinfo:custom:PTEX.Fullbanner         : chr "This is pdfTeX, Version 3.14159265-2.6-1.40.18 (TeX Live 2017/Debian) kpathsea version 6.2.3"
     $ pdf:docinfo:modified                       : chr "2017-11-30T13:39:02Z"
     $ pdf:docinfo:producer                       : chr "pdfTeX-1.40.18"
     $ pdf:docinfo:trapped                        : chr "False"
     $ pdf:encrypted                              : chr "false"
     $ producer                                   : chr "pdfTeX-1.40.18"
     $ resourceName                               : chr "rtika_file778a177d613c"
     $ tika:file_ext                              : chr ""
     $ tika_batch_fs:relative_path                : chr "tmp/Rtmp3rNd6C/rtika_file778a177d613c"
     $ trapped                                    : chr "False"
     $ xmp:CreatorTool                            : chr "TeX"
     $ xmpTPg:NPages                              : chr "37"

Similar Packages
----------------

In March 2012, I created a repository on `r-forge` called `r-tika` (See: <https://r-forge.r-project.org/projects/r-tika/>) to interface with Apache Tika (See: <https://tika.apache.org/>). While no code was publicly released, my initial code-base used low-level functions from the `rJava` package to interface with the Tika library. I halted development after discovering that the Tika command line interface (CLI) served my purposes.

In September 2017, user *kyusque* released `tikaR`, which uses the `rJava` package to interact with Tika (See: <https://github.com/kyusque/tikaR>). As of writing, it provided a `xml` parser and metadata extraction.

With `rtika`, I chose to interface with the Tika CLI and its 'batch processor' tool. Much of the batch processor is implemented in Tika 1.17 (See: <https://wiki.apache.org/tika/TikaBatchOverview>). The Tika batch processor has good efficiency when processing tens of thousands of documents, is not too slow for a single document, and handles errors gracefully. Further, connecting `R` to the Tika CLI batch processor is relatively easy to maintain, because the `R` code is simple. I anticipate that various researchers will need plain text output, while others want json output. These are implemented in the CLI and hence in `rtika` (although apparently not in `tikaR`). Multiple threads are supported in both the CLI and `rtika`. The `rtika` package anticipates future features with the `args` attribute of the `tika` function, that allows access to the Tika CLI. Another motivation was that `rJava` hwasonce bifficult to get working on Ubuntu and CentOS, especially around wthe time hen Java was not open sourced, although that probably ihas improved
