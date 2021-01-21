---
layout: default
title: 빈도 분석 (English)
parent: Text 분석
nav_order: 1
--- 

# 빈도 분석 (English)
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

## nltk 준비 & text 수집

***nltk**: natural language tool kit (English text anlaysis에 필요한 python module)

- `pip install nltk`로 설치
- 처음 사용할 때는 아래와 같이 다운받아야 사용 가능
```python
import nltk
nltk.download('all')   ## 한 컴퓨터에 한 번만 해두면 된다
```

*Frequency Analysis에 사용할 text 수집해오기
```python
import requests
from bs4 import BeautifulSoup

url = 'https://www.nytimes.com/2017/06/12/well/live/having-friends-is-good-for-you.html'

r = requests.get(url)
soup = BeautifulSoup(r.text, 'lxml')

title = soup.title.text.strip()
text_list = soup.select('p.css-axufdj')
content = ''
for text in text_list:
    content = content +' '+ text.text.strip()

print(title, '\n')
print(content)
```
```
Social Interaction Is Critical for Mental and Physical Health - The New York Times 

 Hurray for the HotBlack Coffee cafe in Toronto for declining to offer Wi-Fi to its customers. There are other such cafes, to be sure, including seven of the eight New York City locations of Café Grumpy. But it’s HotBlack’s reason for the electronic blackout that is cause for hosannas. As its president, Jimson Bienenstock, explained, his aim is to get customers to talk with one another instead of being buried in their portable devices. “It’s about creating a social vibe,” he told a New York Times reporter. “We’re a vehicle for human interaction, otherwise it’s just a commodity.” What a novel idea! Perhaps Mr. Bienenstock instinctively knows what medical science has been increasingly demonstrating for decades: Social interaction is a critically important contributor to good health and longevity. Personally, I don’t need research-based evidence to appreciate... (생략)
```

## 전처리 (Preprocessing)

&nbsp;
{: .fs-1 .lh-0}

*전처리 과정 (English):
- 불필요한 기호 / 표현 없애기(예, !, ., “, ; 등)
- 대소문자 변환 (Case conversion, 소문자 ↔ 대문자)
- 단어 (혹은 Token) 단위로 짤라주기 (Tokenization)
- 단어의 품사 찾기 (Part of Speech tagging)
- 원하는 품사의 단어들만 선택
- 단어의 원형(혹은 어근) 찾기(Lemmatization / Stemming)
- 불용어 (Stopwords) 제거

필요에 따라서는 위 과정의 순서가 바뀔수도 있고, 같은 과정을 반복 수행할 수도 있다

### Text Cleaning
: 불필요한 symbol / mark 등 제거
- `.replace()` 혹은 정규식 활용
- ※ 분석 목적에 따라, '어떠한 mark 나 symbol 들을 이 단계에서 제거할 것인가?'를 고민하는 것이 중요함
    - ex) 텍스트 분석이 문장 단위로 이루어지는 경우, 문장의 끝을 나타내는 기호들(마침표(.) 물음표(?) 느낌표(!) 등)을 없애지 않아야 한다

```python
# 불필요한 기호 없애기 -  정규식 사용
import re

filtered_content = re.sub('[^,.?!\w\s]','', content)  ## ,.?!와 문자+숫자+_(\w)와 공백(\s)만 남김
filtered_content
```
```
Hurray for the HotBlack Coffee cafe in Toronto for declining to offer WiFi to its customers. There are other such cafes, to be sure, including seven of the eight New York City locations of Café Grumpy. But its HotBlacks reason for the electronic blackout that is cause for hosannas. As its president, Jimson Bienenstock, explained, his aim is to get customers to talk with one another instead of being buried in their portable devices. Its about creating a social vibe, he told a New York Times reporter. Were a vehicle for human interaction, otherwise its just a commodity. What a novel idea! Perhaps Mr. Bienenstock instinctively knows what medical science has been increasingly demonstrating for decades Social interaction is a critically important contributor to good health and longevity. Personally, I dont need researchbased evidence to appreciate the value of making and maintaining social connections. I experience it daily during my morning walk with up to three women, then... (생략)
```

