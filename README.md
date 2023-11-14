![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/dc0161c2-98d2-4d79-b945-afc454476406)
# Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock

Investiment strategy on certain stocks is a challenging topic and of keen interest to a wide audience. The stock market, with its inherent complexity and unpredictability, presents a fascinating area for machine learning exploration. Its difficulty in prediction stems from a multitude of factors including economic indicators, global events, investor behavior, and market speculation, all of which contribute to its volatile nature.

Leveraging advanced Large Language Models (LLMs) presents an innovative approach to this task. LLMs enable the rapid processing and interpretation of vast amounts of news information. My goal in this project is to conduct a thorough sentiment analysis of news related to Tesla in a short period of time, aiming to uncover any potential correlations between public sentiment in the news and fluctuations in Tesla's stock price. This endeavor not only poses a significant challenge but also holds the promise of offering novel insights into the complicated dynamics between media sentiment and stock market movements.

## 1. Data
APIs are very convinent way to collect required data in this task. The following APIs are used:
* [NewsAPI](https://newsapi.org/) (limited to past 30 days)
* [marketstack](https://marketstack.com/)

In this task, data from the past **30 days** are evaluated. The time period is from **2023-10-11** to **2023-9-12**.

## 2. Method
After collecting the news, some manual labeling were performed for 60% of the news articles. Some articles were found not directly related to Tesla but related to Elon Musk's personal life, his political opinions and his other companies. Therefore, a classification tool is required to tell if a news is directly related to Tesla or not.

✨ **Flan-T5** model, a LLM developed by Google to handle text2text generation tasks, such as translation and summarization was chosen for this task. The model size is 248M parameters. In order to perform required classification task on news, tuning is needed for this model. The following methods were implemented:
* [Prompt Engineering / In-Context Learning](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/blob/33746f5045a6ece519ab6f5e0d9205aeb82a640d/notebook/02_News_Classification_with_Prompt_Engineering_Flan_T5.ipynb)
* Fine-tuning the model with manual labels

✨ **FinBERT** model, a LLM developed for financial sentiment analysis with BERT, was used for sentiment analysis of news related to Tesla

## 3. Data Collection

**1. NewsAPI**
   
   All news related to either 'Tesla' or 'Elon Musk' were extracted using NewsAPI and here are the number of news by publisher.
   
![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/ddf4bed1-819d-483c-8a30-154a24a9395e)

**2. Tesla Stock Price**

   Open price, close price and other properties about Tesla stock were extracted using MarketstackAPI. The open price and close price are plotted below.
   
![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/4cc82822-dfbd-415c-8cce-8315317c9ba0)

[*Data Collection Notebook*](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/blob/33746f5045a6ece519ab6f5e0d9205aeb82a640d/notebook/01_Data_Collecting_and_EDA.ipynb)

## 4. News Classification

In-context learning was investigated to leverage Flan-T5 model to classify if a news is related to Tesla or not. BERT model was also evaluated but ICL failed to work for it. ICL worked for LLMs with generative capabilites, such as Flan-T5. The following methods were tested:
* Zero-shot learning
* One-shot learning
* Few-shot learning (2, 3, 4, 5 shots)

Here is the classification report for one-shot learning.

![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/9f5a74d6-d187-446b-acec-9d7fac05dea1)

Here is the classification report for few-shot learning (2 shots):

![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/23e63d40-2dc7-4cbd-b6cb-d73637e4cf0d)

**One-shot learning provided the best performance with an average accuracy of 0.85. Including more examples in the prompt (few-shot learning) didn't improve the overall accuracy.** 

More examples in the prompt improved precision for 'yes' and recall for 'no'. Few-shot learning suffered from misclassifying true 'yes' as 'no'. 

Here is the distribution of Tesla-related news and non-Tesla-related news from one-shot learning.

![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/79312080-cd1b-4715-84cd-a2911ae4a7f0)

[*News Classification Notebook*](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/blob/2a77bf68c85d0481bb3a82af48f8c2614aab3c9c/notebook/02_News_Classification_with_Prompt_Engineering_Flan_T5.ipynb)

## 5. Sentiment Analysis
### 5.1. FinBERT Sentiment Analysis Evaluation
The FinBERT model sentiment results were compared with manual labels for the 60% of the news data. Here is the confusion matrix for news directly related to Tesla. The FinBERT model tended to label positive news as neutral ones assuming the manual labels were true labels.

![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/dab52eb8-5c7c-4911-b68f-7887f0ac5ba9)

### 5.2. Sentiment Distribution
Sentiment distributions for all data using FinBERT model.

![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/9b9e8854-4311-4570-b7c4-5ddf8e338c36)

* The majority news were neutral, which could be suprising given the common assumption that media outlets prefer to report negative news.
* There were more negative ones than positive ones.
  * This could be true.
  * Or related to FinBERT model's tendency to label positive Tesla-related news as neutral ones.

### 5.3. Sentiment Polarity of Publishers
Positive news rate - Negative news rate was chosen to illustrate sentiment polarity of each publisher. Here is the distribution.

![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/526bbe7d-c39d-4348-b0c1-7443aedb8554)

Here is the plot with individual positive rates and negative rates.

![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/fe2cb1b1-45fd-4a08-bc3d-c0e316e170f6)

* The majority publishers are more on neutral side.
* There are some publishers tended to report more negative Tesla news, such as NBC News, The Irish Times, Politico, , The Washington Post and Breitbart News.
  * Some the publishers above are big names, such as NBC News and The Washington Post that could have a larger impact on public opinion on how Tesla is doing.
* There are some publishers tended to report more positive Tesla news, such as Next Big Future, Engadget, The Times of India, TechRadar, The Jerusalem Post and BBC News.
  * This trend suggests that technology-focused publishers generally maintain a positive stance towards Tesla.
  * Major publishers from specific countries appeared to be more positive in their coverage of Tesla, which could indicate a favorable disposition towards providing Tesla with better opportunities.

[Sentiment Analysis Notebook](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/blob/2a77bf68c85d0481bb3a82af48f8c2614aab3c9c/notebook/03_Sentiment_Analysis_with_FinBERT.ipynb)

## Correlation with Stock Price

## 6. Future Improvements
