---
title: "Day 2 - Module 8b: DESeq2 Initialization"
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

<!--- Allow the page to be wider --->
<style>
    body .main-container {
        max-width: 1200px;
    }
</style>
> # Objectives
> * Import and process count data
> * Understand possible confounding factors when comparing raw gene counts
> * Understand the impact of batches or additional covariates



# DESeq2 Initialization

To walk through the differential expression (DE) workflow we use in the Core, we'll be starting with a count matrix from a RNA-Seq dataset that is described in [Kenny PJ et al, Cell Rep 2014](http://www.ncbi.nlm.nih.gov/pubmed/25464849). The scientific context for this analysis is that the authors were investigating interactions between genes involved in Fragile X syndrome, a disease where there is aberrant prodcution of the FMRP protein. One gene of interest, MOV10 is a putative RNA helicase that associates with FMRP in the microRNA processing pathway.

For the experiment in Kenny et al, HEK293F cellswere either transfected with a MOV10 transgene, or siRNA to knock down Mov10 expression, or non-specific (irrelevant) siRNA before RNA was isolated and sequenced. This resulted in 3 conditions "Mov10 oe" (over expression), "Mov10 kd" (knock down) and "Irrelevant kd", respectively. We'll look at how many replicates were generated for each condition and what comparisons shouldbe made after we load the data.