+) 아래와 같이 replace를 사용해서 제거할 수도 있다
```python
filtered_content = content.replace('[','').replace(']','').replace('"','')
filtered_content = filtered_content.replace('“','').replace('”','').replace('’',"")

# +) 아래와 같이 특정 text 특성상 불필요한 부분도 추가로 제거 가능
filtered_content = filtered_content.replace('The New York Times', '')
```

### Case Conversion
- 영어의 경우, python에서는 같은 단어라도 대문자와 소문자를 다르게 인식하므로 통일해줘야 한다

```python
filtered_content = filtered_content.lower()  # .lower()를 사용해서 소문자로 변환
filtered_content
```
```
 hurray for the hotblack coffee cafe in toronto for declining to offer wi-fi to its customers. there are other such cafes, to be sure, including seven of the eight new york city locations of café grumpy. but its hotblacks reason for the electronic blackout that is cause for hosannas. as its president, jimson bienenstock, explained, his aim is to get customers to talk with one another instead of being buried in their portable devices. its about creating a social vibe, he told a new york times reporter. were a vehicle for human interaction, otherwise its just a commodity. what a novel idea! perhaps mr. bienenstock instinctively knows what medical science has been increasingly demonstrating for decades: social interaction is a critically important contributor to good health and longevity. personally, i dont need research-based evidence to appreciate the value of making and maintaining social connections. i experience it daily during my morning walk with up to three women, then... (생략)
```

### Tokenization
- Token: 뜻을 갖고 사용될 수 있는 가장 작은 글의 단위. 보통 하나의 단어(word)라고 생각하면 된다
- Tokenization:토큰(단어) 단위로 잘라주는 것

*두 가지 방법이 가능:
- `nltk.word_tokenize()` 함수 사용
- `.split()` 함수로 whitespace 기준으로 text split (영어의 경우 split()으로만 잘라줘도 단어 단위로 잘 잘림)

```python
# nltk.word_tokenize() 함수를 사용해서 tokenize

import nltk  # import해서 사용

word_tokens = nltk.word_tokenize(filtered_content)
print(word_tokens)    ## 단어의 list로 나옴
```
```
['hurray', 'for', 'the', 'hotblack', 'coffee', 'cafe', 'in', 'toronto', 'for', 'declining', 'to', 'offer', 'wi-fi', 'to', 'its', 'customers', '.', 'there', 'are', 'other', 'such', 'cafes', ',', 'to', 'be', 'sure', ',', 'including', 'seven', 'of', 'the', 'eight', 'new', 'york', 'city', 'locations', 'of', 'café', 'grumpy', '.', 'but', 'its', 'hotblacks', 'reason', 'for', 'the', 'electronic', 'blackout', 'that', 'is', 'cause', 'for', 'hosannas', '.', 'as', 'its', 'president', ',', 'jimson', 'bienenstock', ',', 'explained', ',', 'his', 'aim', 'is', 'to', 'get', 'customers', 'to', 'talk', 'with', 'one', 'another', 'instead', 'of', 'being', 'buried', 'in', 'their', 'portable', 'devices', '.', 'its', 'about', 'creating', 'a', 'social', 'vibe', ',', 'he', 'told', 'a', 'new', 'york', 'times', 'reporter', '.', 'were', 'a', 'vehicle', 'for', 'human', 'interaction', ',', 'otherwise', 'its', 'just', 'a', 'commodity', '.', 'what', 'a', 'novel', 'idea', (생략)]
```

### POS tagging 
: (=Parts-of-Speech tagging) 각 단어의 품사를 찾아서 태깅. (ex. 'this': pronoun(대명사), 'class': noun, 'is': be동사, 'a': article(관사), ...)
- tag된 품사의 종류 해석하는 법:  https://www.ling.upenn.edu/courses/Fall_2003/ling001/penn_treebank_pos.html

