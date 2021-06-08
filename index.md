---
layout: default
title: Home
nav_order: 1
description: "data analytics website by chaelist."
permalink: /
---

# Chaelist
{: .fs-9 .fw-300 }

Data Analytics Blog
{: .fs-5 .fw-300 }

[Email](mailto:chaeyun1248@gmail.com){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2  target="_blank"}   [LinkedIn](https://www.linkedin.com/in/chaeyun-chung-2b946b171/){: .btn .fs-5 .mb-4 .mb-md-0  target="_blank"}


<br/>

[Frequency Analysis (빈도 분석)]({{ site.baseurl }}{% link docs/text_analysis/korean_text.md %}){: .fs-7 .fw-300 .text-grey-dk-000}

뉴스 기사를 수집해 전처리를 거쳐 단어별 빈도를 파악 & 기사의 내용을 워드클라우드로 압축적으로 표현

![WordCloud](../../../assets/images/text_korean/wordcloud2.png){: width="200"} 

<br/>

[Harry Potter Network Analysis (인물 네트워크 분석)]({{ site.baseurl }}{% link docs/network_analysis/social_network.md %}){: .fs-7 .fw-300 .text-grey-dk-000}

소설 \<Harry Potter> 시리즈 속 인물들 간 연결 관계 및 권별 인물의 중요도 변화를 분석

![HP1_Graph](../../../assets/images/social_network/hp1_network2.png){: width="200"}  &nbsp; ![HP_Evolution_of_Top_Characters](../../../assets/images/social_network/hp_evolution4.png){: width="300"} 


<br/>

[Sementic Network Analysis (언어 네트워크 분석)]({{ site.baseurl }}{% link docs/network_analysis/semantic_network.md %}){: .fs-7 .fw-300 .text-grey-dk-000}

뉴스 기사 속 주요 단어들 간의 연결 관계를 파악해, 기사의 핵심 내용을 유추

![Korean_SNA](../../../assets/images/semantic_network/korean_sna2.png){: width="250"}

<br/>

[Movie Review Sentiment Analysis (영화 리뷰 감성 분석)]({{ site.baseurl }}{% link docs/ml_application/sentiment_analysis.md %}){: .fs-7 .fw-300 	.text-grey-dk-000}

영화 100개의 평점-리뷰 데이터를 수집해, 리뷰의 감성(긍정/부정)을 예측하는 모델을 구축


<br/>

[News Clustering (뉴스 기사 군집화)]({{ site.baseurl }}{% link docs/ml_application/news_clustering.md %}){: .fs-7 .fw-300 .text-grey-dk-000}

서로 다른 토픽의 뉴스 기사들을 Clustering을 통해 유사한 기사들끼리 묶어줌

<br/>

[Time Series Data Forecasting (시계열 데이터 예측)]({{ site.baseurl }}{% link docs/ml_application/time_series.md %}){: .fs-7 .fw-300 .text-grey-dk-000}

Facebook의 Prophet 라이브러리를 활용해 시계열 데이터를 예측 & 파라미터 튜닝

![Forecast_Real_Comparison](../../../assets/images/ml_applied/real_forecast_comparison.png){: width="300"}  &nbsp; ![Changepoint1](../../../assets/images/ml_applied/changepoint1.png){: width="250"}

<br/>

[Topic Modeling (토픽모델링)]({{ site.baseurl }}{% link docs/ml_application/topic_modeling.md %}){: .fs-7 .fw-300 .text-grey-dk-000}

Google Play Store의 'Netflix' 앱 리뷰 데이터를 활용해 잠재 디리클레 할당(Latent Dirichlet Allocation, LDA) 기법으로 토픽 모델링 구현

![TopicModeling](../../../assets/images/ml_applied/TopicModeling.png){: width="400"}