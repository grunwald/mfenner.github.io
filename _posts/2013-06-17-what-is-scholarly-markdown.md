--- 
layout: post 
title: "What is Scholarly Markdown?" 
description: "" 
category:
tags: [markdown, meeting] 
---

One of the important discussions taking place at the [Markdown for Science][1] workshop last weekend was about the definition of Scholarly Markdown. We came up with [this][2]:

1. Markdown that supports the requirements of scientific texts
2. Markdown as format that glues open scientific text resources together
3. A reference implementation with documentation and tests
4. A community

We also agreed that **Scholarly Markdown** is a better term than **Markdown for Science**, as it also includes the Social Sciences and Humanities. And we agreed on a hashtag, [#scholmd][3].

I like #3 and #4, and I was not surprised to see #1. #2 is one of the most important outcomes of the workshop for me personally, and was reflected in the discussion we had in the breakout session on *What is needed for Markdown to be adopted by the scientific community?* One important strategy is the following:

> We need an online tool that makes it easy for scholars to write scholarly markdown in a collaborative manner.

We called this the **Google Docs for Scientists** (Google Docs is a good collaborative tool, but is lacking some important features required for scientific documents, e.g. integrated citation management). [Authorea][4] was mentioned as a promising example of this concept. It was also noted that some previous efforts failed, because the tool looked too different from Microsoft Word. But building such a tool wouldn't really require markdown as a file format, and could for example also be done directly in HTML5. This would be a reasonable strategy, but in my mind is falling short because I think the problem we need to solve is more complicated than making collaboration easier with tools that look like Microsoft Word. We therefore also discussed a different strategy:

> We need multiple tools that make it easy for scholars to create scholarly markdown documents and openly share them. This collaborative work is not limited to authoring scholarly papers, but also includes shorter scholarly texts, e.g. experimental results, lab notebooks, lecture notes, blog posts and working papers. Ease of use is not only defined by the writing experience, but also how easy it is to share documents with others.

This definition almost sounds like a definition for Open Science, and assumes that data - and increasingly software - are an integral part of reporting science. This makes [reproducibility][5] of scientific results much easier, and one nice example how this can be done is the integration of markdown into the R statistical software, using the [knitr package][6]. Using the [May 2013 PLOS article-level metrics data][7] which are freely available for download, the R code below can be embedded into a markdown file and will produce the bar plot below when the markdown file is run in R (to try this yourself, download the ALM data and [markdown file for this article][8]).

```r
# Load required libraries
library(reshape2)

# Options
report_date <- "2013-05-20"

# Load the data from the bulk download, filter out DOIs that are not from PLoS journals
alm <- read.csv("data-alm/alm_report_2013-05-20.csv", encoding = "UTF8", sep = ",", stringsAsFactors=FALSE, na.strings=c("0"))
alm <- subset(alm, (substr(alm$doi,1,15) == "10.1371/journal"))
alm$publication_date <- as.Date(alm$publication_date)
alm$counter_html <- as.numeric(alm$counter_html)

# Options
plos.start_date <- NA
plos.end_date <- NA
plos.colors <- c("#304345","#304345","#789aa1","#789aa1","#789aa1","#ad9a27","#ad9a27","#ad9a27","#ad9a27","#ad9a27","#ad9a27","#ad9a27")
plos.title <- c("Proportion of articles covered by source")
plos.description <- c("Article-level metrics for all 80,602 PLOS journal articles published until May 20, 2013.")

# Aggregate notes and comments
alm$comments <- as.numeric(alm$comments)

# Aggregate Mendeley
alm$mendeley <- rowSums(subset(alm, select=c("mendeley_readers","mendeley_groups")), na.rm=TRUE)
alm$mendeley[alm$mendeley == 0] <- NA

# Use subset of columns
alm <- subset(alm, select=c("counter_html","pmc_html","crossref","scopus","pubmed","mendeley","citeulike","comments","researchblogging","facebook","twitter","wikipedia"))

# Calculate percentage of values that are not missing (i.e. have a count of at least 1)
colSums <- colSums(!is.na(alm)) * 100 / length(alm$counter_html)
exactSums <- sum(as.numeric(alm$pmc_html),na.rm =TRUE)

# Plot the chart. 
opar <- par(mar=c(1,7,2,1)+0.1,omi=c(1,0.3,1,1))
plos.names <- c("PLoS HTML Views", "PMC HTML Views","CrossRef","Scopus","PubMed Citations", "Mendeley","CiteULike","PLoS Comments","Research Blogging","Facebook","Twitter","Wikipedia")
y <- barplot(colSums,horiz=TRUE,col=plos.colors, border = NA, xlab=plos.names, xlim=c(0,120), axes=FALSE, names.arg=plos.names,las=1, adj=0)
text(colSums+6,y,labels=sprintf("%1.0f%%", colSums))
title(main=plos.title, cex.main=2, outer=TRUE, line=1, adj=0)
mtext(paste(strwrap(plos.description,width=90), collapse="\n"), side=3, outer=TRUE, line=-.5, adj=0)
```
 
![**Fig. 1**. Proportion of articles covered by source][9]

In a way this approach to scholarly markdown is much more difficult than building a nice online collaborative writing tool. But for me scholarly markdown is not about competing with Microsoft Word, it is about building something new that scholars want to use because it allows them to do something that is impossible with the existing tools. For the same reason my todo item at the end of the workshop was *think about document type where markdown shines*. The R example above is a great example where markdown shines. If you can think of additional examples, please add them to the comments.

[1]: <https://github.com/scholmd/scholmd/wiki>

[2]: <https://github.com/scholmd/scholmd/wiki/What-is-Markdown>

[3]: <https://twitter.com/search?q=%23scholmd&src=typd>

[4]: <https://www.authorea.com>

[5]: <https://www.scienceexchange.com/reproducibility>

[6]: <http://yihui.name/knitr/>

[7]: <http://article-level-metrics.plos.org/plos-alm-data/>

[8]: <https://github.com/articlemetrics/plosOpenR/blob/master/barPlotSummary.Rmd>

[9]: </images/barplot-2013-06-17.png>
