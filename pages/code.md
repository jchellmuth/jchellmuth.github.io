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
[**Unique Immune Cell Coactivators Specify Locus Control Region Function and Cell Stage**](https://www.cell.com/molecular-cell/fulltext/S1097-2765(20)30743-7)  
Chu and Hellmuth et al.  
Mol Cell. 2020. PubMed PMID: 33232656.  
[View or download PDF here]({% link pdfs/Chu and Hellmuth Mol Cell.pdf %})  

  
Resources:  
* A UCSC track hub to fully explore all relevant data at the BCL6 locus is available [here](http://genome.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr3:187710000-188270000&hubUrl=https://raw.githubusercontent.com/jchellmuth/BCL6.track.hub/master/hub.txt)
* A version of this track hub with highlights for each constituent enhancer is available [here](http://genome.ucsc.edu/cgi-bin/hgTracks?db=hg38&position=chr3:187710000-188270000&hubUrl=https://raw.githubusercontent.com/jchellmuth/BCL6.track.hub/master/hub.txt&highlight=hg38.chr3%3A187900071-187903310%230064C8%7Chg38.chr3%3A187918231-187920230%23808080%7Chg38.chr3%3A187934971-187936970%23808080%7Chg38.chr3%3A187941871-187947250%23808080%7Chg38.chr3%3A187957831-187959830%230064C8%7Chg38.chr3%3A187962431-187964430%23808080%7Chg38.chr3%3A187968751-187970750%230064C8%7Chg38.chr3%3A187973731-187975950%23808080%7Chg38.chr3%3A187977811-187980750%230064C8%7Chg38.chr3%3A187981291-187983290%23808080%7Chg38.chr3%3A187743727-187745726%23228B22)  
  
Code:  
* In silico design for a maximum density CRISPR screening library  - [script 1.1]({% link html/1.1_Library-design-LCR.html %})  
* Analysis of CRISPRi screening results
  *  Screening data normalization and QC [script 2.1]({% link html/2.1_CRISPRi-analysis.html %})  
  *  Identification of essential enhancers [script 2.2]({% link html/2.2_enhancer_definition.html %})  
* R-based FACS analysis for drop-out experiments
* Transcrition factor motif analysis  
  * Motif download, processing and matching to a region of interest (BCL6 locus) - [script 4.1]({% link html/3.1_TF-motif-matching.html %})
  * Motif density comparison in essential vs non-essential enhancers - [script 4.2]({% link html/3.2_TF-motif-density.html %})
