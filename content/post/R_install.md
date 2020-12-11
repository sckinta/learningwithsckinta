---
title: 'Troubleshoot R package installation'
author: "Chun Su"
date: "2020-12-10"
categories: ["r"]
tags: ["install"]
output: 
  blogdown::html_page:
          toc: false
---

## understand 3 import R install associated system files
There are three important system files for R package installation

- `~/.Renviron`
  - set downloading key as environment variable
  
  ```R
  GGMAP_GOOGLE_API_KEY=fakekey
  LDLINK_TOKEN=fakekey
  ENTREZ_KEY=fakekey
  TWITTER_PAT=~/.rtweet_token.rds
  ```
- `~/.R/Makevars`
  - set FLAGS, which usually relate to some installation error like (:cannot find lib `.so`)
  
  ```R
  LDFLAGS=-L/cm/shared/apps_chop/gsl/2.5/lib -lgsl -lgslcblas
  CFLAGS=-I/cm/shared/apps_chop/gsl/2.5/include
  ```
  - set PKG_LIBS
  
  ```R
  PKG_LIBS=-L/cm/shared/apps_chop/gsl/2.5/lib
  PKG_LIBS =$(LAPACK_LIBS) $(BLAS_LIBS) $(FLIBS)
  ```
  - set others
  
  ```R
  SOURCES = $(wildcard *.cpp) $(wildcard **/*.cpp)
  OBJECTS = $(SOURCES:.cpp=.o)
  ```
  
- `~/.Rprofile`
  - used to set options.
  
  ```R
  options(blogdown.hugo.version = "0.77.0")
  options(blogdown.initial_files.number = 0)
  ```
  
  - used to set repo (very useful for deploying shiny app)
  
  ```R
  local({
  r <- getOption("repos")
  r["CRAN"] <- "https://cran.rstudio.com/"
  r["Bioconductor"] <- "https://bioconductor.org/packages/3.10/bioc/"
  r["BioconductorData"] <- "https://bioconductor.org/packages/3.10/data/annotation/"
  options(repos = r)
  })
  ```

## install R package from source file
It works for package does not require too many dependency

```R
install.packages(path_to_file, repos = NULL, type="source")
```

## Bioconductor
Bioconductor archive R packages in their specific release. You cannot install packages from release "A" under the BioManager of release "B" using default `BiocManager::install()`. 

The Bioconducter release can be found https://www.bioconductor.org/about/release-announcements/

To install R packages from bioconductor in certain version
- make sure your R version is compatible based on [Bioconducter release](https://www.bioconductor.org/about/release-announcements/)
- find your current BiocManager version and bioconductor version

```R
packageVersion("BiocManager")
BiocManager::version()
```
OR

```R
library(BiocManager)
# Bioconductor version 3.9 (BiocManager 1.30.10), ?BiocManager::install for help
# Bioconductor version '3.9' is out-of-date; the current release version '3.12' is available with R version '4.0'; see https://bioconductor.org/install
```
- find which version of R package you want to install.
  - package source file archive is at https://bioconductor.org/packages/3.10/bioc/src/contrib/Archive
  - install repo for `install.package()` is at https://bioconductor.org/packages/3.10/bioc
  
\* *change `3.10` to whatever [bioconductor release](https://www.bioconductor.org/about/release-announcements/) you find your package version is at*  
\* *bioconductor repo is important for shiny app deploy too (see below)*

- install from repo

```R
install.packages(
    "S4Vectors",
    repos = c("https://bioconductor.org/packages/3.10/bioc")
)
```

Some other tips of using `BioManager` can be found in [BiocManager vignettes](https://cran.r-project.org/web/packages/BiocManager/vignettes/BiocManager.html)
