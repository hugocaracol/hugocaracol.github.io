---
layout: post
title: "Relevant keywords using TF-IDF"
date: 2013-07-16 16:26
comments: true
categories: [data-mining]
---
TF-IDF is a numerical statistic that tells us how important some word is in a document. It stands for **term frequency - inverse document frequency**.
There are a lot of words with a high term frequency like *the*, *a*, *and*, but they, themselves, don't convey much 
meaning. In other words, they are not **keywords**.

<!-- more -->

The second part of this statistic is the **inverse document frequency** which tells us how common a word is across all documents. It 
is calculated by dividing the total number of documents in a collection by the number of documents containing the specific word we want to analyse.
After that quotient is determined, we take the logarithm of that.


**TF-IDF** = n_count_word_document **x** log2(count_all_documents / count_documents_containing_word)

Let's get some data for some keywords in this blog post. 

*The values in the *TF* column include title of this post and exclude anything below this paragraph.
 The values in the *No docs containing term* column result from googling the term we are analysing. It is assumed, for demonstration
purposes that google contains 50 billion documents indexed.*


<table>
<tr>
<td><bold>Term</bold></td>
<td>TF</td>
<td>No. docs containing term</td>
<td>TF-IDF</td>
</tr>

<tr>
<td>tf-idf</td>
<td>3</td>
<td><center>0.662 B</center></td>
<td>18.72</td>
</tr>

<tr>
<td>statistic</td>
<td>2</td>
<td><center>0.0604 B</center></td>
<td>19.39</td>
</tr>

<tr>
<td>keyword</td>
<td>3</td>
<td><center>0.992 B</center></td>
<td>16.97</td>
</tr>

<tr>
<td>frequency</td>
<td>4</td>
<td><center>0.391 B</center></td>
<td>28</td>
</tr>

<tr>
<td>blog</td>
<td>1</td>
<td><center>9.34 B</center></td>
<td>2.42</td>
</tr>

</table>


As you can see, the term *blog* does not make a good keyword. All other terms are a good fit.

