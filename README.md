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

**Flan-T5** model, a LLM developed by Google to handle text2text generation tasks, such as translation and summarization was chosen for this task. The model size is 248M parameters. In order to perform required classification task on news, tuning is needed for this model. The following methods were implemented:
* [Prompt Engineering / In-Context Learning](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/blob/33746f5045a6ece519ab6f5e0d9205aeb82a640d/notebook/02_News_Classification_with_Prompt_Engineering_Flan_T5.ipynb)
* Fine-tuning the model with manual labels

## 3. Data Collection

**1. NewsAPI**
   
   All news related to either 'Tesla' or 'Elon Musk' were extracted using NewsAPI and here are the number of news by publisher.
   
![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/ddf4bed1-819d-483c-8a30-154a24a9395e)

**2. Tesla Stock Price**

   Open price, close price and other properties about Tesla stock were extracted using MarketstackAPI. The open price and close price are plotted below.
   
![image](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/assets/130683390/4cc82822-dfbd-415c-8cce-8315317c9ba0)

[*Data Collection Notebook*](https://github.com/wangtuguahhh/Sentiment-Analysis-for-Investment-Strategies-on-Tesla-Stock/blob/33746f5045a6ece519ab6f5e0d9205aeb82a640d/notebook/01_Data_Collecting_and_EDA.ipynb)

## 4. News Classification

## 5. Sentiment Analysis and Stock Price Correlation

## 6. Future Improvements
