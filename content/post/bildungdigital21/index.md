---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Bildungdigital21"
subtitle: "Future direction for german digital education strategy"
summary: "Producing a fast tagloud from a transcript of a Youtube video"
authors: ["admin"]
tags: ["rstats", "NLP", "coding"]
categories: 

- Codesnippets

date: 2021-03-07T08:05:11+01:00
lastmod: 2021-03-07T08:05:11+01:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

## A new direction for the German digital education strategy?

The German government has organized on the 22nd of February 2021 an online-event regarding a future strategy on digital education. The event was a mix of political intention statements and some advertisement of existing initiatives. What I found remarkable was that there is still the vision, that any overarching OER platform would solve any problems with education despite this has failed on the European level already many years ago. The video of the discourse is embedded below.

<iframe width="560" height="315" src="https://www.youtube.com/embed/WAlP39a5LUU" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Visualizing the discourse
{{< tweet 1363950810644766728 >}}
To get a fast overview about the main topics of the discourse I have hacked together a short R-script to produce a tagcloud from the most used meaningful words in the discourse.

```R
# We first install the Youtubecaption library
install.packages('youtubecaption')
# Now we import the captions from the video
library(youtubecaption)
library(quanteda)
library(tidyverse)
url <- "https://www.youtube.com/watch?v=WAlP39a5LUU"
caption <- get_caption(url, language ="de")
caption
# Now we need to make a document-term-matrix from the content 
my_corpus <- corpus(caption$text)
summary(my_corpus)
# Extending the stopwordlist with a more extended one from Github
ger_stopwords <- read_lines("https://raw.githubusercontent.com/stopwords-iso/stopwords-de/master/stopwords-de.txt")
custom_stopwords <- setdiff(ger_stopwords, stopwords("german"))
# Constructing the 
meine.dfm <- dfm(my_corpus, remove_numbers = TRUE, remove_punct = TRUE, remove = c(stopwords("german"), custom_stopwords))
meine.dfm.trim <- dfm_trim(meine.dfm, min_docfreq = 1, min_nchar = 3)
# Visualizing the matrix
textplot_wordcloud(meine.dfm.trim, min_size = 1, max_size = 2, max_words = 100)
```
This code takes the full transcript from youtube, builds a corpus, removes stopwords and visualizes the most frequent words in the resulting tagloud.

{{< figure src="featured.png" caption="Tagcloud from the event" >}}