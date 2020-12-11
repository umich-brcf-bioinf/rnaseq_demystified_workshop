---
title: "Alignment and Gene Quantification"
author: "UM Bioinformatics Core"
date: "2020-12-11"
output:
    html_document:
        theme: readable
        toc: true
        toc_depth: 4
        toc_float: true
        number_sections: true
        fig_caption: true
        keep_md: true
---

<!---
library(rmarkdown)
render('Module4b_Alignment.Rmd', output_dir = 'site')
--->

<!--- Allow the page to be wider --->
<style>
    body .main-container {
        max-width: 1200px;
    }
</style>

> # Objectives
> *
> *
> *
> *
> *

# Differential Expression Workflow

In this lesson we will discuss the alignment and gene quantification steps that are necessary prior to doing a test for differential expression, the topic of Day 2.

| Step | Task |
| :--: | ---- |
| 1 | Experimental Design |
| 2 | Biological Samples / Library Preparation |
| 3 | Sequence Reads |
| 4 | Assess Quality of Raw Reads |
| **5** | **Splice-aware Mapping to Genome** |
| **6** | **Count reads associated with genes** |
| 7 | Test for DE Genes |

# Notes on STAR

- Algorithmic details to mention (or not):
    - MMP, maximal mappable prefix
    - Suffix arrays
- Fig 1 of [STAR](https://academic.oup.com/bioinformatics/article/29/1/15/272537) shows how the algorithm naturally allows for mismatches or even for polyA tails / adapter sequence.

# Notes on RSEM

- RSEM aims to deal with the problem of reads mapping to multiple genes / isoforms.
- No reference required, just a set of transcripts (GTF?).
- Requirements of alignments from aligner:
    - First, and most critically, aligners must be configured to report all valid alignments of a read, and not just a single "best" alignment.
    - Second, we recommend that aligners be configured so that only matches and mismatches within a short prefix (a "seed") of each read be considered when determining valid alignments.
        - The idea is to allow RSEM to decide which alignments are most likely to be correct, rather than giving the aligner this responsibility.
- RSEM has non-integer values because of the duplicative nature of genes/isoforms

> "STAR's default parameters are optimized for mammalian genomes."

> Although there is general agreement between the mappings and the gene quantifications produced by different RNA-seq pipelines, quantifications of individual transcript isoforms, being much more complex, can differ substantially depending on the processing pipeline employed and are of unknown accuracy. Therefore, alignments and gene quantifications can be used confidently, while transcript quantifications should be used with care.
