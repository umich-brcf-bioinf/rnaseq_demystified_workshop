---
title: "Day 2 - Module 7a: Introduction to R/Rstudio "
author: "UM Bioinformatics Core"
date: "2020-11-17"
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
> * Shared understanding of R/RStudio environment & packages    
> * Import and review count tables    
> * Understand how to find help

# Using RStudio

Our goal for any analysis is to do it in a way where it can be easily and exactly replicated, i.e. following standards for "reproducible research". Rstudio has great tools to facilitate this process and is freely available, just like the underlying programming language (R). 

First, we'll review the key parts of Rstudio, specifically:   
* Console   
* Scripts    
* Environments    
* File/Plots Viewer    

*Add diagram of Rstudio here?*

## Using Rmarkdown & navigating directories

Today we'll be using a series of [Rmarkdown](https://rmarkdown.rstudio.com/) files that should have been included in the course materials. To open the file for the first module today, we'll want to navigate to the same project directory we used yesterday, which can be done using the file viewer within Rstudio. 

*Add example of project directory?*

After opening the file, we'll review how Rmarkdown files mix both text and executable code blocks:


```r
# comments are for our reference
getwd() # copy and paste this command intothe console
```


## Best practices for file organization

To follow best practices as discussed by [Nobel,2009](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424) for file organization for bioinformatics/computational projects, we will need to make sure there are separate storage locations for:  
* Raw data   
* Code   
* Output files    

*Add diagram from Noble PLOS paper* 

## Helpful resources

*[Rmarkdown cheatsheet](https://rstudio.com/wp-content/uploads/2015/02/rmarkdown-cheatsheet.pdf)  

# Interacting with the Console

To ensure that we have a record of our commands, it is better to write all commands within our '.Rmd' file and send the command to the console instead of directly entering them. **Ctrl-Enter** is a standard shortcut in Rstudio to send the current line (or selected lines) to the console. If you see an `>`, then R has executed the command. If you see a `+`, this means that the command is not complete and R is waiting (usually for a `)`).

## Key R syntax review

For a more extensive introduction to R syntax, there are many resources available. We drew on [Data Carpentry](https://datacarpentry.org/R-genomics/00-before-we-start.html) for these materials and also recommend the [Software Carpentry](https://swcarpentry.github.io/r-novice-gapminder/) materials for a more extensive overview of the R programming language.

### Functions

We already used a function when reviewing the Rmarkdown code blocks. These are built in capablities of R or capabilities that have been added to our session with supplementary packages. 

Functions have a specific pattern and usually get one or more inputs called **arguments**. If the argument alters the way the function operates, it is often called an **option**. Some functions can take multiple arguements but may have defaults to fall back on if no specific argument or option is given.


```r
print() # does this work?
print("this is an argument")

round(3.14159) 
round(3.14159, 2) # how many digits do we get?
```

### Object names & assignments

Objects are strings that act as place holders for any type of data. R has some restrictions for naming objects:    
* Cannot start with numbers   
* Cannot include dashes   
* Cannot have spaces    
* Should not be identical to a named function    
* Dots & underscores will work but are better to avoid    


```r
3 + 7 
Mars  <- "planet"
Number <- 3 + 7 # can also use a `=`
Mars <- "red"

### check the values here ###
```

Objects maintain their assignment unless reassigned or removed from the session environment. 

### Vectors and data types

There are a few different data types that we will be working with when analyzing RNA-seq data. We'll review vectors and what they can store first.


```r
VectorExString <- c("MFG1, MFG2, MFG3")
VectorExNumeric <- c(4, 5, 6)
VectorExLogical <- c(TRUE, FALSE, FALSE)

### get an overview of each vector with the function `str()` ###

### How can we convert strings to factors? ###
```


### Reading in count data

The starting point for our differential expression analysis will be a table of counts, so let's look at the example provided. First, we'll need to read in the file into memory from storage using a function.

*Note: will likely need to update file name and syntax to ensure properly read in*

```r
CountTable <- read.table("./data/ExampleCounts.txt") 
head(CountTable) # look at the top of the table

### get an overview of the structure of the table ###
```

Now that the file is read into R, we can work with the data and we've created a data frame, which is a standard data structure for tabular data in R.

### Working with data frames

Data frames are a cornerstone of working with data in R and there are some key syntax and functions that will be helpful as we analyze RNA-seq data. *Include how to add columns or not??*

```r
dim(CountTable) # lets us know the number of rows & columns

CountTable[1, ] # lets us look at the first row
CountTable[ , 1] # lets us look at the first column

rownames(CountTable) # rownames, if they exist
colnames(CountTable) # colnames, if they exist

### Subset the table by rows & columns ###

### Shorthand for select a column by name ($ or with string) ###
```

### Getting help within Rstudio

Rstudio has some built in help resources.


```r
?dim
```

# Check package installations

Before ending this module, load the libraries we'll need to use for the next module. *will need to update with any additional packages*

```r
library(DESeq2)
library(ggplot2)
library(pheatmap)
library('ggrepel', character.only=TRUE)
```

