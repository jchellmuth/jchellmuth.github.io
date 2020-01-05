
---
title: Posting Rmarkdowns to Jekyll
---

## Introduction
This website is hosted by [Github Pages](https://help.github.com/en/github/working-with-github-pages)

To go from Rmarkdown to Jekyll post:
1. Knit the Rmarkdown to markdown (github variant)
 It is important to preserve

1. Knit the Rmarkdown to markdown (github variant)
2. Include the YAML header in knit so it will be present in github markdown (header, author, etc.)

Usually, my Rmakdown will be in the `_drafts` folder. It is important to knit straight to the `_posts` folder so that the links to image files produce when knitting are correct. 
To knit to the `_posts` folder, use the `rmakdown::render` command as shown below (there is no way to do this with the Rstudio 'knit' button):

```{r}
rmarkdown::render("2020-01-03-3C-arc-plot.Rmd",output_dir = "../_posts")
```