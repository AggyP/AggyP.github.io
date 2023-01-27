---
title: Text analytics - Sentiment analysis customer review
date: 2019-10-13
categories: [Assignment]
math: true
---

**Programming** - R 

**Extraction Tool** - DataMiner

This work focus on sentiment analysis of two of the most famous beauty products brand
which are Sephora and Ulta to provide insight for the enhancement of the related products and
applications according to the actual needs of the market.

### Web scraping and Dataset

Data is extracted from two review website:
trustpilot.com, a website founded in Denmark in 2007 which publishes reviews for online businesses.
consumeraffairs.com, a website that empowers consumers by providing a forum for their reviews.

The data is extracted using DataMiner, a Google chrome extension that can scrape data form web pages into a CSV file.

Recipes created to extract data from both websites:

![Picture](/assets/textimages/Q3text.png)

Here is example of one review from trustpilot:

![Picture](/assets/textimages/review1.png)

Here is example of one review from trustpilot:

![Picture](/assets/textimages/review2.png)

The table below shows the data being extracted

| **Object** | **Source** | **No. of Reviews** |
| Sephora | Trust Pilot | 216 |
| Sephora | Consumer Affairs | 420 |
| Ulta Beauty | Trust Pilot | 289 |
| Ulta Beauty | Consumer Affairs | 350 |

### Data Cleaning

The reviews are converted into a collection of text documents or know as "Corpus". The *tm* package in R is used to convert text into corpus as shown as following:

        >#Creating the corpus
        > docs1 <- Corpus(VectorSource(sephoradata$Text))

A set of pre-processing is performed on the corpus as following:

![Picture](/assets/textimages/clean.png)

Example before cleaning:

![Picture](/assets/textimages/before.png)

Example after cleaning: 

![Picture](/assets/textimages/after.png)

### Bag of words

With a clean corpus, a term document matric (TDM) which consists of the words used in the text and their frequecies.Therefore, we can see which are the most frequent words.

**Bar plot with the 20 top frequent words.**:

![Picture](/assets/textimages/barplot1.png)


**Wordcloud**

A word cloud can also be generated to visualize the frequently used words.

        #Generate a word cloud
        wordcloud(sephora_word_freq$term,sephora_word_freq$num,max.words = 100, colors = ("purple","pink","blue","red"),scale=c(3,0.2))


Here is the word cloud:

![Picture](/assets/textimages/wordcloud.png)

**Insights:**

From the plot and word cloud, we can draw some insights.

1) The name Sephora itself is the most important features, many reviewers mentioned Sephora in their reviews. Order also considered an important feature.

2) The user experience with Sephora is neutral. We don’t have any word that expresses positive or negative among those with a frequency greater than 200. But from the wordcloud we noticed some words like back and return appears quite frequent. Most of the customers might return their purchase so it might not be a good sign. Therefore, we should suggest Sephora should check and improve their online products.

#### Word clustering

Word clustering is used to identify word groups used together, based on frequency distance.This is a dimension reduction technique. It helps in grouping words into related clusters. Word clusters are visualized with dendrograms as shown below:

![Picture](/assets/textimages/dendrogram.png)


**Insights:**

We can observe some information from the dendrogram, “service” has short distance with “shipping”, and “called”. We can assume that many customer called to ask about their shipping of orders.


### Polarity
3 different modes are used, Rsentiment, Qdap, Tidy method.

The AFINN lexicon assigns words with a score that runs between -5 and 5, with negative scores indicating negative sentiment and positive scores indicating positive sentiment. The bing lexicon categorizes words into a binary category which are positive and negative categories.The NRC lexicon categorizes words into 10 categories of emotions corresponding to the first ring of the Plutchik's wheel of emotions such as anger, anticipation, disgust, fear, joy, sadness,surprise, positive, negative and trust.

| **Sentiment Lexicon** | **Level of classification** | 
| AFINN | Score from -5 to 5 | 
| Bing | Positive or Negative | 
| NRC | anger, anticipation, disgust, fear, joy, negative, positive, sadness, surprise, trust | 

The *inner_join()* function from *dplyr* package is used to identify the words which are in both the sentiment lexicon and the Sephora text document.

