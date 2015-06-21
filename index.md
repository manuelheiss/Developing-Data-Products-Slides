---
title       : Developing Data Products
subtitle    : String Similarity Measures Shiny App
author      : 
job         : 
framework   : io2012
highlighter : highlight.js
hitheme     : tomorrow
widgets     : [mathjax]
mode        : selfcontained
knit        : slidify::knit2slides
---

### Introduction

In a world with typos and fuzziness everywhere it is highly valuable to be able to compare the similarity of stings. The shiny app developed in this programming assignment provides functionality to compute the $\textit{String Similarity}$ between two given strings according to a bunch of common $\textit{String Similarity Measures}$. A string similarity measure is a function that maps two strings to a value between $0$ and $1$ indicating the degree of similarity between both. Two strings one wants to compare can be entered to the shiny app. Similarity measures that should be applied can be chosen by using check boxes. After hitting the calculate button the results are displayed at the right. Please note that the two input string are only compared after lowercasing them and that the resulting similarities are rounded. The following slides introduce the different Similarity Measures incorporated into the app.

--- .class #id

### Exact, Jaro and Jaro-Winkler Similarity

The $\textit{Exact Similarity}$ is just one if the two strings are the same and zero if not.

All of the following string similarity measures are provided in the "stringdist" package.

The $\textit{Jaro Similarity}$ ($j$) of the strings $s^{}_{1}$ and $s^{}_{2}$ is defined as
$j = \left\{ \begin{array}{l l} 0 & \text{for }m = 0\\ \frac{1}{3}\left(\frac{m}{|s^{}_{1}|} + \frac{m}{|s^{}_{2}|} + \frac{m - t}{m}\right) & \text{else} \end{array} \right.$

Where $m$ is the number of matching characters and $t$ is half the number of transpositions between $s^{}_{1}$ and $s^{}_{2}$.
The $\textit{Jaro-Winkler Similarity}$ is than the Jaro Similarity plus the Winkler bonus for common prefixes $jw = j + (\ell \cdot p \cdot (1 - j))$. ($\ell$ is the common prefix length, $p = 0.1$). Comparing "text" and "texts" results in similarity 0.93 for Jaro and 0.96 for Jaro-Winkler. Whereas "text" and "stext" have the same similarity (0.93) since there is no common prefix. 


--- .class #id

### (Damerau-)Levenshtein and N-Gram Similarity

$\textit{Levenshtein distance}$ is calculated by counting all string operations needed to transform one string into another. Valid operations are insertion, deletion and substitution of characters. E.g. substitute 's' with 'x' to transform "test" into "text". To get a similarity from that distance, we have to normalize it with $1 - \frac{distance}{\min\{|s^{}_{1}|, |s^{}_{2}|\}}$, where $|\cdot|$ gives the length of a string:

```r
1 - stringdist("test", "text", method = "lv") / min(4, 4)
```

```
## [1] 0.75
```
$\textit{Damerau-Levenshtein}$ is an extension of Levenshtein which has an additional operation, the transposition of characters. Comparing "test" and "tset" has a Damerau-Levenshtein Similarity of 0.75. Levenshtein Similarity is just 0.5 since two substitutions are needed instead of one transposition.

The $\textit{N-Gram Similarity}$ is another extension of Levenshtein. Instead of working on single characters it works on so called $n$-grams which are character sets of length $n$. For $n = 1$ Levenshtein and N-Gram Similarity are equal.

--- .class #id 

### Cosine, Jaccard and SoundEx Similarity

$\textit{Cosine}$ and $\textit{Jaccard Similarity}$ also operate on $n$-grams.

The formula for the Cosine Similarity is given by $1- \frac{a^{T}b}{\sqrt{a^{T}_{}a\cdot b^{T}_{}b}}$, where $a$ and $b$ are the vectors holding the $n$-gram frequencies of the two strings. With $n = 2$ the frequency vectors for "abc" and "cab" are $(1, 1, 0)^{T}_{}$ and $(1, 0, 1)^{T}_{}$, so the similarity is $1 - \frac{1}{\sqrt{2 \cdot 2}} =$ 0.5.

Jaccard Similarity is given by $1- \frac{|A \cap B|}{|A \cup B|}$, where $A$ and $B$ are the set of $n$-grams of the two compared strings.
Using Jaccard with $n = 2$ the similarity between "abc" and "cab" is 0.33 since the $2$-grams are "ab", "bc" and "ca" and both share "ab".

$\textit{SoundEx}$ is a phonetic similarity measure. It computes some kind of phonetic hash value for each string. If those values are the same the similarity is one and if not zero. Comparing "test" with "text" gives us a similarity of 1, whereas "test" and "teot" have similarity 0. All other similarity measures would have the same similarity for this two comparisons.
