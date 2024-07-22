
# rhinotypeR

<!-- badges: start -->
<!-- badges: end -->


               /**       /**                       /**                                   /*** *** 
              | **      |**/                      | **                                  | **__  **
      /****** | *******  /** /*******   /******  /******   /**   /**  /******   /****** | **  \ **
     /**__  **| **__  **| **| **__  ** /**__  **|_  **_/  | **  | ** /**__  ** /**__  **| *******/
    | **  \__/| **  \ **| **| **  \ **| **  \ **  | **    | **  | **| **  \ **| ********| **__  **
    | **      | **  | **| **| **  | **| **  | **  | ** /**| **  | **| **  | **| **_____/| **  \ **
    | **      | **  | **| **| **  | **|  ******/  |  *****/|  *******/| ********|  *******/| **  | 
    |__/      |__/  |__/|__/|__/  |__/ \______/    \___/   \____  **| **____/  \_______/|__/  |__/
                                                           /**  | **| **                          
                                                          |  ******/| **                          
                                                           \______/ |__/                          

## Table of Contents

1.  [Background](#Background)
2.  [Test-Data](#Test-Data)
3.  [Workflow](#Workflow)
4.  [Package](#Package)
5.  [Citation](#Citation)
6.  [Contributors](#Contributors)

## Background

Rhinoviruses (RV), common respiratory pathogens, are positive-sense,
single-stranded RNA viruses characterized by a high antigenic diversity
and mutation rate. With their genome approximately 7.2 kb in length, RVs
exhibit mutation rates between 10^-3 and 10^-5 mutations per nucleotide
per replication event. These viruses are classified into 169 types
across three species: RV-A, RV-B, and RV-C. Genotype assignment, a
critical aspect of RV research, is based on pairwise genetic distances
and phylogenetic clustering with prototype strains, a process currently
executed manually and laboriously.

## Test-Data

The project utilizes VP4/2 sequences available in the public domain from
GenBank and reference prototype strains from www.picornaviridae.com The
input datasets (target, reference and prototype) are fasta files. Here’s
an example of a FASTA file: ![fasta
file](https://github.com/omicscodeathon/rhinotyper/blob/main/man/figures/example_fasta_file.png)

## Workflow

<figure>
<img
src="https://github.com/omicscodeathon/rhinotyper/blob/main/man/figures/workflow.png"
alt="workflow" />
<figcaption aria-hidden="true">workflow</figcaption>
</figure>

RhinotypeR workflow. The user downloads prototype strains using
`getPrototypeSeqs()` function, combines these with their newly generated
VP4/2 sequences, aligns and manually curates the alignment. The user
then reads the curated alignment into R using `readFasta()` function.
The readFasta object can then be used to run all the second-level
functions, including `assignTypes()` which assigns the sequences into
genotypes, filters out the prototype sequences and returns the genotype
assignment of the new sequences. This output can be used to visualise
the frequency of assigned genotypes. The distance matrix object, an
output of `pairwiseDistance()` function, can be used to create a
phylogenetic tree or a heatmap to visualize genetic relatedness of
sequences.

## Package

Our project aims to develop an R package to automate RV genotype
assignment, facilitating genomic scientists in efficiently genotyping RV
infections.

Our methodology involves: 1. Parsing and preprocessing of VP4/2 sequence
data. 2. Implementation of algorithms to calculate pairwise genetic
distances. 3. Integration of methods for constructing Maximum Likelihood
phylogenetic trees.

### Installation

You can install the development version of rhinotypeR from
[GitHub](https://github.com) with:

``` r
devtools::install_github("omicscodeathon/rhinotypeR")
```

##### Load Library

``` r
library("rhinotypeR")
```

## Functions

The package encompasses functions to compute genetic distances, perform
phylogenetic clustering, and compare sequences against RV prototype
strains. These functionalities are designed to be user-friendly and
adaptable to various research needs.

- The package (summarized in Table 1) does the following:
  - Assigns genotypes to query sequences
  - Computes for pairwise distance among query sequences
  - Calculates pairwise distance between query and prototype sequences
  - Calculates overall genetic distance of query sequences

#### Table 1. A summary of the functions

| Function                | Role                                                                                                                                                                                                                                                                                                                                                            | Input                                            | Output                                                                            |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------------------|
| `getPrototypeSeqs()`    | Downloads rhinovirus prototype strains into a local directory. These sequences should be combined with and aligned alongside newly generated sequences before being imported into R for genotype assignment                                                                                                                                                     | Destination path                                 | RV prototypes are downloaded into the local machine                               |
| `readFasta`()           | Reads sequence alignment/fasta files into R for processing                                                                                                                                                                                                                                                                                                      | fasta file                                       | A fasta file imported into R                                                      |
| `SNPeek()`              | Visualizes single nucleotide polymorphisms (SNPs) relative to a specified reference sequence. To specify the reference, move it to the bottom of the alignment. Substitutions are color-coded by nucleotide: A = green, T = red, C = blue, G = yellow                                                                                                           | fasta file                                       | A plot highlighting SNPs per sequence                                             |
| `plotAA()`              | Plots amino acid substitutions with a specified sequence as the reference. The input is an amino acid fasta file (translated DNA sequences). To specify the reference sequence, move it to the bottom of the alignment. Changes are coloured by the class of amino acid: Red = Positively charged, Blue = Negatively charged, Green = Polar, Yellow = Non-polar | Amino acid fasta file                            | A plot highlighting amino acid substitutions per sequence                         |
| `assignTypes()`         | Rapidly assigns genotypes to input sequences. The input fasta file should include the prototype strains, which can be downloaded using getPrototypeSeqs()                                                                                                                                                                                                       | fasta file                                       | CSV file with three columns: sequence header, assigned type, and genetic distance |
| `pairwiseDistances()`   | Estimates pairwise distances across input sequences using a specified evolutionary model                                                                                                                                                                                                                                                                        | fasta file                                       | A dense distance matrix                                                           |
| `overallMeanDistance()` | Estimates the overall mean distance of input sequences                                                                                                                                                                                                                                                                                                          | fasta file                                       | A single numeric value                                                            |
| `countSNPs`()           | Counts single nucleotide polymorphisms across input sequences                                                                                                                                                                                                                                                                                                   | fasta file                                       | A dense matrix                                                                    |
| `PlotFrequency()`       | Plots the frequency of assigned genotypes This function uses the output of assignTypes() as input                                                                                                                                                                                                                                                               | output from assignTypes                          | Barplot                                                                           |
| `PlotDistances()`       | Visualizes pairwise genetic distances in a heatmap. This function uses the output of pairwiseDistances() as input                                                                                                                                                                                                                                               | distance matrix from prototype distance function | Heatmap                                                                           |
| `PlotTree()`            | Plots a simple phylogenetic tree using the genetic distances estimated by pairwiseDistances()                                                                                                                                                                                                                                                                   | output from pairwise distances                   | A simple phylogenetic tree                                                        |

### Running the functions

#### Function 1: getPrototypeSeqs

- Downloads rhinovirus prototype strains into a local directory. These
  sequences should be combined with and aligned alongside newly
  generated sequences before being imported into R for genotype
  assignment.

Example

``` r
getPrototypeSeqs(destinationFolder = "./output")
#> Warning in file.create(to[okay]): cannot create file './output/RVRefs.fasta',
#> reason 'No such file or directory'
#> The reference sequences have been downloaded to ./output
```

Own data

``` r
# getPrototypeSeqs(destinationFolder = "path to an output folder")
```

##### Function 2: readFasta

- Reads sequence alignment/fasta files into R for processing.

Example

``` r
   # Load the dataset
  test <- system.file("extdata", "test.fasta", package = "rhinotypeR")
  
  # run command
  readFasta(test)
#> $sequences
#>                                                                                                                                                                                                                                                                                                                                                                                                                       AF343652.1_RVB99 
#> "ATGGGTGCACAGGTTTCAACTCAGAAAAGCGGTTCTCATGAAAATCAGAACATTCTTACCAACGGCTCAAATCAAACATTCACAGTGATTAATTATTACAAGGATGCAGCCAGTTCATCATCCGCTGGACAATCATTATCAATGGATCCAAGCAAGTTTACTGAACCAGTAAAGGACATCATGTTAAAAGGTGCACCAGCACTTAATTCACCTAATATAGAAGCTTGTGGTTATAGTGATAGAGTGGAACAAATAACATTAGGCAATTCCACAATTACCACTCAAGAAGCAGCAAACACAGTTGTTGCTTATGGAGAATGGCCTTCTTTTCTATCAGACAATGATGCTAGTGATGTTAATAAAACTACCAAACCAGATACTTCAGCTTGTAGATTTTATACTTTAGATAGTAAGTTGTGG" 
#>                                                                                                                                                                                                                                                                                                                                                                                                                       AY040238.1_RVB92 
#> "ATGGGAGCTCAGGTGTCTACACAGAAGAGTGGTTCACACGAAAACCAAAACATTTTAACCAATGGGTCTCATCAAACATTTACAGTTATCAACTATTATAAAGATGCAGCAAGTTCATCATCAGCTGGCCAATCTTTGTCAATGGATCCATCCAAATTTACTGAACCAGTGAAAGATCTAATGTTAAAAGGTGCTCCAGCATTGAATTCACCAAATGTTGAGGCATGTGGCTACAGTGATAGAGTTCAGCAGATCACGCTTGGAAATTCAACAATTACAACGCAGGAAGCTGCCAATGCTGTTGTCTGCTATGCAGAATGGCCGGAATATTTATCAGATAATGATGCAAGTGATGTGAACAAAACTTCCAAACCAGACACCTCAGTGTGCAGGTTTTACACACTAGATAGTAAAGATTGG" 
#> 
#> $headers
#> [1] "AF343652.1_RVB99" "AY040238.1_RVB92"
```

Own data

``` r
# readFasta("path to fastaFile")
```

##### Function 3: SNPeek

- Visualizes single nucleotide polymorphisms (SNPs) relative to a
  specified reference sequence. To specify the reference, move it to the
  bottom of the alignment. Substitutions are color-coded by nucleotide:
  A = green, T = red, C = blue, G = yellow.

Example

``` r
  # Load the dataset
  test <- system.file("extdata", "test.fasta", package = "rhinotypeR")
  #read input
  fastaData <- readFasta(fastaFile = test)
  # run command
  SNPeek(fastaData, showLegend = F)
```

<img src="man/figures/README-unnamed-chunk-8-1.png" width="100%" />

Own data

``` r
  # fastaData <- readFasta(fastaFile = "path to fasta file")
  # SNPeek(fastaData, showLegend = F)
```

##### Function 4: assignTypes

- Rapidly assigns genotypes to input sequences. The input fasta file
  should include the prototype strains, which can be downloaded using
  getPrototypeSeqs().
- The model could be either p-distance, JC, Kimura2p, Tamura3p based on
  your preference

Example

``` r
  # Load the dataset
  test <- system.file("extdata", "input_aln.fasta", package = "rhinotypeR")
  #read input
  fastaD <- readFasta(test)
  # run command
  head(assignTypes(fastaD, "p-distance"))
#>        query assignedType   distance       reference
#> 1 MT177836.1   unassigned         NA  AY040242.1_B97
#> 2 MT177837.1   unassigned         NA  AY040242.1_B97
#> 3 MT177838.1          B99 0.08974359  AF343652.1_B99
#> 4 MT177793.1          B42 0.08012821  AY016404.1_B42
#> 5 MT177794.1         B106 0.05769231 KP736587.1_B106
#> 6 MT177795.1         B106 0.05769231 KP736587.1_B106
```

Own data

``` r
  # fastaD <- readFasta("path to fasta file")
  # assignTypes(fastaD, "p-distance")
```

##### Function 5: pairwiseDistances

- Estimates pairwise distances across input sequences using a specified
  evolutionary model.

Example

``` r
  # Load the dataset
  test <- system.file("extdata", "input_aln.fasta", package = "rhinotypeR")
  
  # Example usage
  fastaD <- readFasta(test)
  
  output <- pairwiseDistances(fastaD, "p-distance", gapDeletion = TRUE)
  #pairwiseDistances(fastaD, "JC", gapDeletion = TRUE)
  #pairwiseDistances(fastaD, "Kimura2p", gapDeletion = TRUE)
  #pairwiseDistances(fastaD, "Tamura3p", gapDeletion = TRUE)
```

Own data

``` r
# fastaD <- readFasta("path to fasta file")
# pairwiseDistances(fastaD, model = "p-distance", gapDeletion = TRUE)
```

##### Function 6: overallMeanDistance

- Estimates the overall mean distance of input sequences.

Example

``` r
  # Load the dataset
  test <- system.file("extdata", "input_aln.fasta", package = "rhinotypeR")
  
  # usage
  fastaData <- readFasta(test)
  
  overallMeanDistance(fastaData, model="p-distance")
#> [1] 0.3332344
  overallMeanDistance(fastaData, model="JC")
#> [1] 0.4605268
  overallMeanDistance(fastaData, model="Kimura2p")
#> [1] 0.4621316
  overallMeanDistance(fastaData, model="Tamura3p")
#> [1] 0.5252031
```

Own data

``` r
# fastaD <- readFasta("path to fasta file")
# overallMeanDistance(fastaD,  model="p-distance")
```

##### Function 7: countSNPs

- Counts single nucleotide polymorphisms across input sequences.

Example

``` r
  # Load the dataset
  test <- system.file("extdata", "test.fasta", package = "rhinotypeR")
  
  # run
  fastaData <- readFasta(test)
  countSNPs(fastaData)
#>                  AF343652.1_RVB99 AY040238.1_RVB92
#> AF343652.1_RVB99                0               93
#> AY040238.1_RVB92               93                0
```

Own data

``` r
# fastaD <- readFasta("path to fasta file")
# countSNPs(fastaD) 
```

##### Function 8: plotFrequency

- Plots the frequency of assigned genotypes. This function uses the
  output of assignTypes() as input.

Example

``` r
  # Load the dataset
  test <- system.file("extdata", "input_aln.fasta", package = "rhinotypeR")
  
  # Run 
  queryFastaData <- readFasta(test)
  df <- assignTypes(queryFastaData, "p-distance")
  
  plotFrequency(df)
```

<img src="man/figures/README-unnamed-chunk-18-1.png" width="100%" />

``` r
  
  # Color legend
  # "A" = "blue", "B" = "red", "C" = "green", "Other" = "grey"
```

Own data

``` r
  #queryFastaData <- readFasta("path to fasta file")
  #df <- assignTypes(queryFastaData, "p-distance")
  #plotFrequency(df)
```

##### Function 9: plotDistances

- Visualizes pairwise genetic distances in a heatmap. This function uses
  the output of pairwiseDistances() as input.

Example

``` r
  # Load the dataset
  test <- system.file("extdata", "input_aln.fasta", package = "rhinotypeR")
  
  # Example usage
  fastaD <- readFasta(test)
  distancesToPrototypes <- pairwiseDistances(fastaD, "p-distance")
  plotDistances(distancesToPrototypes)
```

<img src="man/figures/README-unnamed-chunk-20-1.png" width="100%" />

Own data

``` r
  #fastaD <- readFasta("path to fasta file")
  #distancesToPrototypes <- pairwiseDistances(fastaD, "p-distance")
  #plotDistances(distancesToPrototypes)
```

##### Function 10: plotTree

- Plots a simple phylogenetic tree using the genetic distances estimated
  by pairwiseDistances().

Example

``` r
  # Load the dataset
  test <- system.file("extdata", "input_aln.fasta", package = "rhinotypeR")
  
  # Example usage
  fastaD <- readFasta(test)
  pdistances <- pairwiseDistances(fastaD, "p-distance")
  plotTree(pdistances)
```

<img src="man/figures/README-unnamed-chunk-22-1.png" width="100%" />

Own data

``` r
 #fastaD <- readFasta("path to fasta file")
 #pdistances <- pairwiseDistances(fastaD, "p-distance")
 #plotTree(pdistances)
```

##### Function 11: plotAA

- Plots amino acid substitutions with a specified sequence as the
  reference. The input is an amino acid fasta file (translated DNA
  sequences). To specify the reference sequence, move it to the bottom
  of the alignment. Changes are coloured by the class of amino acid: Red
  = Positively charged, Blue = Negatively charged, Green = Polar, Yellow
  = Non-polar.

Example

``` r
  # Load the dataset
  test <- system.file("extdata", "test.translated.fasta", package = "rhinotypeR")
  
  # usage
  plotAA(test, showLegend = T)
```

<img src="man/figures/README-unnamed-chunk-24-1.png" width="100%" />

Own data

``` r
  #fastaData <- "path to fasta file"
  #plotAA(fastaData, showLegend = T)
```

## Contributors

- Martha Luka
- Ruth Nanjala
- Wafaa Rashed
- Winfred Gatua
- Olaitan Awe

## Citation

Martha M. Luka1, Ruth Nanjala2, Wafaa M. Rashed3,4, Winfred Gatua5,6,
Olaitan I. Awe7

- Authors’ Affiliation

- School of Biodiversity, One Health and Veterinary Medicine, University
  of Glasgow, Glasgow, G12 8QQ, UK.

- Kennedy Institute of Rheumatology, Nuttfield Department of
  Orthopaedics, Rheumatology and Musculoskeletal Sciences, University of
  Oxford, UK.

- Pharmacy Practice Department, Faculty of Pharmacy, Ahram Canadian
  University, Egypt.

- Computational Systems Biology Laboratory, USP, Brazil

- MRC Integrative Epidemiology Unit, University of Bristol, UK.  

- Population Health Sciences, Bristol Medical School, UK.

- African Society for Bioinformatics and Computational Biology, Cape
  Town, South Africa.