Before performing inner join, a tidy element of Sephora reviews is created by using the term document matrix of Sephora that we created before. Then, *get_sentiments()* function from *tidytext* R package is being used to obtain the dictionary of bing sentiments. The tibble dataset obtained from the tidy element of Sephora and the tibble dataset obtained from Bing sentiment lexicon are being inner join by using the common words between these two datasets. This means that R will perform inner join by matching the term vector in tidy element of Sephora and the word vector in bing sentiments. The code to obtain a tibble dataset which contains the term, document (number of reviews which show the word), the count (the frequency of each term) and 

**Bing sentiment lexicon**

A small section of joining Sephora reviews and the bing sentiment lexicon is shown below:

![Picture](/assets/textimages/bing.png)

**Insights:**

We can observe that the word amazing appear 17 times in 1 review of Sephora. Besides knowing the number of reviews which shows the word and the frequency of the word in the dataset, we can also know the classification of the word as positive or negative by using the bing sentiment dictionary. We also found that abusive, abysmal and accusing that appeared in the dataset are classified as negative word while the other are classified as positive word.

The top 10 words which have the highest frequency in negative and positive is shown below:

![Picture](/assets/textimages/bingbar.png)

**Insights:**

we can observe that the frequency of the positive words is slightly higher than the negative words. From the positive words, we know that more customers give positive reviews by using the word like, free and reward while giving negative reviews by using the word error, issue and bad. From here, we found that the customer like the free product and reward given by Sephora while giving negative comments on the error and issue they found when purchasing.

**NRC sentiment lexicon**

The frequency of frequent NRC sentiment lexicon used in the reviews of Sephora is shown below:

![Picture](/assets/textimages/NRCbar.png)


The 50 most commonly used words the emotions of NRC sentiment is shown below:

![Picture](/assets/textimages/NRCcloud.png)

**Insights:**

We found that the most frequent emotion that shown in the reviews of Sephora is positive, followed by negative, trust and anticipation. According to Plutchik, the combination of trust and anticipation forms a primary blend that corresponds to fatalism. The fatalism of the consumer is very important for a business as customer will believe strongly in their service and product.


### Important features when customers making an online purchase

To understand what features are most important, an iterative process using *grep()*. With Grep command, we can search plain-text data sets for lines that
match a regular expression. With Grepl () and sum () we can find in how many documents it is true that the regular expression is present.

The most significant features are identified as below:

| **Features** | **Frequency** | **Explanation** | 
| email | 153 | The speed of customer service through email service. |
| order | 320 | The order status. |
| package | 121 | The quality of the package. |
| service | 299 | The service of Sephora provided to customers. |
| shipping | 97 | Shipping process. |
| time | 244 | Time taken for the order to arrive. |

For each feature, we determine in which document it is present and determine whether the feature is dicussed with positive or negative sentiment.

The percentage of positive and negative in each feature is shown below:

![Picture](/assets/textimages/feature.png)

**Insights:**

We noticed that all features have a percentage of positive comments above
50%. The greatest satisfaction of consumers is on the time which reach about 60% of positive comments.

The total number of positive/negative sentimens for each feature is shown below:

![Picture](/assets/textimages/feature2.png)

**Insights:**

The best ratio between the total number of words/positive words lies with the order and time, which we can certainly identify as two elements of the brand's strength.

Using the AFFIN lexicon, a treemap is generated to see which feature has the highest score as shown below:

![Picture](/assets/textimages/feature3.png)

**Insights:**

The feature with best positive score is Time followed by Order. The
worst is Email. (the darker green components are those with the highest score). Notice that Email has a lower score. Even predominance of terms related to these elements is positive, it can be a warning signal.

Therefore, it is necessary to inspect the main problems related to Email. By selecting only the negative words for each feature. A facet bar chart is plot with negative words for each feature as shown below:

![Picture](/assets/textimages/feature4.png)


**Insights:**

We see that the temporal component is fundamentally the value of the final experience. Among the negative words most associated with email, we find "error" and "problem" occur most with email. Sephora should improve their email service as it always happens to have error for customers.

There are many “wrong” words appears for shipping and service. Sephora should therefore be aware of the product shipping and need to check their department of shipping to identify why wrong order always happens. It is also necessary to increase the quality of the service with greater precision. we need to reduce the frequency of words like “error” ,“wrong”, “bad”, and “worst”.

### Comparison between Sephora and Ulta

By joining the Sephora and Ulta reviews with the AFFIN lexicon, a density plot of AFFIN scores of the Sephora and Ulta is as shown below:

![Picture](/assets/textimages/density.png)

