---
layout: default
title: 뉴스 기사 Clustering
parent: Machine Learning 응용
nav_order: 2
--- 

# 뉴스 기사 Clustering
{: .no_toc }
<br/>

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }


1. TOC
{:toc}
</details>
---

## 뉴스 기사 Clustering
- 뉴스 기사의 내용을 바탕으로 유사한 기사끼리 묶어보려고 함

```python
# 사용할 library를 먼저 모두 import
import pandas as pd
import konlpy
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.cluster import AgglomerativeClustering
```


### 뉴스 기사 준비
- 우선, Clustering할 기사 데이터를 수집
- [다음 뉴스](https://news.daum.net/breakingnews/politics/assembly){: target="_blank"}에서, '국회/정당', '금융', '건강', '사건/사고' 4개 토픽의 기사를 조금씩 수집해 옴

```python
## '다음 뉴스'에서 직접 수집해 온 데이터를 dataframe으로 정리해 둠
news_df.head()
```

<div class="code-example" markdown="1">

|    | topic     | article_title    | news_text        |
|---:|:----------|:-----------------|:-----------------|
|  0 | 국회/정당 | [포토]재보선 D-1, '시민들의 선택은?' | [국회사진취재단] 6일 오후 서대문구 홍제역에서 열린 박영선 더불어민주당 서울시장... |
|  1 | 국회/정당 | "약속 지킨다"..김종인, 8일 국민의힘 떠난다  | 김종인 국민의힘 비상대책위원장이 5일 오후 서울 관악구 서울대입구역에서 오세훈 서울... |
|  2 | 국회/정당 | "위선·무능 정부 심판해야"..빨강 운동화에 '골목' 찾은 오세훈 | 오세훈 국민의힘 서울시장 후보가 6일 서울 성북구 정릉시장에서 만난 한 시민에게 허... |
|  3 | 국회/정당 | 김종인, 강남 대치역 찾아 오세훈 후보 지지호소   | (서울=뉴스1) 국회사진취재단 = 김종인 국민의힘 비상대책위원장이 6일 오후 서울...   |
|  4 | 국회/정당 | 이낙연 "특권층 득세하고 차별하는 서울로 퇴보할 텐가"  | [이데일리 이정현 기자] 이낙연 더불어민주당 상임선대위원장이 6일 오세훈 국민의힘..   |

</div>

&nbsp;
{: .fs-1 .lh-0}

```python
news_df.groupby('topic')[['article_title']].count()
```

<div class="code-example" markdown="1">

| topic     |   article_title |
|:----------|----------------:|
| 건강      |              16 |
| 국회/정당 |              13 |
| 금융      |              15 |
| 사건/사고 |              15 |

</div>

### tokenize & 명사만 저장
- 기사의 주제를 판단하는 데에 명사가 가장 중요하다고 생각해 명사만 사용

```python
documents_processed = []

for text in news_df['news_text']:
    okt = konlpy.tag.Okt() 
    okt_pos = okt.pos(text)

    words = []
    for word, pos in okt_pos:
        if 'Noun' in pos:
            words.append(word)

    documents_processed.append(' '.join(words))

print(len(documents_processed))
documents_processed[0]
```
```
59
'국회 진취 재단 오후 서대문구 홍제역 박영선 민주당 서울시장 후보 집중 유세 시민 휴대폰 관심 표명 노진환'
```

### Text Vector화 & 학습
```python
tfidf_vectorizer = TfidfVectorizer(min_df=1, ngram_range=(1,1))

tfidf_vector = tfidf_vectorizer.fit_transform(documents_processed)

tfidf_dense = tfidf_vector.todense()   ## sparse array로는 Agg.Clustering 학습이 안되어서, dense array로 바꿔줘야 함
```

*[TfidfVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html){: target="_blank"}: 각 단어의 Term Frequency - Inverse Document Frequency를 기반으로 text를 vector화.  
- ngram_range: (1, 1) means only unigrams, (1, 2) means unigrams and bigrams, and (2, 2) means only bigrams. (default: (1,1))
- min_df: ignore terms that have a document frequency strictly lower than the given threshold. (default: 1)



→ AgglomerativeClustering으로 학습

```python
model = AgglomerativeClustering(linkage='complete', affinity='cosine', n_clusters=4)
model.fit(tfidf_dense)
model.labels_
```
```
array([3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1,
       0, 1, 1, 1, 1, 1, 2, 0, 1, 2, 0, 2, 2, 2, 0, 2, 0, 2, 0, 2, 2, 2,
       0, 0, 0, 0, 0, 0, 0, 3, 0, 0, 0, 0, 0, 0, 3], dtype=int64)
```

### 학습 결과 확인
- 원래 daum에서 분류되어 있던 주제와 비교해서, 유사한 기사끼리 잘 묶였나 확인


```python
# topic, article_title에 cluster를 붙여서 확인

cluster_df = pd.DataFrame(zip(news_df['topic'], news_df['article_title'], model.labels_), columns=['topic', 'title', 'cluster'])
cluster_df.head()
```

<div class="code-example" markdown="1">

|    | topic     | title                                                       |   cluster |
|---:|:----------|:------------------------------------------------------------|----------:|
|  0 | 국회/정당 | [포토]재보선 D-1, '시민들의 선택은?'                        |         3 |
|  1 | 국회/정당 | "약속 지킨다"..김종인, 8일 국민의힘 떠난다                  |         3 |
|  2 | 국회/정당 | "위선·무능 정부 심판해야"..빨강 운동화에 '골목' 찾은 오세훈 |         3 |
|  3 | 국회/정당 | 김종인, 강남 대치역 찾아 오세훈 후보 지지호소               |         3 |
|  4 | 국회/정당 | 이낙연 "특권층 득세하고 차별하는 서울로 퇴보할 텐가"        |         3 |

</div>

→ 각 topic별 cluster 배정 현황을 확인:

```python
cluster_df.groupby(['topic', 'cluster'])[['cluster']].count()
```

<div class="code-example" markdown="1">

|  topic  |   cluster    |   count |
|------:|-------:|----------:|
| 건강  | 0      |         5 |
| ^^    | 1      |         1 |
| ^^    | 2      |        10 |
| 국회/정당  | 1 |         2 |
| ^^  |  3  |        11 |
| 금융   | 0     |         2 |
| ^^  |  1     |        13 |
| 사건/사고  | 0 |        13 |
| ^^    |   3 |         2 |

</div>

- 주로 '건강': 2, '국회/정당': 3, '금융': 1, '사건/사고': 0으로 많이 배정되었음을 확인


→ daum의 분류와 다른 부분을 확인:

```python
cluster_df[cluster_df['topic'] == '국회/정당']
```

<div class="code-example" markdown="1">

|    | topic     | title                                                        |   cluster |
|---:|:----------|:-------------------------------------------------------------|----------:|
|  0 | 국회/정당 | [포토]재보선 D-1, '시민들의 선택은?'                         |         3 |
|  1 | 국회/정당 | "약속 지킨다"..김종인, 8일 국민의힘 떠난다                   |         3 |
|  2 | 국회/정당 | "위선·무능 정부 심판해야"..빨강 운동화에 '골목' 찾은 오세훈  |         3 |
|  3 | 국회/정당 | 김종인, 강남 대치역 찾아 오세훈 후보 지지호소                |         3 |
|  4 | 국회/정당 | 이낙연 "특권층 득세하고 차별하는 서울로 퇴보할 텐가"         |         3 |
|  5 | 국회/정당 | 시민들과 기념사진 찍는 김종인 비대위원장                     |         3 |
|  6 | 국회/정당 | 강남 찾아 오세훈 지지호소하는 김종인                         |         3 |
|  7 | 국회/정당 | 김종인, 오세훈 후보 지원유세                                 |         3 |
|  8 | 국회/정당 | 지원유세 마치고 차량에 오른 김종인                           |         3 |
|  9 | 국회/정당 | 오세훈 후보 지원유세하는 김종인                              |         3 |
| 10 | 국회/정당 | 내 선거'처럼 뛴 안철수는 국민의힘에서 무엇이 될까            |         3 |
| 11 | 국회/정당 | 웹툰에서도 부딪히는 네이버 vs 카카오..'원조 국밥집' 논란도?  |         1 |
| 12 | 국회/정당 | 토스, 매출 230% 급증.."출범 후 처음으로 매출 이익 동시 개선" |         1 |

</div>

- topic 간 경계가 모호한 기사의 경우 daum에서 배정한 topic과 다른 cluster로 묶인 것...




