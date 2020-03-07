Cancer cells
================
Timothy Keyes
2020 -

  - [Challenge scenario](#challenge-scenario)
  - [Reading in unfamiliar file
    types](#reading-in-unfamiliar-file-types)
  - [Functional programming, biological data, and
    `purrr`](#functional-programming-biological-data-and-purrr)
  - [Exploratory data analysis in
    cancer](#exploratory-data-analysis-in-cancer)
  - [Answers](#answers)

``` r
# Libraries
library(tidyverse)

# If you will not be providing answers, simply delete the following lines.
# Parameters
  # File where generated answers are saved, by default in the home directory
file_answers <- "~/Desktop/answers.rds"
  # Save answers
SAVE_ANSWERS <- TRUE



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
q1.1 <- 
  stem_cell_path %>% 
  readr::read_table() %>% 
  as_tibble()
```

    ## Parsed with column specification:
    ## cols(
    ##   `FCS3.0          58    2767    2808  350220       0       0\$BEGINANALYSIS\0\$ENDANALYSIS\0\$BEGINDATA\2808\$ENDDATA\350220\$BEGINSTEXT\0\$ENDSTEXT\0\$NEXTDATA\0\$MODE\L\$BYTEORD\4,3,2,1\$DATATYPE\F\$TOT\2227\$P1B\32\$P2B\32\$P3B\32\$P4B\32\$P5B\32\$P6B\32\$P7B\32\$P8B\32\$P9B\32\$P10B\32\$P11B\32\$P12B\32\$P13B\32\$P14B\32\$P15B\32\$P16B\32\$P17B\32\$P18B\32\$P19B\32\$P20B\32\$P21B\32\$P22B\32\$P23B\32\$P24B\32\$P25B\32\$P26B\32\$P27B\32\$P28B\32\$P29B\32\$P30B\32\$P31B\32\$P32B\32\$P33B\32\$P34B\32\$P35B\32\$P36B\32\$P37B\32\$P38B\32\$P39B\32\$P12E\0,0\$P28E\0,0\$P31S\pSTAT5\$P31R\13855.6572265625\$P31N\pSTAT5\$P11S\IgMi\$P11R\26322.21484375\$P27S\IgMs\$P27R\13833.779296875\$P11N\IgMi\$P27N\IgMs\$P31E\0,0\$P11E\0,0\$COM\FCS file exported using Cytobank\$P27E\0,0\$P30S\p4EBP1\$P30R\2783.00927734375\$P30N\p4EBP1\$P10S\CD123\$P10R\5319.43359375\$P26S\HLADR\$P26R\24278.958984375\$P10N\CD123\$P26N\HLADR\$P30E\0,0\$P10E\0,0\$P26E\0,0\FCSversion\3\$P25S\FITC_myeloid\$P25R\763.592651367188\$P25N\FITC_myeloid\$P9S\CD179a\$P9R\11147.751953125\$P9N\CD179a\$P25E\0,0\$P9E\0,0\$P24S\CD3\$P24R\11.1146621704102\$P24N\CD3\$P8S\CD34\$P8R\13575.072265625\$P8N\CD34\$P24E\0,0\$P8E\0,0\$P23S\CD58\$P23R\6808.6162109375\$P39S\pCreb\$P39R\14289.2177734375\$P23N\CD58\$P7S\CD20\$P39N\pCreb\$P7R\10276.1337890625\$P19S\TdT\$P19R\26589.607421875\$P7N\CD20\$P19N\TdT\$P23E\0,0\$P39E\0,0\$P7E\0,0\$P19E\0,0\$P22S\CD38\$P22R\11913.115234375\$P38S\pErk\$P38R\6016.47216796875\$P22N\CD38\$P6S\CD79b\$P38N\pErk\$P6R\8258.806640625\$P18S\RAG1\$P18R\8620.6484375\$P6N\CD79b\$P18N\RAG1\$P22E\0,0\$P38E\0,0\$P6E\0,0\$P18E\0,0\$P21S\CD43\$P21R\19411.81640625\$P37S\pS6\$P37R\15393.0419921875\$P21N\CD43\$P5S\tIkaros\$P37N\pS6\$P5R\7666.611328125\$P17S\CD127\$P17R\7267.35302734375\$P5N\tIkaros\$P17N\CD127\$P21E\0,0\$P37E\0,0\$PAR\39\$P5E\0,0\$P17E\0,0\$P20S\Pax5\$P20R\13989.185546875\$P36S\pSyk\$P36R\2738.23706054688\$P20N\Pax5\$P4S\CD22\$P36N\pSyk\$P4R\12587.466796875\$P16S\TSLPr\$P16R\5719.853515625\$P4N\CD22\$P16N\TSLPr\$P20E\0,0\$P36E\0,0\$P4E\0,0\$P16E\0,0\$P35S\pAkt\$P35R\6217.02587890625\$P3S\CD19\$P35N\pAkt\$P3R\2356.96337890625\$P15S\CD24\$P15R\18105.341796875\$P3N\CD19\$P15N\CD24\$P35E\0,0\$P3E\0,0\$P15E\0,0\$P34S\Gd157\$P34R\13546.6044921875\$P34N\Gd157\$P2S\CD45\$P2R\216.802230834961\$P14S\CD179b\$P14R\20432.74609375\$P2N\CD45\$P14N\CD179b\$P34E\0,0\$P2E\0,0\$P14E\0,0\$P33S\pIkaros\$P33R\5973.9814453125\$P1S\CD235_61\$P33N\pIkaros\$P1R\11.2323770523071\$P13S\CD10\$P13R\18186.587890625\$P29S\pPLCg1_2\$P1N\CD235_61\$P29R\3799.17724609375\$P13N\CD10\$P29N\pPLCg1_2\$P33E\0,0\$P1E\0,0\$P13E\0,0\$P29E\0,0\$P32S\Ki67\$P32R\13648.31640625\$P32N\Ki67\$P12S\Kappa_lambda\$P12R\8981.7265625\$P28S\cPARP\$P28R\8.71114444732666\$P12N\Kappa_lambda\$P28N\cPARP\$P32E\0,0\` = col_character()
    ## )

``` r
# Print results
q1.1
```

    ## # A tibble: 2,141 x 1
    ##    `FCS3.0          58    2767    2808  350220       0       0\\$BEGINANALYSIS\…
    ##    <chr>                                                                        
    ##  1 "=*Z#\xbf%\xf7\x19\xbf\x1f\xad\xffC\x03\x93\xf1\xbd\x1a\x16p\xbe\xd9\xbf\xb0…
    ##  2 "\xbf\x02\x9b\xb7=\xfd\ba\xbd\x9a"                                           
    ##  3 "\xdf?\x85\xb8\x12\xbf\a\x98+\xbe\xfd\xc3\xd0\xbf\x1e\xee@C\x19:\xdd=\x82bz?…
    ##  4 "\xd5\xbf);'B\xed\xca\x1d@"                                                  
    ##  5 "\x03\x02@\x19\x16TA82\x95?\x14{\x7f\xbf8\xca\x99\xbeO\xd5&?\xa3\x97\x92\xbf…
    ##  6 "?3\x11\x1f@1\xd4\xe2?@;)\xbf\x04\xcf\x8e\xbf\b\xa8\x8b\xbe\xd8Q\xf5\xbe\xd6…
    ##  7 "\xc1\xd7?0\x89\xc5@3\xc8\xea\xbe\xf5\xba7B\xb3\x11+?\xc7\x88\x90A\x17\x12\x…
    ##  8 "j\xbe\x9a)\xb8C6\xce\xc2@\v]"                                               
    ##  9 "A\x9c\x8b6\xbd\xc8l\xbcB\xab\xc0\xfaB4\xb7\xd9>\xd7\x13_\xbf"               
    ## 10 "\x14\xb1\xbf%\x80^?J\xf2\t\xbf4U\xcc\xbd\x91gX?f\xb4\xe6\xbe0\xba(@*!\xe4\x…
    ## # … with 2,131 more rows

From above, we can see that reading in a .fcs file gives us a lot of
nonsense. You can see that there’s a weird header that `readr` doesn’t
like to parse, and there are a decent number of non-standard characters
that get a weird representation in the RStudio console. In general, .fcs
files don’t play well with standard `readr` functions.

There are some specific reasons for this that have to do with how the
file is formatted - .fcs stands for “flow cytometry standard,” a
specialized text file type that was developed by the International
Society for the Advancement of Cytometry. Specifically, .fcs files have
a bunch of weird character headings and other metadata stored alongside
the data that you usually want, and most of that metadata is a bit
challenging to parse. Many biological applications have their own
specific file type, and many of them are fairly complicated.

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

There are many packages that can read in .fcs files and perform various
computations with them, but the most well-known ones are the following:

  - `flowCore`
  - `cytofkit`

**q1.3** You find the package `flowCore` on Bioconductor and decide to
use it for the rest of your analysis. Figure out how to download this
package from Bioconductor, and attach it to your R Session.

Hint: You may be prompted to update some of the packages on your
computer after `flowCore` is installed. You can update them if you want,
but if you just want to continue with the challenge, just type `n` into
the console.

``` r
if (!requireNamespace("BiocManager", quietly = TRUE)) {
  install.packages("BiocManager")
}

BiocManager::install("flowCore")
```

    ## Bioconductor version 3.9 (BiocManager 1.30.10), R 3.6.2 (2019-12-12)

    ## Installing package(s) 'flowCore'

    ## 
    ## The downloaded binary packages are in
    ##  /var/folders/m2/zt80gp694kdf_4293f4ckwhm0000gn/T//RtmpodByGu/downloaded_packages

    ## Old packages: 'bit', 'callr', 'cli', 'dendextend', 'devtools', 'hardhat',
    ##   'klaR', 'knitr', 'labelled', 'lwgeom', 'partykit', 'rlang', 'rstan',
    ##   'rstanarm', 'sf', 'SQUAREM', 'stringi', 'text2vec', 'tidymodels',
    ##   'tidypredict', 'tidyr', 'tidyselect', 'vctrs', 'yardstick', 'ade4', 'BH',
    ##   'bookdown', 'broom', 'caTools', 'checkmate', 'deldir', 'digest', 'dplyr',
    ##   'DT', 'FactoMineR', 'fansi', 'farver', 'foreach', 'foreign', 'future',
    ##   'ggnetwork', 'ggridges', 'gh', 'gplots', 'hexbin', 'Hmisc', 'hms', 'janitor',
    ##   'jsonlite', 'laeken', 'lattice', 'leaps', 'leiden', 'manipulateWidget',
    ##   'metap', 'mime', 'mnormt', 'modelr', 'multcomp', 'mvtnorm', 'nlme', 'nnet',
    ##   'pbkrtest', 'plotly', 'prettyunits', 'processx', 'ps', 'R.methodsS3',
    ##   'ranger', 'RcppAnnoy', 'RcppProgress', 'RCurl', 'recipes', 'remotes',
    ##   'reticulate', 'rgl', 'rmarkdown', 'rrcov', 'rstudioapi', 'rsvd', 'rtweet',
    ##   'RVAideMemoire', 'Seurat', 'shinyjs', 'sn', 'sp', 'statmod', 'tidycensus',
    ##   'tigris', 'tinytex', 'umap', 'uuid', 'vcd', 'VIM', 'vroom', 'xfun', 'XML',
    ##   'xts', 'yaml', 'zoo'

``` r
library(flowCore)
```

    ## 
    ## Attaching package: 'flowCore'

    ## The following object is masked from 'package:tibble':
    ## 
    ##     view

**q1.4** Use `flowCore`’s `read.FCS()` function to read your 01\_HSC.fcs
file into a variable called `q1.4`. What does the result look like?
Inspect the `class()` of `q1.4`. What does `flowCore`’s documentation
tell you about what `q1.4` is?

``` r
q1.4 <- 
  stem_cell_path %>% 
  read.FCS(transformation = FALSE, truncate_max_range = FALSE)

class(q1.4)
```

    ## [1] "flowFrame"
    ## attr(,"package")
    ## [1] "flowCore"

`q1.4` is a data structure called a “flowFrame”, which is a type of
annotated `dataFrame` specifically developed for storing flow or mass
cytometry data. There are 3 main parts to an object of class
`flowFrame`:

1.  A numeric matrix of the raw measurement values where each row
    represents an event (one cell) and each column represents one
    parameter (a protein measurement).

2.  Annotations (aka short descriptions) for each of the parameters
    (e.g., the measurement channels, stains, dynamic range)

3.  Some additional annotation provided through “keywords” in the FCS
    file (most of this is junk).

**q1.5** Read the documentation for the `flowCore::exprs()` function,
and briefly describe what it does. Use `flowCore::exprs()` to extract
the most interesting information from `q1.4`, and convert the result
into a tibble. Store this tibble into the variable `q1.5`.

``` r
q1.5 <- 
  q1.4 %>% 
  exprs() %>% 
  as_tibble()
```

The `exprs()` function extracts the numeric matrix component from a
flowFrame.

**q1.6** After reading in `q1.5`, it’s apparent to you that each row in
the tibble represents a single cell…based on the column names, what do
you think the columns represent? Try googling a few of the column names
to get a sense of what they represent.

Each of the column names of the dataset represents the name of a
specific protein that the authors measured when they were collecting
their data on the mass cytometer. Thus, `q1.5` represents the protein
expression of each cell along its rows for each protein along its
columns.

For instance, CD45 is a surface marker protein expressed on all cells of
the hematopoietic (blood) lineage, whereas CD34 is a protein expressed
only on hematopoietic stem cells (and other primitive cell types). So,
it makes sense that a lab studying blood cancer might include these
markers in their experiments.

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

``` r
healthy_cells <- 
  readRDS("~/GitHub/dcl-2020-01/tim/c01-own/data/challenge_healthy_cells.rds")

cancer_cells <- 
  readRDS("~/GitHub/dcl-2020-01/tim/c01-own/data/challenge_cancer_cells.rds")
```

**q2.2** Using the descriptions above and your own tidyverse sleuthing
skills, try to understand `healthy_cells` and `cancer_cells`. What does
the `data` column in `healthy_cells` (or `cancer_cells`) represent?

Hint: Do the column names in each element of `healthy_cells$data` remind
you at all of `q1.5`?

`healthy_cells` contains a list-column in `data` in which each column
represents one of several protein measurements and each row represents a
single cell, as before. So, it is a nested tibble\! `cancer_cells`
contains a very similar data structure, except for cancer cells (not
healthy ones).

**q2.3** You’re interested in learning a bit more about each of the cell
populations in `healthy_cells`. Add a column called `cell_number` to
`healthy_cells` that represents the number of cells in each
subpopulation. Store the result as a new tibble named `q2.3`.

``` r
q2.3 <- 
  healthy_cells %>%
  mutate(cell_number = map_int(data, nrow))
q2.3
```

    ## # A tibble: 15 x 3
    ##    population        data                    cell_number
    ##    <chr>             <list>                        <int>
    ##  1 HSC               <tibble [2,227 × 11]>          2227
    ##  2 Progenitor_1      <tibble [15,015 × 11]>        15015
    ##  3 Progenitor_2      <tibble [7,182 × 11]>          7182
    ##  4 Progenitor_3      <tibble [3,524 × 11]>          3524
    ##  5 Pre_Pro_B         <tibble [1,428 × 11]>          1428
    ##  6 Pro_B1            <tibble [2,072 × 11]>          2072
    ##  7 Pro_B2            <tibble [2,727 × 11]>          2727
    ##  8 Pre_B1            <tibble [3,839 × 11]>          3839
    ##  9 Pre_B2            <tibble [32,846 × 11]>        32846
    ## 10 Early_Progenitors <tibble [4,051 × 11]>          4051
    ## 11 Late_Progenitors  <tibble [78,164 × 11]>        78164
    ## 12 Immature_B1       <tibble [7,525 × 11]>          7525
    ## 13 Immature_B2       <tibble [4,225 × 11]>          4225
    ## 14 Mature_B          <tibble [98,323 × 11]>        98323
    ## 15 Mature_Non_B      <tibble [336,624 × 11]>      336624

**q2.4** Visualize the number of cells for each subpopulation in `q2.3`
as an EDA plot. What conclusions can you draw?

``` r
q2.3 %>%
  mutate(
    population = 
      factor(population, levels = population) %>% 
      fct_rev()
  ) %>% 
  ggplot(aes(x = population, y = cell_number)) + 
  geom_col() + 
  coord_flip()
```

![](solution_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

From the plot above, we can see that certain subpopulations are much
more rare than others - for instance, HSCs (hematopoeitic stem cells)
are relatively rare in the bone marrow, but mature cells (“Mature\_B”
and “Mature\_Non\_B”) are much more common.

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
q2.5 <- 
  q2.3 %>%
  mutate(
    data = 
      data %>% 
      map(~ mutate_all(., ~ asinh(. / 5)))
  ) 
```

**q2.6** You’re interested in applying a clustering algorithm to your
data, but it requires that you know the mean expression of each protein
in each population from `q2.5`. Add a column to `q2.5` called `means` in
which each entry is a named vector of means for each protein in the
`data` column, and store the result in `q2.6`.

Hint: If you’re stuck, trying using `purrr::map()` across all of the
tibbles in `q2.5$data` inside of a call to `mutate()`

``` r
q2.6 <- 
  q2.5 %>% 
  mutate(
    means = map(
      data, 
      ~ (.) %>% 
        summarize_all(mean) %>% 
        pivot_longer(everything()) %>% 
        deframe()
    )
  )
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
get_cov(q2.6$data[[1]])
```

    ##               CD19        CD20         CD34        CD38        IgMi        IgMs
    ## CD19    7.91530592 -2.05716229  0.338737191  0.55102911 -1.02741549 -1.07637220
    ## CD20   -2.05716229  4.12779315 -0.050380226  0.48912636 -0.23646233  0.04664455
    ## CD34    0.33873719 -0.05038023  1.490775907 -0.31616302  0.22384186  0.04699325
    ## CD38    0.55102911  0.48912636 -0.316163023 13.79485875 -0.20440865 -0.16618142
    ## IgMi   -1.02741549 -0.23646233  0.223841855 -0.20440865  1.60623662 -1.14764175
    ## IgMs   -1.07637220  0.04664455  0.046993253 -0.16618142 -1.14764175  4.59275668
    ## CD179a  0.08183568 -0.52063228 -0.065491659  0.05501776 -0.01499236 -0.09755677
    ## CD179b -0.88695765  0.10337680  0.193384066 -0.15045998 -0.09753549  0.15227145
    ## CD127  -0.06810981  0.08308286  0.001165767 -0.73184642 -0.11448041  0.10438760
    ## Tdt    -0.06151013  0.46226595 -1.100245973 -1.11811871 -0.08093413 -0.16640976
    ## CD45   -1.27006714 -0.64239074 -0.359115962 -0.78314377 -0.59880283 -0.30532250
    ##             CD179a      CD179b        CD127         Tdt       CD45
    ## CD19    0.08183568 -0.88695765 -0.068109814 -0.06151013 -1.2700671
    ## CD20   -0.52063228  0.10337680  0.083082857  0.46226595 -0.6423907
    ## CD34   -0.06549166  0.19338407  0.001165767 -1.10024597 -0.3591160
    ## CD38    0.05501776 -0.15045998 -0.731846416 -1.11811871 -0.7831438
    ## IgMi   -0.01499236 -0.09753549 -0.114480405 -0.08093413 -0.5988028
    ## IgMs   -0.09755677  0.15227145  0.104387604 -0.16640976 -0.3053225
    ## CD179a  3.05269122 -0.11879796  0.058199630 -0.06901753  0.1088058
    ## CD179b -0.11879796 13.23258430 -0.037643251 -0.46806368  0.3036847
    ## CD127   0.05819963 -0.03764325  2.100567886 -0.26724802 -0.2817127
    ## Tdt    -0.06901753 -0.46806368 -0.267248021  8.15889708 -0.8108360
    ## CD45    0.10880578  0.30368467 -0.281712669 -0.81083598  5.0855320

Your clustering algorithm also requires you to calculate the covariance
matrix for each tibble in the `data` column of `q2.6`. Use `get_cov()`
Add a column to `q2.6` called `covariance` in which each entry the
covariance matrix of the corresponding tibble in `q2.6`’s `data` column.
Store the result in `q2.7`.

``` r
q2.7 <- 
  q2.6 %>% 
  mutate(covariance = map(data, get_cov))
```

**q2.8 (optional)** Phew\! That was a lot of `purrr`. You’re happy that
this dataset is relatively small, so the compute time for the wrangling
above wasn’t too bad. But you also know that some biological datasets
can be quite large. Look online for some documentation about the `furrr`
package: if you had a large dataset, how might using `furrr` make your
life easier?

The `furrr` package is a useful tool if you want to do parallel
computing without changing much of your `purrr` syntax: it uses the
`futures` package to tell your computer to divvy up each of the
computations in a `purrr:map_*()` function so that they can happen at
the same time on different parts of your computer (which makes the
computation overall much faster).

With a very large dataset, `furrr` could make your life easier by
speeding up your computations so that you don’t have to wait as long for
your results. Nice\!

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

``` r
q3 <- 
  q2.7 %>% 
  select(population, healthy = data) %>%
  left_join(
    cancer_cells %>% 
      rename(cancer = data), 
    by = "population"
  ) %>% 
  mutate(population = factor(population, levels = POPULATIONS)) %>% 
  pivot_longer(
    cols = c(healthy, cancer), 
    names_to = "condition", 
    values_to = "data"
  ) %>% 
  unnest(cols = data) %>% 
  pivot_longer(
    cols = c(-population, -condition), 
    names_to = "channel", 
    values_to = "value"
  ) %>% 
  group_by(population, condition, channel) %>% 
  summarize(
    median = median(value), 
    upper = quantile(x = value, probs = 0.75), 
    lower = quantile(x = value, probs = 0.25)
  ) %>% 
  ungroup()

cancer_plot <- function(my_channel) { 
  q3 %>% 
    dplyr::filter(channel == my_channel) %>% 
    ggplot(aes(x = population, y = median, color = condition)) + 
    geom_line(aes(group = condition), size = 1.5) + 
    geom_crossbar(
      aes(ymin = lower, ymax = upper, fill = condition), 
      alpha = 0.3, 
      color = NA
    ) +
    geom_point(aes(fill = condition), shape = 21, color = "black", size = 3) + 
    geom_text(
      aes(x = population, y = median + 1.5), 
      data = 
        q3 %>% 
        dplyr::filter(
          population %in% c("Pro_B2", "Pre_B1"), 
          channel == my_channel
        ) %>% 
        group_by(channel, population) %>% 
        summarize(median = mean(median)), 
      label = "\n↓", 
      color = "black", 
      size = 6
    ) + 
    labs(
      title = str_c(my_channel, " expression in cancer and healthy cells"), 
      subtitle = 
        "Points indicate medians; crossbars indicate interquartile range",
      x = NULL, 
      y = str_c(my_channel, " expression (arbitrary units)"),
      color = "Condition",
      fill = "Condition",
      caption = "Source: Good et al., 2018, Nature Medicine"
    ) + 
    theme(
      axis.text.x = element_text(angle = 90, hjust = 1, vjust = 0.5), 
      legend.position = "bottom"
    )
}

map(.x = MARKERS, .f = cancer_plot)
```

    ## [[1]]

<img src="solution_files/figure-gfm/unnamed-chunk-15-1.png" width="100%" />

    ## 
    ## [[2]]

<img src="solution_files/figure-gfm/unnamed-chunk-15-2.png" width="100%" />

    ## 
    ## [[3]]

<img src="solution_files/figure-gfm/unnamed-chunk-15-3.png" width="100%" />

    ## 
    ## [[4]]

<img src="solution_files/figure-gfm/unnamed-chunk-15-4.png" width="100%" />

    ## 
    ## [[5]]

<img src="solution_files/figure-gfm/unnamed-chunk-15-5.png" width="100%" />

    ## 
    ## [[6]]

<img src="solution_files/figure-gfm/unnamed-chunk-15-6.png" width="100%" />

    ## 
    ## [[7]]

<img src="solution_files/figure-gfm/unnamed-chunk-15-7.png" width="100%" />

    ## 
    ## [[8]]

<img src="solution_files/figure-gfm/unnamed-chunk-15-8.png" width="100%" />

    ## 
    ## [[9]]

<img src="solution_files/figure-gfm/unnamed-chunk-15-9.png" width="100%" />

    ## 
    ## [[10]]

<img src="solution_files/figure-gfm/unnamed-chunk-15-10.png" width="100%" />

    ## 
    ## [[11]]

<img src="solution_files/figure-gfm/unnamed-chunk-15-11.png" width="100%" />

The plots above are plotted like a time-series because the populations
on the x axis are ordered according to which come first in developmental
time biologically. Here, we emphasize the medians of the population
distributions as well as their inter-quartile ranges in order to get a
sense of the central tendency *and* dispersion of the subpopulations.

In the last 3 plots, we can see that, in most cases, there is at least
quite a bit of overlap between the cancer and healthy populations, which
suggests that the authors’ “alignment” algorithm works fairly well at
least for several important markers. However, when we look at CD19, we
see that many of the earlier populations are not similar at all to their
healthy counterparts.

However, we note in each of the plots that the two populations the
authors were most interested in (indicated with arrows) always
overlapped quite a bit (and often are right on top of one another),
suggesting that there might be more validity in these populations than
in others that are less of-interest.

## Answers

To create an RDS file with answers, save all of your solutions in
variables such as `q1`, `q2.1`, etc. The following code will create an
answer file when you knit the solution.Rmd file. You specify where the
answer file is saved using the `file_answers` variable in the
parameters.

To provide answers, set `eval=TRUE` in the chunk below. If you will not
be providing answers, simply delete the following lines.

Save answers.

``` r
if (SAVE_ANSWERS) {
  ls(pattern = "^q[1-9][0-9]*(\\.[1-9][0-9]*)*$") %>%
    str_sort(numeric = TRUE) %>% 
    set_names() %>% 
    map(get) %>%
    discard(is.ggplot) %>%
    write_rds(file_answers)
}
```
