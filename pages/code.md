---
layout: page
title: Code
author: Johannes C Hellmuth
description: Code examples and tutorials focusing on epigenetics, CRISPR and transcription factor biology. Most code is in R / Rstudio. Central code examples and walk-throughs of my published papers will be posted here.
permalink: /code/
---

### Posts

|<img alt="3C arc plot" style="width: 400px" src="/images/2020-01-03-3C-arc-plot-1.png">|[Plotting 3C data as arcs - a ggplot solution]({% link news/_posts/2020-01-03-3C-arc-plot.md %})<br><br>3C data measuring the interaction frequency between two genomic coordinates can be represented graphically as arcs between the anchor (aka viewpoint) and the region of interest. Here's how to do this with ggplot.|
|<img alt="High-throughput FACS assays with R" style="width: 400px" src="/images/2020-01-07-Live-gate-facet-thumbnail.png">|[Analyzing high-throughput FACS assays with R]({% link news/_posts/2020-01-07-FACS-analysis-with-R.md %})<br><br>Traditional flow cytometry analysis software performs badly with large datasets. Here's an example analysis workflow for a simple FACS-based assay run in 96-well format.|


### Code and resources from published papers
<br/>
[An OCT2 / OCA-B / MEF2B Ternary Complex Controls the Activity and Architecture of an Essential Locus Control Region for Normal and Malignant Germinal Center B-Cells.](https://ashpublications.org/blood/article/134/Supplement_1/24/427812/An-OCT2-OCA-B-MEF2B-Ternary-Complex-Controls-the)  
Resources:
A UCSC track hub to fully explore all relevant data at the BCL6 locus is available [here](https://melnicklab.s3.amazonaws.com/jch2004/BCL6-LCR_pub-tracks/hub.txt) 
Code:
* In silico design for a maximum density CRISPR screening library  - [script 1 (under construction)]
* Analysis of CRISPR screening results  - [script 2 (under construction)]
* R-based FACS analysis for drop-out experiments  - [script 3 (under construction)] 
* Transcrition factor motif analysis  
  * Motif download, processing and matching to a region of interest (BCL6 locus) - [script 4.1]({% link html/3.1_TF-motif-matching.html %})
  * Motif density comparison in essential vs non-essential enhancers - [script 4.2]({% link html/3.2_TF-motif-density.html %})
