---
title: 'Create a rmarkdown template'
author: "Chun Su"
date: "2021-10-26"
categories: ["r"]
output: 
  blogdown::html_page:
          toc: false
---

Inspired by David Robinson's tidytuesday screencast, I start my own tidytuesday practice. Noticing some structures of `.Rmd` were frequently used in all the practice, I decided to create my own Rmd template specifically for tidytuesday practice. 

With reference to [rstudio4edu-book chapter 13](https://rstudio4edu.github.io/rstudio4edu-book/rmd-templates.html), here are the steps I took for my tidytuesday Rmd template

## step 1: build a package infrastructure. 

Here package directory is `/Volumes/Chug/Project/rmdTemplates`

```R
usethis::create_package("/Volumes/Chug/Project/rmdTemplates")
```
If `/Volumes/Chug/Project/rmdTemplates` already exist with `.Rproj`, the console output may ask if you’d like to overwrite the pre-existing R project. Select No.

## step 2: create inst/ subdirectory

```R
usethis::use_rmarkdown_template(template_name = "tidytuesday")
```
This will message out
```
✓ Setting active project to '/Volumes/Chug/Project/rmdTemplates'
✓ Creating 'inst/rmarkdown/templates/tidytuesday/skeleton/'
✓ Writing 'inst/rmarkdown/templates/tidytuesday/template.yaml'
✓ Writing 'inst/rmarkdown/templates/tidytuesday/skeleton/skeleton.Rmd'
```

## step 3: Edit template and install template
Edit template `inst/rmarkdown/templates/tidytuesday/skeleton/skeleton.Rmd` as below

- yaml header 
  - output format as github_document
  - using params to allow dynamic date for Tidy Tuesday data load
- set plot theme
- use date information to create link to readme and insert in md text by `r readme_link`
- load frequently used packages
- two major contents: EDA and ML

The complete template document can be found [here](https://github.com/sckinta/example_code/blob/master/code_examples/skeleton.Rmd)

After editing, Save skeleton.Rmd, then menu `Build` > `Install and Restart`. 

Make sure that there is only skeleton.Rmd in skeleton folder, otherwise, it will create folders instead of generating single .Rmd file while select R markdown file.

## step 4: Test template

Once your R session has been restarted, navigate to `File` > `New File` > `R Markdown`. Select From Template from the dialogue box menu.

![](/img/rmdtemplate_1.png)

