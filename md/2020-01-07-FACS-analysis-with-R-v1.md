---
layout: post
title: Analyzing high-throughput FACS assays with R (v1)
author: Johannes C Hellmuth
categories: code ggplot visualization tutorial FACS
permalink: /draft/FACS-with-R-v1/
output:
  md_document:
    variant: markdown_github
    preserve_yaml: true
---
**Note:** a new version of this tutorial is available [here.]({% link news/_posts/2020-01-07-FACS-analysis-with-R.md %})

Introduction
------------

Analysis of flow cytometry data with R may seem daunting at first but I
highly recommend it to anyone performing mid- or high-throghput
FACS-based assays. I frequently run experiments in 96-well formats with
hundreds of samples (this obviously requires a plate reader on your FACS
machine). Even if you only look at very few markers, traditional flow
analysis software like FlowJo doesn’t handle large numbers of samples
well. It’s slow, prone to crashing and exporting large plots is painful.
This is where flow cytometry analysis in R does a great job. There are
multiple R packages for flow cytometry data analysis. The two packages I
am using in this tutorial and which I highly recommend are:  

- [flowCore](https://bioconductor.org/packages/release/bioc/html/flowCore.html)  
- [ggcyto](https://bioconductor.org/packages/release/bioc/html/ggcyto.html)

Both have great documentation and I suggest you work through their
vignettes to get you started with flow analysis in R. The following
tutorial assumes that you have some knowledge of flow cytometry data
analysis (such as basic gating operations) and familiarized yourself
with the basics of flowCore datastructures and functions.

In the example analysis below, I am looking at the fraction of GFP
positive cells in a 96-well plate. Each well has been infected with a
different GFP-tagged gRNA. In the actual experiment, I then look at the
fraction of GFP positive cells over time to see which gRNAs drop out.
For now, we’ll just be looking at one timepoint / dataset.  
Because I am only looking at one marker, no compensation is required.
Note also, that I am not transforming the data (as suggested by the
flowCore vignette). Instead, I use the scale function of ggcyto
(e.g. scale\_x\_flowJo\_biexp) which I find much more convenient.

If you’d like to follow along, you can download the example data
[here.](https://jchellmuth.github.io/downloads/2020-07-08-FACS-data.zip)

Load required libraries
-----------------------

``` r
library(flowCore)
library(ggcyto)
library(tidyverse)
```

Read and format .fcs data
-------------------------

The `read.flowSet` allows you to read many `.fcs` at once. The resultant
flowSet object will store the data from all these `.fcs` files.

``` r
fs <- read.flowSet(path = "../../downloads/2020-07-08-FACS-data//",pattern = ".fcs",alter.names = T)
```

Sample information (aka phenotypic data) such as sample names can be
accessed with the `pData` function. The default sample names are not
very useful. So, let’s extract the well ID from the sample name (this
will make things easier later on).

``` r
pData(fs) %>% head(3)
```

    ##                                                      name
    ## Specimen_001_B10_B10_009.fcs Specimen_001_B10_B10_009.fcs
    ## Specimen_001_B11_B11_010.fcs Specimen_001_B11_B11_010.fcs
    ## Specimen_001_B2_B02_001.fcs   Specimen_001_B2_B02_001.fcs

``` r
pData(fs)$well <- gsub(".*_.*_(.*)_.*.fcs","\\1",sampleNames(fs)) # extract well from name and add new 'well' column
pData(fs) %>% head(3)
```

    ##                                                      name well
    ## Specimen_001_B10_B10_009.fcs Specimen_001_B10_B10_009.fcs  B10
    ## Specimen_001_B11_B11_010.fcs Specimen_001_B11_B11_010.fcs  B11
    ## Specimen_001_B2_B02_001.fcs   Specimen_001_B2_B02_001.fcs  B02

The default channel names on most FACS machines are useless and changing
them there is often tedious. On our BD FACS Canto II, for example, the
\[405\|450/50\] channel is named “FITC.A” and the \[488\|530/30\] is
named “Pacific.Blue.A” - irrespective of the fluorphore you are actually
reading. Below is a quick and simple way to change the channel names for
the entire flowSet. This makes coding easier and will result in proper
axis labels in your plots.

``` r
colnames(fs)
```

    ## [1] "FSC.A"          "FSC.H"          "FSC.W"          "SSC.A"         
    ## [5] "SSC.H"          "SSC.W"          "FITC.A"         "Pacific.Blue.A"
    ## [9] "Time"

``` r
colnames(fs)[colnames(fs)=="FITC.A"] <- "GFP"
colnames(fs)[colnames(fs)=="Pacific.Blue.A"] <- "BFP"
```

To be able to add gates, the flowSet has to be transformed to a
GatingSet object with the `GatingSet` function

``` r
gs <- GatingSet(fs)
```

Set singlet gate
----------------

There are automatic singlet gating commands available, e.g.
`flowStats::gate_singlet`. However, I generally find manual gating more
reliable. As with all gating steps below, setting the gates can be
tedious in R as opposed to interactive gating with FlowJo. However, if
you usually use similar cells or cell lines and keep the voltage
settings on your machine constant, you really only have to set these
gates once. After that, all you may have to do is tweak the gates a
little. The basic steps I take for each of the gatings below are:  
1. define the gate  
2. check the gate in a plot (before adding it to the gating set)  
3. if the gate seems right, add / apply it to the gating set  
4. recompute the gatingSet to have statistics available

``` r
g.singlets <- polygonGate(filterId = "Singlets","FSC.A"=c(2e4,25e4,25e4,2e4),"FSC.H"=c(0e4,12e4,18e4,6e4)) # define gate
ggcyto(gs[[1]],aes(x=FSC.A,y=FSC.H),subset="root")+geom_hex(bins = 200)+geom_gate(g.singlets)+ggcyto_par_set(limits = "instrument") # check gate
```

![](/images/2020-01-07-singlets-gate-1.png)

``` r
add(gs,g.singlets) # add gate to GatingSet
```

    ## [1] 2

``` r
recompute(gs) # recompute GatingSet
```

If you want to check where the singlets you filtered out show up in FSC
vs SSC (the plot that is typically used to remove debris or gate live
cells), you can do this by setting different subets in the ggcyto
command (‘root’ or ‘Singlets’).

``` r
ggcyto(gs[[1]],aes(x=FSC.A,y=SSC.A),subset="root")+geom_hex(bins = 200)+ggcyto_par_set(limits = "instrument")
```

![](/images/2020-01-07-singlets-gate-post-check-1.png)

``` r
ggcyto(gs[[1]],aes(x=FSC.A,y=SSC.A),subset="Singlets")+geom_hex(bins = 200)+ggcyto_par_set(limits = "instrument")
```

![](/images/2020-01-07-singlets-gate-post-check-2.png)

Now comes the fun part where R works so much better than FlowJo:
plotting the data for all samples at once. This is based on ggplot’s
`facet_wrap` command (included by default in `ggcyto` when using even if
you don’t explicitly call that command). If we now use the ‘well’ column
with `facet_wrap`, we’ll get meaningful titles for each plot. Although
you might not use this overview plot for any kind of publication
(espcially not something boring like your singlet gate), this kind of
plot allows you to immediately spot any issues with certain samples
(dead cells, clumped cells, technical errors with your FACS machine).
Thus, I recommend generating these overview plots at each gating step.

``` r
ggcyto(gs,aes(x=FSC.A,y=FSC.H),subset="root")+geom_hex(bins = 100)+geom_gate("Singlets")+
  geom_stats(adjust = 0.8)+ggcyto_par_set(limits = "instrument")+
  facet_wrap(~well,ncol = 10)
```

![](/images/2020-01-07-singlets-gate-facet-1.png)

Set live gate
-------------

Gating steps are as above. Differentiating live and dead cells solely
based on FSC/SSC works very well for the cells used here (OCI-LY7) but
may not work well for all kinds of cells.

``` r
g.live <- polygonGate(filterId = "Live","FSC.A"=c(5e4,24e4,24e4,8e4),"SSC.A"=c(0,2e4,15e4,5e4)) # define gate
ggcyto(gs[[1]],aes(x=FSC.A,y=SSC.A),subset="Singlets")+geom_hex(bins = 200)+geom_gate(g.live)+ggcyto_par_set(limits = "instrument") # check gate
```

![](/images/2020-01-07-live-gate-1.png)

``` r
add(gs,g.live,parent="Singlets") # add gate to GatingSet
```

    ## [1] 3

``` r
recompute(gs) # recompute GatingSet
```

Again, you can plot the live gate for all samples.

``` r
ggcyto(gs,aes(x=FSC.A,y=SSC.A),subset="Singlets")+geom_hex(bins = 100)+geom_gate("Live")+
  geom_stats(adjust = 0.8)+ggcyto_par_set(limits = "instrument")+
  facet_wrap(~well,ncol = 10)
```

![](/images/2020-01-07-live-gate-facet-1.png)

Set GFP+ gate
-------------

Because we are only looking at one channel (GFP) for the next gating
step, we can now switch to density plots instead of the hex plots above.
These are faster to generate, simpler to look at and need less storage.

``` r
g.gfp <- rectangleGate(filterId="GFP positive","GFP"=c(1000, Inf)) # set gate
ggcyto(gs[[1]],aes(x=GFP),subset="Live")+geom_density(fill="forestgreen")+geom_gate(g.gfp)+ggcyto_par_set(limits = "instrument")+scale_x_flowJo_biexp() # check gate
```

![](/images/2020-01-07-GFP-gate-1.png)

``` r
add(gs,g.gfp,parent="Live") # add gate to GatingSet
```

    ## [1] 4

``` r
recompute(gs) # recalculate Gatingset
```

Now, we can look at the data we were looking for: the fraction of GFP
positive cells in each well.

``` r
ggcyto(gs,aes(x=GFP),subset="Live",)+geom_density(fill="forestgreen")+geom_gate("GFP positive")+
  geom_stats(adjust = 0.1,y=0.002,digits = 1)+
  ggcyto_par_set(limits = "instrument")+scale_x_flowJo_biexp()+
  facet_wrap(~well,ncol = 10)
```

![](/images/2020-01-07-GFP-gate-facet-1.png)

Get populations stats for downstream analysis
---------------------------------------------

To get the data from each gating step from the `GatingSet`, we can use
the `getPopStats` function.

``` r
getPopStats(gs) %>% head
```

    ##                            name                  Population         Parent
    ## 1: Specimen_001_B10_B10_009.fcs                   /Singlets           root
    ## 2: Specimen_001_B10_B10_009.fcs              /Singlets/Live      /Singlets
    ## 3: Specimen_001_B10_B10_009.fcs /Singlets/Live/GFP positive /Singlets/Live
    ## 4: Specimen_001_B11_B11_010.fcs                   /Singlets           root
    ## 5: Specimen_001_B11_B11_010.fcs              /Singlets/Live      /Singlets
    ## 6: Specimen_001_B11_B11_010.fcs /Singlets/Live/GFP positive /Singlets/Live
    ##    Count ParentCount
    ## 1:  9211       10000
    ## 2:  6770        9211
    ## 3:  3467        6770
    ## 4:  9176       10000
    ## 5:  7158        9176
    ## 6:  4503        7158

``` r
ps <- data.frame(getPopStats(gs)) # make data.frame with PopStats
```

We can now easily calculate the ‘percent of parent’ (a common metric in
FACS analysis).

``` r
ps$percent_of_parent <- ps$Count/ps$ParentCount
```

If we want to retain any Pheno Data from the initial flowSet, we can use
a simple merge (here, we only have well information).

``` r
psm <- merge(ps,pData(fs),by="name")
```

<br> <br> Please feel free to email me with any questions, comments or
suggestions and I’ll be happy to post them here.  
info at jchellmuth.com