```python
# nltk.pos_tag() 함수를 사용해서 품사 태깅
tokens_pos = nltk.pos_tag(word_tokens)   # pos_tag()의 입력값으로는 단어의 리스트가 들어가야 한다
print(tokens_pos)
```
```
[('hurray', 'NN'), ('for', 'IN'), ('the', 'DT'), ('hotblack', 'NN'), ('coffee', 'NN'), ('cafe', 'NN'), ('in', 'IN'), ('toronto', 'NN'), ('for', 'IN'), ('declining', 'VBG'), ('to', 'TO'), ('offer', 'VB'), ('wi-fi', 'JJ'), ('to', 'TO'), ('its', 'PRP$'), ('customers', 'NNS'), ('.', '.'), ('there', 'EX'), ('are', 'VBP'), ('other', 'JJ'), ('such', 'JJ'), ('cafes', 'NNS'), (',', ','), ('to', 'TO'), ('be', 'VB'), ('sure', 'JJ'), (',', ','), ('including', 'VBG'), ('seven', 'CD'), ('of', 'IN'), ('the', 'DT'), ('eight', 'CD'), ('new', 'JJ'), ('york', 'NN'), ('city', 'NN'), ('locations', 'NNS'), ('of', 'IN'), ('café', 'NN'), ('grumpy', 'NN'), ('.', '.'), ('but', 'CC'), ('its', 'PRP$'), ('hotblacks', 'NNS'), ('reason', 'NN'), ('for', 'IN'), ('the', 'DT'), ('electronic', 'JJ'), ('blackout', 'NN'), ('that', 'WDT'), ('is', 'VBZ'), ('cause', 'NN'), ('for', 'IN'), ('hosannas', 'NN'), ('.', '.'), ('as', 'IN'), ('its', 'PRP$'), ('president', 'NN'), (생략)]
```

**+) 특정 PoS의 단어들만 추출하기**
- 보통, 주제/키워드를 분석하고자 할 때는 noun만 사용해서 분석
- 하지만 감정 분석에는 형용사/부사도 중요 → 목적에 따라 적절히 필요한 품사 선택!

```python
# 명사 단어만 추출하기
NN_words = []

for word, pos in tokens_pos:
    if 'NN' in pos:           ## Noun 종류 4개는 모두 'NN'을 포함하고 있어서 (NN, NNS, NNP, NNPS)
        NN_words.append(word)

print(NN_words)
```
```
['hurray', 'hotblack', 'coffee', 'cafe', 'toronto', 'customers', 'cafes', 'york', 'city', 'locations', 'café', 'grumpy', 'hotblacks', 'reason', 'blackout', 'cause', 'hosannas', 'president', 'jimson', 'bienenstock', 'aim', 'customers', 'devices', 'vibe', 'york', 'times', 'vehicle', 'interaction', 'commodity', 'idea', 'bienenstock', 'science', 'decades', 'interaction', 'contributor', 'health', 'longevity', 'evidence', 'value', 'connections', 'experience', 'morning', 'walk', 'women', 'swim', (생략)]
```

### Lemmatization
: lemma=원형. 단어를 원형으로 바꿔줌 (ex. is -> be, ate -> eat, methods -> method)
- 동사는 동사원형으로, 명사는 singular(단수형)으로 변환

```python
# nltk.WordNetLemmatizer() 함수 사용

wlem = nltk.WordNetLemmatizer()
lemmatized_words = []

for word in NN_words:
    new_word = wlem.lemmatize(word)
    lemmatized_words.append(new_word)

print(lemmatized_words)
```
```
['hurray', 'hotblack', 'coffee', 'cafe', 'toronto', 'customer', 'cafe', 'york', 'city', 'location', 'café', 'grumpy', 'hotblacks', 'reason', 'blackout', 'cause', 'hosanna', 'president', 'jimson', 'bienenstock', 'aim', 'customer', 'device', 'vibe', 'york', 'time', 'vehicle', 'interaction', 'commodity', 'idea', 'bienenstock', 'science', 'decade', (생략)]
```

### Stopwords Removal
: 불용어 제거. articles(a, an, the,...), pronouns(this, that,...) 등 별 의미가 없는 단어들을 제거해준다.

