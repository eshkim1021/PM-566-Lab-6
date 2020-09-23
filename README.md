---
title: "Lab 06 - Text Mining"
name: "Edward Kim"
output: github_document
---

```{r setup}
knitr::opts_chunk$set()
```

# Learning goals

- Use `unnest_tokens()` and `unnest_ngrams()` to extract tokens and ngrams from text.
- Use dplyr and ggplot2 to analyze text data

# Lab description

For this lab we will be working with a new dataset. The dataset contains transcription samples from https://www.mtsamples.com/. And is loaded and "fairly" cleaned at https://raw.githubusercontent.com/USCbiostats/data-science-data/master/00_mtsamples/mtsamples.csv.

This markdown document should be rendered using `github_document` document.

### Setup packages

You should load in `dplyr`, (or `data.table` if you want to work that way), `ggplot2` and `tidytext`.
If you don't already have `tidytext` then you can install with

```{r, eval=FALSE}
install.packages("tidytext")
```

### read in Medical Transcriptions

Loading in reference transcription samples from https://www.mtsamples.com/

```{r, warning=FALSE, message=FALSE}
library(readr)
library(tidyverse)
library(tidytext)
mt_samples <- read_csv("https://raw.githubusercontent.com/USCbiostats/data-science-data/master/00_mtsamples/mtsamples.csv")
mt_samples <- mt_samples %>%
  select(description, medical_specialty, transcription)

head(mt_samples)

mt_samples$transcription[1]
```

---

## Question 1: What specialties do we have?

We can use `count()` from `dplyr` to figure out how many different catagories do we have? Are these catagories related? overlapping? evenly distributed?

```{r}
mt_samples %>%
  count(medical_specialty, sort = TRUE)
```
There are 40 unique medical specialties in this dataset. Some of these categories are related, as surgery is a very general medical specialty that includes aspects from other medical fields such as cardiology, OB-GYN, and neurosurgery. 

---

## Question 2

- Tokenize the the words in the `transcription` column
- Count the number of times each token appears
- Visualize the top 20 most frequent words

```{r}
mt_samples %>% 
  unnest_tokens(token,transcription) %>% 
  count(token,sort =TRUE)
```

Explain what we see from this result. Does it makes sense? What insights (if any) do we get?


This result shows the words that appear with the highest frequency in the 
---

## Question 3

- Redo visualization but remove stopwords before
- Bonus points if you remove numbers as well

What do we see know that we have removed stop words? Does it give us a better idea of what the text is about?

---

# Question 4

repeat question 2, but this time tokenize into bi-grams. how does the result change if you look at tri-grams?

---

# Question 5

Using the results you got from questions 4. Pick a word and count the words that appears after and before it.

---

# Question 6 

Which words are most used in each of the specialties. you can use `group_by()` and `top_n()` from `dplyr` to have the calculations be done within each specialty. Remember to remove stopwords. How about the most 5 used words?

# Question 7 - extra

Find your own insight in the data:

Ideas:

- Interesting ngrams
- See if certain words are used more in some specialties then others