**Insights:**

From the density plot, we observe there is a clear difference in the AFINN score of the two brands. We can observe that the distribution of Sephora’s AFINN score is similar for positive and negative values. However, the distribution of Ulta’s AFINN score is more towards the positive score. This can be shown by the area under the density plot of Ulta where the area for the positive values is bigger than the area for the negative values. When we compare the density plot of Sephora and Ulta, we also found that the reviews of Sephora contains more negative scores as compared to Ulta. This can be shown by the bigger area under the density line of Sephora in the negative values range as compared to Ulta. When comparing the distribution of AFINN positive scores between Sephora and Ulta, we also observe that the reviews of Ulta contains more positive scores as compared to Sephora. This can be shown by the bigger area under the density line of Ulta in the positive values range as compared to Sephora. From here, we can conclude that the value of the user experience appears to be significantly higher for the Ulta brand as compared to Sephora brand.


A radar chart is plotted to display the amount of word for several groups of emotion defined in NRC sentiment lexicon. The variation of different emotions found in the reviews of Sephora and Ulta can be shown clearly in radar chart below and thus make the comparison of the sentiment analysis between Sephora and Ulta become efficient and easy.

![Picture](/assets/textimages/radar.png)

**Insights:**

We observe that the customer’s emotion towards both Sephora and Ulta has the similar distribution in radar chart. Both Sephora’s radar and Ulta’s radar have a peak at trust, anticipation and joy with anticipation at the highest peak,followed by trust and joy. However, Sephora obtain a higher value at both trust and anticipation as compared to Ulta. On the other hand, Sephora and Ulta obtain almost the same value for the
words with joy and surprise emotion. Although Sephora obtain a higher value for positive words, we observe that the value of negative words with disgust and fear emotion are higher as compared to Ulta. Both Sephora and Ulta have almost the same value for the sad word.

The frequency of the appearance of common words in Sephora and Ulta is compared by plotting a pyramid plot as shown below:

![Picture](/assets/textimages/pyramid.png)

**Insights:**

We observe that both Sephora and Ulta obtain the highest frequency for the word order. However, Sephora obtain a higher frequency for the order
word as compared to Ulta. Besides, we also found some interesting keyword in the datasets such as variety, error, gift and samples. We found that the word variety and the word gift mentioned more frequent in Ulta reviews as compared to Sephora. This probably because Ulta marketing strategy is providing a variety of product and abundant gift to attract customers. The word samples has a high frequency as compared to the word gift in Sephora instead show that Sephora more focus on giving samples as compared to gift. Besides, the error word is mentioned significant frequent in Sephora’s reviews as compared to Ulta showed that more customers complain about the errors that have made by Sephora as compared to Ulta.

A comparison cloud between Sephora and Ulta is generated to show the disjunction between the two corpora.

![Picture](/assets/textimages/comparecloud.png)

**Insights:**

We can visualize immediately that the marketing strategy of Ultra is more focusing on providing variety of products and giving gift while the marketing strategy of Sephora is more focus on providing samples to the customer. Besides, we also found that the presence of positive words such as right and like is much greater for Ulta brand as compared to Sephora. 


### Conclusion
- There are more positive perceptions as compared to negative perception to Sephora brand.

- Most of the customers satisfied with the free product, samples and reward given by Sephora.This can be proven by the high positive perceptions shown by Sephora’s reviews from the result of three sentiment analysis including Rsentiment,Qdap and the Tidy method.

- Many customers provide the negative feedback about the error and issues they face when purchasing Sephora products. From this result, we can say that Sephora can increase their free product through promotion, testing samples and point rewards to customers to attract more customers. 

- Sephora also required to reduce the error and issues faced by customers through online chat box to have a close contact and solve the customer’s problem efficiently.

- After evaluating the positive and negative reviews provided by the customer through polarity analysis, it is found that there are more positive words as compared to negative words in Sephora reviews. 

- Most of the positive words mentioned by the customer are free, reward, beauty, support and refund while most of the negative words mentioned by customer are error, issue, bad and wrong.

- From the dendrogram, we also found that customers more focus on Sephora’s service and shipping. 

- From the facet bar chart of feature sentiments, we also found that “wrong” appeared frequently with shipping and service. From this result, we can say that Sephora
required to reduce their wrong shipping and services to reduce the negative perceptions given by the customer and remain competitive in beauty industry.