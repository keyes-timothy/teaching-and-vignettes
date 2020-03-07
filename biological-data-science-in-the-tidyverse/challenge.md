Cancer Cells
================
Your Name
2020-

  - [Challenge scenario](#challenge-scenario)
  - [Reading in unfamiliar file
    types](#reading-in-unfamiliar-file-types)
  - [Functional programming, biological data, and
    `purrr`](#functional-programming-biological-data-and-purrr)
  - [Exploratory data analysis in
    cancer](#exploratory-data-analysis-in-cancer)

``` r
# Libraries
library(tidyverse)
library(dcl)

# If you will not be providing answers, simply delete the following lines.
# Parameters
  # File for downloaded answers
file_answers <- "~/Desktop/answers.rds"

### Parameters
stem_cell_path <- file.path("~", "Desktop", "healthy_pops", "01_HSC.fcs")

MARKERS <- 
  c(
    'CD19', 
    'CD20', 
    'CD34', 
    'CD38', 
    'IgMi', 
    'IgMs', 
    'CD179a', 
    'CD179b',
    'CD127', 
    'Tdt',
    'CD45'
  )

POPULATIONS <- 
  c(
    "HSC", 
    "Progenitor_1", 
    "Progenitor_2", 
    "Progenitor_3", 
    "Pre_Pro_B", 
    "Pro_B1", 
    "Pro_B2", 
    "Pre_B1", 
    "Pre_B2", 
    "Early_Progenitors", 
    "Late_Progenitors", 
    "Immature_B1", 
    "Immature_B2", 
    "Mature_B", 
    "Mature_Non_B"
  )
  



#===============================================================================

# Read in answers
if (str_length(file_answers) > 0) {
  answers <- read_rds(file_answers) 
}
```

## Challenge scenario

Imagine that you’re a first-year graduate student who just joined a new
lab that studies cancer. Specifically, your lab studies leukemia, or
cancer of the blood and bone marrow. You’re excited to get started with
your research as the group’s new biomedical data scientist\!

As your first task, your advisor has asked you to look into some of the
analyses that were published a few years ago in [this seminal
paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5953207/) in the
journal *Nature Medicine*. The group that published this work used a new
technology called [“mass
cytometry”](https://en.wikipedia.org/wiki/Mass_cytometry) to measure
proteins on millions of cells that were collected from pediatric
patients with leukemia. They did a variety of analyses on these
measurements, and you’re interested in delving a bit into their
methodology and results for your own project.

## Reading in unfamiliar file types

**q1.1** You’re able to download some of the data files that the *Nature
Medicine* authors used in their original analyses. They are called
[“.fcs” files](https://en.wikipedia.org/wiki/Flow_Cytometry_Standard),
a file type that you’ve never worked with before.

Download the file called “01\_HSC.fcs” from Box and store in a directory
that is not a GitHub repository. Try reading in the .fcs file using the
`readr::read_table()` function that you’re familiar with from the
`tidyverse`, and store your results in a variable of your choosing. What
is your result? Try inspecting it using `View()`, `glimpse()`, `str()`,
or any other method you’d like. Describe your results.

``` r
# Compare result with answer
if (exists("q1")) q1.1
```

**q1.2** You notice that reading in .fcs files is a bit complicated
because of all the additional metadata they store (in addition to the
data you actually want). You remember your advisor mentioning a website
called [Bioconductor](http://bioconductor.org/) where bioinformaticians
and quantitative biologists publish software packages for other
researchers to use. You wonder if there’s a package somewhere on
Bioconductor that could help you read in this .fcs file.

Look through Bioconductor and list at least two packages that can
process .fcs files. What are these packages’ names?

Hint: Google is your friend\! Remember, you’re working with flow
cytometry and mass cytometry data.

**q1.3** You find the package `flowCore` on Bioconductor and decide to
use it for the rest of your analysis. Figure out how to download this
package from Bioconductor, but **don’t** attach it to your R Session.

Hint: You may be prompted to update some of the packages on your
computer after `flowCore` is installed. You can update them if you want,
but if you just want to continue with the challenge, just type `n` into
the console.

**q1.4** Use `flowCore`’s `read.FCS()` function to read your 01\_HSC.fcs
file into a variable called `q1.4`. What does the result look like?
Inspect the `class()` of `q1.4`. What does `flowCore`’s documentation
tell you about what `q1.4` is?

Hint: Don’t attach `flowCore` to your R session - instead, just use
`flowCore::read.FCS()`.

``` r
# Compare result with answer
if (exists("q1.4")) compare(answers$q1.4, q1)
```

**q1.5** Read the documentation for the `flowCore::exprs()` function,
and briefly describe what it does. Use `flowCore::exprs()` to extract
the most interesting information from `q1.4`, and convert the result
into a tibble. Store this tibble into the variable `q1.5`.

Hint: Don’t attach `flowCore` to your R session - instead, just use
`flowCore::exprs()`.

``` r
# Compare result with answer
if (exists("q1.5")) compare(answers$q1.5, q1.5)
```

**q1.6** After reading in `q1.5`, it’s apparent to you that each row in
the tibble represents a single cell…based on the column names, what do
you think the columns represent? Try googling a few of the column names
to get a sense of what they represent.

## Functional programming, biological data, and `purrr`

You perform some additional data wrangling with some of the .fcs files
you found on the website of the lab whose work you’re investigating.
Ultimately, you end up with the two data files saved on Box, called
“healthy\_cells.rds” and “cancer\_cells.rds”.

“healthy\_cells.rds” contains a tibble in which each row represents a
distinct subpopulation of healthy blood cells. The populations are
provided in the order that they mature in the bone marrow (row 1 comes
before row 2 and so on), so you can think of the data as having a
temporal element (similar to a time series).

“cancer\_cells.rds” contains a tibble in which each row represents a
cancer cell population (there are 1000 cells in each population). We’ll
talk more about the cancer data later, so don’t worry too much about it
now.

Download both files and save them into a directory that is not a GitHub
repository.

**q2.1** Read both files into variables called `healthy_cells` and
`cancer_cells`, respectively.

**q2.2** Using the descriptions above and your own tidyverse sleuthing
skills, try to understand `healthy_cells` and `cancer_cells`. What does
the `data` column in `healthy_cells` (or `cancer_cells`) represent?

Hint: Do the column names in each element of `healthy_cells$data` remind
you at all of `q1.5`?

**q2.3** You’re interested in learning a bit more about each of the cell
populations in `healthy_cells`. Add a column called `cell_number` to
`healthy_cells` that represents the number of cells in each
subpopulation. Store the result as a new tibble named `q2.3`.

``` r
# Compare result with answer
if (exists("q2.3")) compare(answers$q2.3, q2.3)
```

**q2.4** Visualize the number of cells for each subpopulation in `q2.3`
as an EDA plot. What conclusions can you draw?

**q2.5** Biological data often require a bit of pre-processing before
they’re ready for analysis. For mass cytometry data, it’s customary to
perform an “Arcsinh transformation” of each measurement after dividing
by 5. For example, pre-processing the value `100` would look like the
following:

``` r
asinh(100 / 5)
```

    ## [1] 3.689504

Transform all of the protein measurements in `q2.3` using this strategy
and save your result in a tibble called `q2.5`.

``` r
# Compare result with answer
if (exists("q2.5")) compare(answers$q2.5, q2.5)
```

**q2.6** You’re interested in applying a clustering algorithm to your
data, but it requires that you know the mean expression of each protein
in each population from `q2.5`. Add a column to `q2.5` called `means` in
which each entry is a named vector of means for each protein in the
`data` column, and store the result in `q2.6`.

Hint: If you’re stuck, trying using `purrr::map()` across all of the
tibbles in `q2.5$data` inside of a call to `mutate()`

``` r
# Compare result with answer
if (exists("q2.6")) compare(answers$q2.6, q2.6)
```

**q2.7** The covariance matrix of a dataset can be computed with the
following function:

``` r
get_cov <- function(data) {
  Sx <- cov(data)
  Sx <- solve(Sx)
  return(Sx)
}

#example: 
if (exists("q2.6")) get_cov(q2.6$data[[1]])
```

Your clustering algorithm also requires you to calculate the covariance
matrix for each tibble in the `data` column of `q2.6`. Use `get_cov()`
Add a column to `q2.6` called `covariance` in which each entry the
covariance matrix of the corresponding tibble in `q2.6`’s `data` column.
Store the result in `q2.7`.

``` r
# Compare result with answer
if (exists("q2.7")) compare(answers$q2.7, q2.7)
```

**q2.8 (optional)** Phew\! That was a lot of `purrr`. You’re happy that
this dataset is relatively small, so the compute time for the wrangling
above wasn’t too bad. But you also know that some biological datasets
can be quite large. Look online for some documentation about the `furrr`
package: if you had a large dataset, how might using `furrr` make your
life easier?

## Exploratory data analysis in cancer

**q3** The authors of the original paper claimed that they were able to
use a clustering algorithm to “align cancer cells with their most
similar healthy subpopulation” in order to learn how cancer cells relate
to their native lineage in the healthy body.

The tibble `cancer_cells` that you read in earlier contains 1000 cancer
cells from each “aligned” subpopulation of healthy cells. Do some EDA on
the cancer cells and make a presentation plot that illustrates the
similarity (or lack of similarity) of the cell populations in
`healthy_cells` and `cancer_cells`.

Getting the whole picture will probably require you to make a few plots.
At the very least, look into the most important proteins for this
lineage: `CD19`, `CD34`, `CD38`, and `CD45`. Also, note that the authors
claimed that the `Pro_B2` and `Pre_B1` populations were the most
important for the biology they were interested in. Why might that make
sense (or why not)?

Do you agree with the authors’ original statement that the cancer cells
and healthy cells are similar?

Hint: Remember to use `q2.5` (or a later tibble) to plot the healthy
values, since we’ve transformed them\! `cancer_cells` is already arcsinh
pre-transformed for you.