> After we walk through the workflow, try to comparing the [RNA-Seq methods reported in the paper](https://ars-els-cdn-com.proxy.lib.umich.edu/content/image/1-s2.0-S2211124714009231-mmc1.pdf) to what we did today. Is any information is missing from the methods that would be needed to reproduce their analysis? Did the different tools used for DE impact in the overall findings?

## Read in count data

Another key assumption for DESeq2 is that the analysis will start with [un-normalized counts](http://bioconductor.org/packages/devel/bioc/vignettes/DESeq2/inst/doc/DESeq2.html#why-un-normalized-counts).

To begin our analysis, we'll read in the **raw** count data, which *TODO: where will files be stored from download?*. This table is similar to what could be generated from the files produced in the alignments that we worked through yesterday.

To read in raw count table we'll use the 'read.table()' function.

```r
data <- read.table("./data/Mov10_full_counts.txt", header = TRUE, row.names = 1)
```

You should now see `data` in the "Environment" tab on the right side of your Rstudio window. Let's take a look at the table.

```r
head(data) 
```

```
##             Mov10_kd_2 Mov10_kd_3 Mov10_oe_1 Mov10_oe_2 Mov10_oe_3 Irrel_kd_1
## 1/2-SBSRNA4         57         41         64         55         38         45
## A1BG                71         40        100         81         41         77
## A1BG-AS1           256        177        220        189        107        213
## A1CF                 0          1          1          0          0          0
## A2LD1              146         81        138        125         52         91
## A2M                 10          9          2          5          2          9
##             Irrel_kd_2 Irrel_kd_3
## 1/2-SBSRNA4         31         39
## A1BG                58         40
## A1BG-AS1           172        126
## A1CF                 0          0
## A2LD1               80         50
## A2M                  8          4
```

```r
#OR str(data)
```

We can see that the column names of the table match the expected experimental samples and the rows names look like gene symbols. In addition, we see whole numbers as values in the table. This is very important as the model used by DESeq2 to test for significance assumes count data, i.e. the number of reads that align to each gene, which must be integer values.

We'll discuss later a few normalizations that can be helpful for us to understand how much a gene is expressed within or between samples, but that **should not** be used as inputs for DESeq2.

If we think back to the 'expected_counts' RSEM output, the values are *not* integers (due to how the alignment tool resolves reads that map to multiple locuses). Since DESeq2 requires whole numbers, if we try to use the RSEM ouputs without rounding the estimated counts to a whole number first, DESeq2 will give us an error.

> **Note**: The package `tximport` is [recommended by the DESeq2 authors](https://support.bioconductor.org/p/90672/) to read in the RSEM expected_counts, as this package allows for the average transcript length per gene to be used in the DE analysis and, as [described by the author](https://support.bioconductor.org/p/88763/), the `tximport-to-DESeqDataSet` constructor function round the non-integer data generated by RSEM to whole numbers.

> **RNA-seq versus Microarray data**: With [higher sensitivity, greater flexiblity, and decreasing cost](https://www.illumina.com/science/technology/next-generation-sequencing/microarray-rna-seq-comparison.html), sequencing has largely replaced microarray assays for measuring gene expression. A key difference between the platforms is that microarrays measure intensities and are therefore *continous* data while the count data from sequencing is *discrete*. A more detailed comparison between microarrays and sequencing technologies/analysis is outlined in [the online materials for Penn State's STAT555 course](https://online.stat.psu.edu/stat555/node/30/).

## Generate Sample Information Table

Our next step will be to describe the samples so that we make the proper comparisons. The first step is to look at the sample names from the count table.

```r
colnames(data)
```

```
## [1] "Mov10_kd_2" "Mov10_kd_3" "Mov10_oe_1" "Mov10_oe_2" "Mov10_oe_3"
## [6] "Irrel_kd_1" "Irrel_kd_2" "Irrel_kd_3"
```

We'll use the sample names as a starting point to fill in our sample information table.

```r
meta <- data.frame("samples"=colnames(data))
meta
```

```
##      samples
## 1 Mov10_kd_2
## 2 Mov10_kd_3
## 3 Mov10_oe_1
## 4 Mov10_oe_2
## 5 Mov10_oe_3
## 6 Irrel_kd_1
## 7 Irrel_kd_2
## 8 Irrel_kd_3
```
Sample names are fairly informative so can use as starting point. For most sequencing data generated from large sequencing centers like the AGC, the sample names will likely be less informative so will need to be matched with sample information from submission or experimental notes.

Next, we'll fill in appropriate labels for the treatment groups - making them simple but informative.

```r
meta$condition <- factor(c(rep(c("Mov.KD"), 2), rep(c("Mov.OE"), 3), rep(c("Irrel"), 3)), levels = c("Irrel","Mov.KD", "Mov.OE")) ## levels = c(Case, Control); Control should be last if only two groups.
```
Notice that we set the levels such that the control group is in the first position. This is important for setting the Control group as the denominator in our comparisons when there are three groups.

In this case, there aren't known [**covariates**](https://methods-sagepub-com.proxy.lib.umich.edu/reference/encyc-of-research-design/n85.xml) to include in the sample table. However, if there are additional attributes of the samples that may impact the DE comparisons, like sex, date of collection, or patient of origin, these should be added as [additional columns](https://support.bioconductor.org/p/75309/) in the sample information table.

Next, we'll reorganize the table so it has the shape we need - sample names as the row names for each condition.

```r
row.names(meta) # check the row names
```

```
## [1] "1" "2" "3" "4" "5" "6" "7" "8"
```

```r
row.names(meta) <- meta$samples # set the row names to be the sample names
meta$samples <- NULL # clear the sample name column since isnt needed
```

Before we proceed, we need to make sure that the sample labels (column names) in the count table match the sample information table (row names), including the order.

```r
all(colnames(data) %in% rownames(meta)) #OR?
```

```
## [1] TRUE
```

```r
all(colnames(data) == rownames(meta))
```

```
## [1] TRUE
```
If the sample labels, then we will see an error and need to correct the labels prior to proceeding. Checking the sample information table is extremely important to ensure that the correct samples are grouped together for comparisons.

## Creating DESeq2 object

Bioconductor software packages often define and use custom classes within R for storing data in a way that better fits expectations around biological data, such as illustrated below from [Huber et al. 2015](https://www.nature.com/articles/nmeth.3252).

![](./images/SummarizedExperiment.jpg)

These custom data structures have pre-specified data slots, which hold specific types/classes of data and therefore can be more easily accessed by functions from the same package.

We'll start by creating the DESeqDataSet and then we can talk more about key parts of this object.

To create the DESeqDataSet we need two things we already have: count matrix and the "meta" sample data table. To complete the DESeqDataSet, we will also need to specify a **design formula**.


```r
## Create DESeq object, line by line
dds <- DESeqDataSetFromMatrix(countData = data,
                              colData = meta,
                              design = ~ condition)
```

The design formula specifies column(s) in the metadata table and how they should be used in the analysis. For our dataset we only have one column we are interested in, that is `condition`. This column has three factor levels, which tells DESeq2 that for each gene we want to evaluate gene expression change with respect to these different levels.

If we look at the `dds` object now, we can see how our data is organized.

```r
str(dds)
```
Right now, there are quite a few "empty" slots that will be filled in when we proceed with the model fitting. Before we do that, let's discuss the design formula in more detail.

## Making model choices

The design formula specified informs many of the DESeq2 functions how to treat the samples in the analysis, specifically which column in the samaple metadata table specifies the experimental design.

Our example only includes a single variable, which is the modulation of Mov10 expression. However, we could imagine situations where there are differences between samples in the same treatment group, such as if each sample is from a different patient or from independently established cell lines. We would describe these additional biological differences as **covariates** , and they can be [added to a design formula](https://support.bioconductor.org/p/98700/).

Let's test out adding a covariate to our meta data table and then create a new DESeq2 object.

```r
meta
```

```
##            condition
## Mov10_kd_2    Mov.KD
## Mov10_kd_3    Mov.KD
## Mov10_oe_1    Mov.OE
## Mov10_oe_2    Mov.OE
## Mov10_oe_3    Mov.OE
## Irrel_kd_1     Irrel
## Irrel_kd_2     Irrel
## Irrel_kd_3     Irrel
```

```r
meta$patient <- factor(c("P1", "P2", "P1", "P2", "P3", "P1", "P2", "P3"), levels = c("P1", "P2", "P3"))
```
Notice how we avoid starting with a number our patient covariate labels since R doesn't like that.


```r
dds_patient <- DESeqDataSetFromMatrix(countData = data,
                              colData = meta,
                              design = ~ patient + condition)
```
Now we have specified for DESeq2 that we want to test for the effect of the condition (the last factor) while controlling for the effect of the patient origin (the first factor).

> *Note*: More complex questions, including determining if a fold-change due to treatment or condition is different across groups, such as patient samples, "interaction terms" can be included in the design formula, such as outlined in [this support thread](https://support.bioconductor.org/p/98628/).

Differences between samples can also be due to technical reasons, such as collection on different days or different sequencing runs. Differences between samples that are not due to biological factors as called **batch effects**. We can include batch effects in our design model in the same way as covariates, as long as the technical groups do not overlap, or **confound**, the biological treatment groups.

Let's try add some additional meta-data information where we have counfounding batch effects and create another DESeq2 object.

```r
meta
meta$batch <- factor(c(rep(c("Day1"), 2), rep(c("Day2"), 3), rep(c("Day3"), 3)), levels = c("Day1", "Day2", "Day3"))

dds_batch <- DESeqDataSetFromMatrix(countData = data,
                              colData = meta,
                              design = ~ batch + condition)
```
Notice that the error indicates that variables in the design formula "are linear combinations" which means that batch and condition are correlated and the function is unable to fill in a required 'slot' in the DESeq2 object. So if batches are not balanced by including both case and controls then we cannot attempt to control for those technical effects through statistical modeling.

## Pre-filtering

While not necessary, [pre-filtering](http://bioconductor.org/packages/devel/bioc/vignettes/DESeq2/inst/doc/DESeq2.html#pre-filtering) the count table to exclude genes that were poorly measures helps to not only reduce the size of the DESeq2 object, but also gives you a sense of how many genes were measured at the sequencing depth chosen for your experiment.

Here we will filter out any genes that are lacking counts across any of the samples, i.e.: all columns are zero.

```r
keep <- rowSums(counts(dds)) >= 1
dds <- dds[keep,]
```

After filtering, we'll take a break before proceeding.

---

# Sources
* HBC DGE setup: https://hbctraining.github.io/DGE_workshop/lessons/01_DGE_setup_and_overview.html
* HBC Count Normalization: https://hbctraining.github.io/DGE_workshop/lessons/02_DGE_count_normalization.html
* DESeq2 standard vignette: http://bioconductor.org/packages/devel/bioc/vignettes/DESeq2/inst/doc/DESeq2.html
* DESeq2 beginners vignette: https://bioc.ism.ac.jp/packages/2.14/bioc/vignettes/DESeq2/inst/doc/beginner.pdf
* Bioconductor RNA-seq Workflows: https://www.bioconductor.org/help/course-materials/2015/LearnBioconductorFeb2015/B02.1_RNASeq.html




---

These materials have been adapted and extended from materials listed above. These are open access materials distributed under the terms of the [Creative Commons Attribution license (CC BY 4.0)](http://creativecommons.org/licenses/by/4.0/), which permits unrestricted use, distribution, and reproduction in any medium, provided the original author and source are credited.