```python
# 1차적으로 nltk에서 제공하는 불용어사전을 이용해서 불용어를 제거
from nltk.corpus import stopwords
stopwords_list = stopwords.words('english') # nltk에서 제공하는 영어 불용어사전

unique_NN_words = set(lemmatized_words)  ## 중복을 제거하기 위해 set(집합형)으로 변환
final_NN_words = lemmatized_words

# 불용어 제거
for word in unique_NN_words:
    if word in stopwords_list:
        while word in final_NN_words: final_NN_words.remove(word)

print(final_NN_words)
```
```
['hurray', 'hotblack', 'coffee', 'cafe', 'toronto', 'customer', 'cafe', 'york', 'city', 'location', 'café', 'grumpy', 'hotblacks', 'reason', 'blackout', 'cause', 'hosanna', 'president', 'jimson', 'bienenstock', 'aim', 'customer', 'device', 'vibe', 'york', 'time', 'vehicle', 'interaction',  'commodity', 'idea', 'bienenstock', 'science', 'decade', 'interaction', 'contributor', (생략)]
```

+) 제거하고자 하는 단어가 nltk 제공 사전에 포함되어 있지 않다면, 아래와 같이 직접 불용어 사전을 만들어 추가로 단어를 제거해도 된다
```python
customized_stopwords = ['be', 'today', 'yesterday'] # 직접 만든 불용어 사전
unique_NN_words1 = set(final_NN_words)
for word in unique_NN_words1:
    if word in customized_stopwords:
        while word in final_NN_words: final_NN_words.remove(word)
```

## Frequency Analysis

### Counter
: 단어 사용 빈도를 계산해주는 모듈.

```python 
from collections import Counter  # import해서 사용

c = Counter(final_NN_words)  ## 단어 개수를 세어준다
print(c)
```
```
Counter({'health': 11, 'people': 11, 'researcher': 7, 'study': 6, 'tie': 6, 'interaction': 5, 'friend': 4, 'others': 4, 'exercise': 4, 'york': 3, 'time': 3, 'connection': 3, 'woman': 3, 'problem': 3, 'lifestyle': 3, 'smoking': 3, 'lack': 3, 'heart': 3, 'death': 3, 'connectedness': 3, 'disease': 3, 'loneliness': 3, 'research': 3, 'blood': 3, 'inflammation': 3, 'texas': 3, 'seppala': 3, 'cafe': 2, 'customer': 2, 'reason': 2, 'bienenstock': 2, 'device': 2, 'longevity': 2, 'evidence': 2, (생략)})
```
→ 가장 많이 나온 단어 10개 추출
```python
print(c.most_common(10))
```
```
[('health', 11), ('people', 11), ('researcher', 7), ('study', 6), ('tie', 6), ('interaction', 5), ('friend', 4), ('others', 4), ('exercise', 4), ('york', 3)]
```

### WordCloud
: 단어의 빈도를 크기로 표현해 워드클라우드를 만들어주는 모듈

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt

total_words = ''
for word in final_NN_words:
	total_words = total_words+' '+word

wordcloud = WordCloud(max_font_size=40, relative_scaling=.5).generate(total_words)
plt.figure()
plt.imshow(wordcloud)
plt.axis("off")
plt.show()
```
![WordCloud1](../../../assets/images/text_english/wordcloud1.png)
- `max_font_size`: 가장 크기가 크게 나올 (=가장 빈도가 높은) 단어의 크기에 제한을 둠
- `relative_scaling`: 빈도 차이에 따른 scaling 정도를 지정.
    - 1일 경우, 2배로 빈도가 높으면 2배로 크게 그려짐. 0에 가까울수록 크기 차이가 덜 나게 그리는 것.

+) colormap, 배경색 지정
```python
wordcloud = WordCloud(relative_scaling=.2,
                     background_color='white',
                     colormap='summer').generate(total_words)

plt.imshow(wordcloud)
plt.axis('off')
plt.show()
```
![WordCloud2](../../../assets/images/text_english/wordcloud2.png)