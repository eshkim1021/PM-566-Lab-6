Lab 06 - Text Mining
================

``` r
knitr::opts_chunk$set()
```

# Learning goals

  - Use `unnest_tokens()` and `unnest_ngrams()` to extract tokens and
    ngrams from text.
  - Use dplyr and ggplot2 to analyze text data

# Lab description

For this lab we will be working with a new dataset. The dataset contains
transcription samples from <https://www.mtsamples.com/>. And is loaded
and “fairly” cleaned at
<https://raw.githubusercontent.com/USCbiostats/data-science-data/master/00_mtsamples/mtsamples.csv>.

This markdown document should be rendered using `github_document`
document.

### Setup packages

You should load in `dplyr`, (or `data.table` if you want to work that
way), `ggplot2` and `tidytext`. If you don’t already have `tidytext`
then you can install with

``` r
install.packages("tidytext")
```

### read in Medical Transcriptions

Loading in reference transcription samples from
<https://www.mtsamples.com/>

``` r
library(readr)
library(tidyverse)
library(tidytext)
mt_samples <- read_csv("https://raw.githubusercontent.com/USCbiostats/data-science-data/master/00_mtsamples/mtsamples.csv")
mt_samples <- mt_samples %>%
  select(description, medical_specialty, transcription)

head(mt_samples)
```

    ## # A tibble: 6 x 3
    ##   description                  medical_specialty   transcription                
    ##   <chr>                        <chr>               <chr>                        
    ## 1 A 23-year-old white female ~ Allergy / Immunolo~ "SUBJECTIVE:,  This 23-year-~
    ## 2 Consult for laparoscopic ga~ Bariatrics          "PAST MEDICAL HISTORY:, He h~
    ## 3 Consult for laparoscopic ga~ Bariatrics          "HISTORY OF PRESENT ILLNESS:~
    ## 4 2-D M-Mode. Doppler.         Cardiovascular / P~ "2-D M-MODE: , ,1.  Left atr~
    ## 5 2-D Echocardiogram           Cardiovascular / P~ "1.  The left ventricular ca~
    ## 6 Morbid obesity.  Laparoscop~ Bariatrics          "PREOPERATIVE DIAGNOSIS: , M~

``` r
mt_samples$transcription[1]
```

    ## [1] "SUBJECTIVE:,  This 23-year-old white female presents with complaint of allergies.  She used to have allergies when she lived in Seattle but she thinks they are worse here.  In the past, she has tried Claritin, and Zyrtec.  Both worked for short time but then seemed to lose effectiveness.  She has used Allegra also.  She used that last summer and she began using it again two weeks ago.  It does not appear to be working very well.  She has used over-the-counter sprays but no prescription nasal sprays.  She does have asthma but doest not require daily medication for this and does not think it is flaring up.,MEDICATIONS: , Her only medication currently is Ortho Tri-Cyclen and the Allegra.,ALLERGIES: , She has no known medicine allergies.,OBJECTIVE:,Vitals:  Weight was 130 pounds and blood pressure 124/78.,HEENT:  Her throat was mildly erythematous without exudate.  Nasal mucosa was erythematous and swollen.  Only clear drainage was seen.  TMs were clear.,Neck:  Supple without adenopathy.,Lungs:  Clear.,ASSESSMENT:,  Allergic rhinitis.,PLAN:,1.  She will try Zyrtec instead of Allegra again.  Another option will be to use loratadine.  She does not think she has prescription coverage so that might be cheaper.,2.  Samples of Nasonex two sprays in each nostril given for three weeks.  A prescription was written as well."

-----

## Question 1: What specialties do we have?

We can use `count()` from `dplyr` to figure out how many different
catagories do we have? Are these catagories related? overlapping? evenly
distributed?

``` r
mt_samples %>%
  count(medical_specialty, sort = TRUE)
```

    ## # A tibble: 40 x 2
    ##    medical_specialty                 n
    ##    <chr>                         <int>
    ##  1 Surgery                        1103
    ##  2 Consult - History and Phy.      516
    ##  3 Cardiovascular / Pulmonary      372
    ##  4 Orthopedic                      355
    ##  5 Radiology                       273
    ##  6 General Medicine                259
    ##  7 Gastroenterology                230
    ##  8 Neurology                       223
    ##  9 SOAP / Chart / Progress Notes   166
    ## 10 Obstetrics / Gynecology         160
    ## # ... with 30 more rows

There are 40 unique medical specialties in this dataset. Some of these
categories are related, as surgery is a very general medical specialty
that includes aspects from other medical fields such as cardiology,
OB-GYN, and neurosurgery. The data is not evenly distributed as surgery
was the most common medical specialty.

-----

## Question 2

  - Tokenize the the words in the `transcription` column
  - Count the number of times each token appears
  - Visualize the top 20 most frequent words

<!-- end list -->

``` r
mt_samples %>% 
  unnest_tokens(token,transcription) %>% 
  count(token,sort =TRUE)
```

    ## # A tibble: 23,647 x 2
    ##    token        n
    ##    <chr>    <int>
    ##  1 the     149888
    ##  2 and      82779
    ##  3 was      71765
    ##  4 of       59205
    ##  5 to       50632
    ##  6 a        42810
    ##  7 with     35815
    ##  8 in       32807
    ##  9 is       26378
    ## 10 patient  22065
    ## # ... with 23,637 more rows

Explain what we see from this result. Does it makes sense? What insights
(if any) do we get?

## This result shows the words that appear with the highest frequency in the

## Question 3

  - Redo visualization but remove stopwords before
  - Bonus points if you remove numbers as well

What do we see know that we have removed stop words? Does it give us a
better idea of what the text is about?

-----

# Question 4

repeat question 2, but this time tokenize into bi-grams. how does the
result change if you look at tri-grams?

-----

# Question 5

Using the results you got from questions 4. Pick a word and count the
words that appears after and before it.

-----

# Question 6

Which words are most used in each of the specialties. you can use
`group_by()` and `top_n()` from `dplyr` to have the calculations be done
within each specialty. Remember to remove stopwords. How about the most
5 used words?

# Question 7 - extra

Find your own insight in the data:

Ideas:

  - Interesting ngrams
  - See if certain words are used more in some specialties then others
