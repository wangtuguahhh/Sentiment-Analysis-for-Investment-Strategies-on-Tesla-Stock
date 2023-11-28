![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/dc0161c2-98d2-4d79-b945-afc454476406)
# News Analysis for Potential Investment Strategies on Tesla Stock

Investment strategy on certain stocks is a challenging topic and of keen interest to a wide audience. The stock market, with its inherent complexity and unpredictability, presents a fascinating area for machine learning exploration. Its difficulty in prediction stems from a multitude of factors including economic indicators, global events, investor behavior, and market speculation, all of which contribute to its volatile nature.

Leveraging advanced Large Language Models (LLMs) presents an innovative approach to this task. LLMs enable the rapid processing and interpretation of vast amounts of news information. My goal in this project is to conduct a thorough sentiment analysis of news related to Tesla in a short period of time, aiming to uncover any potential correlations between public sentiment in the news and fluctuations in Tesla's stock price. This endeavor not only poses a significant challenge but also holds the promise of offering novel insights into the complicated dynamics between media sentiment and stock market movements.

[Streamit Interactive Dashboard](https://streamitteslaproject-cv6tnge82hewwmf6cttecw.streamlit.app/)

## 1. Data
APIs are very convinent way to collect required data in this task. The following APIs are used:
* [NewsAPI](https://newsapi.org/) (limited to past 30 days)
* [marketstack](https://marketstack.com/)

In this task, data from the past **30 days** are evaluated. The time period is from **2023-09-12** to **2023-10-11**.

## 2. Method
After collecting the news, some manual labeling were performed for 60% of the news articles. Some articles were found not directly related to Tesla but related to Elon Musk's personal life, his political opinions and his other companies. Therefore, a classification tool is required to tell if a news is directly related to Tesla or not.

✨ **Flan-T5** model, a LLM developed by Google to handle text2text generation tasks, such as translation and summarization was chosen for this task. The model size is 248M parameters. In order to perform required classification task on news, tuning is needed for this model. The following methods were implemented:
* [Prompt Engineering / In-Context Learning for LLM](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/blob/33746f5045a6ece519ab6f5e0d9205aeb82a640d/notebook/02_News_Classification_with_Prompt_Engineering_Flan_T5.ipynb)
* [Parameter Efficient Fine-tuning (LoRA) for LLM](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/blob/3ce1a95e5cb45823cb890ef99e2ad34e738536b5/notebook/03_News_Classification_with_PEFT_LoRA_Flan_T5.ipynb) (not working well due to limited number of data)

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

[*Sentiment Analysis Notebook*](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/blob/b90d39920a1bb735de8080f88d32b48ba7d37a3a/notebook/04_Sentiment_Analysis_with_FinBERT.ipynb)

## 6. Correlation with Stock Price
From Pearson Correlation calculations, the correlation coefficient between open price and positive news number is large, around 0.68. Here is the plot of trends of those two values normalized.

![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/0ac53ece-a1af-4b9e-894f-56f6b054ace9)

There were some similaries between the two trend lines. Next will shift the positive numbers number by 1 day to check its impact on next day's open price.

![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/598a5543-496e-434d-9950-411e4b103aa2)

There is no clear correlation between today's open price with yesterday's number of positive news. Next let's investigate the impact from both positive and negative news on Tesla stock price change.

![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/8a8bec44-e0b0-43fc-91f1-3b7449e5c7d3)
![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/e60028f2-f32c-45b2-af72-1d617dd1ba47)

* The stock price movements correlated with the previous-day news sentiment the best though quite some mis-match.
* News accumulated from more than 1 days were also evaluated. however, the impact from negative sentiments was exaggerated if averaging impact from previous days.  
* Ignoring the neutral news and relying on the raw difference between positive and negative didn't correlate well with the stock price movements.

[*Stock Price Correlation Notebook*](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/blob/b90d39920a1bb735de8080f88d32b48ba7d37a3a/notebook/04_Sentiment_Analysis_with_FinBERT.ipynb)

## 7. Future Improvements
🔭 **Observations:**
* With help of prompt engineering and fine-tuning, we can facilitate open-source LLMs to work on a particular task of interest.
    * **In this work, we imporve Flan-T5 model performance on classifying if a news is related to Tesla or not using one-shot learning method.**
* Most of the news in the past 30 days were neutral to Tesla.
* Ignoring the neutral news, there were more negative news for Tesla in the past 30 days compared to positive ones.
* There is no clear strong correlation between the news sentiments and the stock market price movements.

🧰 **Limitations:**
* The news classification is not perfect. The around was around 0.85 for the Flan-T5 model with in-context learning technique applied.

* The sentiment analysis from FinBERT model is not appropriate.
    * Comparing to human labels, FinBERT model tended to label positive news as neutral ones.
    * FinBERT model was pretrained using finance corpus for sentiment analysis based on BERT.
    * The manual labeling paid more attention to potential impact on Tesla stock price, which is challenging to FinBERT.
        * For example, there were news articles on other EV makers adapting to the battery charging system developed by Tesla. I labeled such news as positive since the adaption change from other EV makers could bring potential developments and more market shares of Tesla in the EV charging system section. However, FinBERT treated those as neutral news.
    * **The NLP taks here is no longer simple sentiment analysis on a piece of context. It required us to generate a tool to predict the potential impact of news on Tesla's stock price with help from LLMs.**

* There are multiple and complex factors that will impact the stock price of a company. 
    * Market-related facors such as GDP, interest rates, inflation, employment rate, indicate how well the whole market is performing and they will have a signifcant impact on individual stocks. 
    * Company reports showing how the company performed in the past period of time is importance to its stock price. Earnings, dividens, debts, management qualities and etc. are key components to drive the stock price movement. 
    * Other factors such as global economy, regulations and goverment policies, industry-specific changes and natural disasters and pandemics all play a role here.

🍀 **Future Work:**
* Collecting data for a longer period of time:
    * then can try fine-tuning LLMs for classification and sentiment analysis
    * to see if there is any long-term trend between the news sentiments and the stock price movement.
* Tuning LLMs with generative capabilities, such as Flan-T5 to achieve the task of predicting the impact of a news on Tesla's stock price.
    * try ICL first
    * try fine-tuning if more data are available
* Incorporate other factors mentioned in the not considered section.
* Create a Tesla stock investigation AI tool for user.
