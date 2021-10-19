---
layout: default
title: App Review 수집
parent: Web Scraping
nav_order: 5
---

# App Review 수집
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


## Google Play Store Scraping
- [Google-Play-Scraper](https://pypi.org/project/google-play-scraper/){: target="_blank"} library를 활용
- `pip install google-play-scraper`로 설치해서 사용

### App Info 수집

```python
from google_play_scraper import app

# https://play.google.com/store/apps/details?id=com.frograms.wplay
result = app(
    'com.frograms.wplay',  # app id (package name)
    lang='ko', # default: 'en'
    country='kr' # defaul: 'us'
)

print(result)
```
```
{'title': '왓챠', 'description': "- 영화/드라마/예능/애니메이션/다큐멘터리 무제한 스트리밍/다운로드 감상 앱.\r\n- 스마트폰, 태블릿, PC, 맥, 스마트 TV, 셋톱박스, 크롬캐스트에서 모두, 최고의 화질로.\r\n- 이제는 온 가족과 함께 즐기세요!\r\n- 왓챠플레이가 많은 분들이 불러주시는 그 이름 그대로 '왓챠'로 다시 태어났습니다.\r\n\r\n[제품 설명]\r\n\r\n• 스트리밍뿐 아니라, 다운로드 및 오프라인 재생 기능까지.\r\n• 스마트폰, 태블릿, PC, 맥, 스마트 TV, 셋톱박스, 크롬캐스트에서 모두.\r\n• HD부터 Ultra HD 4K까지 최고의 화질로.\r\n• 세계 최고 수준의 추천 엔진으로 내 취향에 맞는 작품만.\r\n• 신생아도 직관적으로 조작할 수 있는, 쉽고 편한 인터페이스.\r\n• 영화, 드라마, 예능뿐 아니라 다큐멘터리, 애니메이션까지.\r\n• 가입하고 둘러보는 건 평생 무료. 일단 다운로드해 보세요.\r\n\r\n\r\n (생략)}
```

### App Review 수집

```python
from google_play_scraper import Sort, reviews

result, continuation_token = reviews(
    'com.frograms.wplay',
    lang='ko',   # default: 'en'
    country='kr',   # default: 'us'
    sort=Sort.MOST_RELEVANT,   # default: Sort.MOST_RELEVANT
    count=3,   # default: 100
    filter_score_with=5  # default: None (모든 평점을 다 가져옴)
)

result
```
```
[{'at': datetime.datetime(2021, 10, 16, 8, 2, 12),
  'content': '짱구 극장판보며 우는 중..... 최신화 업데이트 해주시면 안될까요 보고싶은데ㅠㅠ',
  'repliedAt': None,
  'replyContent': None,
  'reviewCreatedVersion': '1.9.97',
  'reviewId': 'gp:AOqpTOFVBSK8EhoqhobkcZbFrDmaaLdIdsIVAC2-oKDVJSgRX62jWkQzMVbMvosS2-GjoA_BC96wOkYq0DkoVnw',
  'score': 5,
  'thumbsUpCount': 1,
  'userImage': 'https://play-lh.googleusercontent.com/a/AATXAJzCaKwsYBHvCTY6ztFEq_F-WmRahkWIF1hhHQct=mo',
  'userName': '슬구'},
 {'at': datetime.datetime(2021, 10, 1, 16, 17, 43),
  'content': '속도조절 생겨서 좋은데 좀 더 디테일한 속도 조절이였으면 좋겠습니다. 1.25배, 1.5배는 너무 빨라서 보기가 불편합니다. 1.1배부터 다양한 조절이 가능했으면 좋겠네요. 곰플도 되는데 왓챠에서 안될순없죠~ 부탁드립니다.',
  'repliedAt': None,
  'replyContent': None,
  'reviewCreatedVersion': '1.9.96',
  'reviewId': 'gp:AOqpTOFdH8PODeF-Al6Xo04_9IPHIDxdf3a5A2br0lHgtLrvi0ntmSW6hYmbUF9tv2x_6LcD5rIaAKSorWrBZZw',
  'score': 5,
  'thumbsUpCount': 20,
  'userImage': 'https://play-lh.googleusercontent.com/a/AATXAJwiQJj1XEdXv4TfeilZfMufI8bKddTSyQFnKVGv=mo',
  'userName': '안희수'},
 {'at': datetime.datetime(2021, 10, 12, 8, 53, 16),
  'content': '왓챠 잘사용하는 유저입니다 카드로결제한거는 해지해서 10월3일쯤 해지된다고하셔서 취소하고 구글기프트카드로 결제햇는데 왜 돈을 안돌려주시죠?돈돌려주시면 좋겟습니다',
  'repliedAt': None,
  'replyContent': None,
  'reviewCreatedVersion': '1.9.96',
  'reviewId': 'gp:AOqpTOEqFS_DVBEaph1OgYa9IwVZxtghfWKYxgBALGM_jN6Q92zsy1MdlnSDJIKXmUJ4HY_VOWEt_XXWlEJh21c',
  'score': 5,
  'thumbsUpCount': 3,
  'userImage': 'https://play-lh.googleusercontent.com/a-/AOh14GgNhr4vwFU8hQyPbFLYGnSoDfJCJFUBCXhxU5aO',
  'userName': '준튜브JUNTUBE'}]
```

+) `continuation_token`을 설정해주면 이어서 수집 가능

```python
result, _ = reviews(
    'com.frograms.wplay',
    continuation_token=continuation_token   # default: None (처음부터 수집)
)

result
```
```
[{'at': datetime.datetime(2021, 8, 2, 14, 59, 45),
  'content': '저 같은 경우에는 오류도 하나도 없고 콘텐츠도 다양해서 너무 좋습니다ㅜㅜㅜ 잘 알려지지 않은 영화라던가 단편이라던가 앞으로도 더욱 많이 들여와주시면 감사하겠구요ㅜ 그리고 혹시 각 작품마다 공유버튼을 만들어주시면 왓챠를 홍보하기 훨씬 편할것같아요!ㅎㅎ 앞으로도 소비자를 위한 왓챠가 되어주길 바랍니당♡',
  'repliedAt': None,
  'replyContent': None,
  'reviewCreatedVersion': '1.9.85',
  'reviewId': 'gp:AOqpTOEH6UJtiev2mz0oCU3vsn_N90fUIM1BYbLbxh6WU6Lu_elLAphl8J7mmFkOrF1DZ510b2AmURAuinSVncc',
  'score': 5,
  'thumbsUpCount': 10,
  'userImage': 'https://play-lh.googleusercontent.com/a-/AOh14Gh5dTIVYgAtJZdgYj99tpKD3NMBaC7QBmRno8ZSng',
  'userName': '정규반배하민'},
 {'at': datetime.datetime(2021, 9, 21, 4, 52, 21),
  'content': '좋은 컨텐츠 계속 제공해주셨으면 좋겠어요!!>_< 각종 영화제에서 상을 받거나 우수한 평가를 받았던 작품들도,,, 부탁드려요 ㅎㅅㅎ',
  'repliedAt': None,
  'replyContent': None,
  'reviewCreatedVersion': '1.9.96',
  'reviewId': 'gp:AOqpTOE1zthMN8AlbYemq_2IZpwlZ32xOK-46-s39ZSZZvcuZm_Ejasisx4EM1hHrd2nvcS9HzoNKEEVH54k0_Q',
  'score': 5,
  'thumbsUpCount': 5,
  'userImage': 'https://play-lh.googleusercontent.com/a/AATXAJxrul7Th625uvpvsdeXIDcqQjmFlfw2_5t9y8ak=mo',
  'userName': '방지영'},
 {'at': datetime.datetime(2021, 9, 23, 13, 49, 42),
  'content': '넷플릭스보다 볼게 많아서 아주 좋습니다 아쉬운건 애니들 언어 선택이 가능 했으면 좋겟습니다 원피스 원어로 보고 싶었는데 ㅜ',
  'repliedAt': None,
  'replyContent': None,
  'reviewCreatedVersion': '1.9.96',
  'reviewId': 'gp:AOqpTOE3L6Kscm9b2rdPafySeISRRREPHxa-qWa_N6HphWdcYqtqFVpU7F24zSk5i6xs-wb0AL5NUol29Coqi7U',
  'score': 5,
  'thumbsUpCount': 7,
  'userImage': 'https://play-lh.googleusercontent.com/a-/AOh14GghVbKmfdpD-Ld65jdsw5k5l-oq05-IceGUJNnB',
  'userName': '짬뽕짜장면'}
```
- 앞서 받아온 'continuation_token'을 넣어주면, 그 다음부터 이어서 수집이 가능하다. 
- 설정도 그대로 유지됨 (5점 리뷰만, 관련도순으로 3개 수집)


### 특정 기간의 App Review만 수집

- ex) 2020 ~ 2021 기간의 Watcha 리뷰만 모두 수집해오기:

```python
from google_play_scraper import Sort, reviews
import pandas as pd

year = 2021
token = None
review_list = []

while year >= 2020:   # 2020년 ~ 2021년만 수집
    result, continuation_token = reviews(
        'com.frograms.wplay',
        lang = 'ko', # default: 'en'
        country = 'kr', # default: 'us'
        continuation_token = token,
        sort = Sort.NEWEST, # default: Sort.MOST_RELEVANT
        count = 100, # default: 100
        filter_score_with = None # default: None (모든 평점을 다 가져옴)
    )
    
    token = continuation_token
    year = result[-1]['at'].year
    
    for review in result:
        if review['at'].year >= 2020:  # 2020년의 리뷰까지만 저장
            temp_list = [review['score'], review['content'], review['at']]
            review_list.append(temp_list)
        
review_df = pd.DataFrame(review_list, columns=['score', 'content', 'date'])    
print(len(review_df))
review_df.head()
```
```
7378
```

<div class="code-example" markdown="1">

|    |   score | content                                               | date                |
|---:|--------:|:------------------------------------------------------|:--------------------|
|  0 |       1 | 2주 무료체험을 하려고 등록해놓았다가 2주 무료체험이 안되서 하지도 않았는데 2주가... | 2021-10-19 09:29:50 |
|  1 |       5 | 진짜                  | 2021-10-19 07:58:21 |
|  2 |       1 | 왓챠 앱지워도 결제계속떠서 결제안해도 될거에 계속해야하니 짜증나요. 해지도 않되고    | 2021-10-19 01:15:30 |
|  3 |       5 | 일본드라마가 많이 있어서 좋아요        | 2021-10-18 15:21:52 |
|  4 |       4 | 꼭 집을 수는 없지만 조금 아쉬움이....😝         | 2021-10-18 13:41:19 |

</div>


## App Store Scraping
- [App-Store-Scraper](https://pypi.org/project/app-store-scraper/){: target="_blank"} library를 활용
- `pip install app-store-scraper`로 설치해서 사용


### App Review 수집

1. app 정보를 입력해 객체 생성
    ```python
    from app_store_scraper import AppStore

    # https://apps.apple.com/kr/app/watcha/id1096493180
    my_app = AppStore(country='kr', app_name='watcha', app_id='1096493180')  # app_id는 optional

    print(my_app)  # 객체 검증
    ```
    ```
        Country | kr
            Name | watcha
            ID | 1096493180
            URL | https://apps.apple.com/kr/app/watcha/id1096493180
    Review count | 0
    ```

2. 리뷰 수집
- ex) 2020 ~ 2021 기간의 Watcha 리뷰만 모두 수집해오기:

    ```python
    import datetime as dt
    import numpy as np

    my_app.review(after=dt.datetime(2020, 1, 1), sleep=np.random.randint(0, 2))
    ```
    - `my_app.review(how_many, after, sleep)`의 형태
    - how_many = int (적어주지 않으면 모든 리뷰를 다 가져온다)
    - after = datetime (optional, 특정 시간 이후의 리뷰만 가져오겠다는 의미)
    - sleep = int (optional, parameter to specify seconds to sleep between each call) 

3. 가져온 리뷰를 dataframe으로 정리

    ```python
    fetched_reviews = my_app.reviews
    ios_review_df = pd.DataFrame(fetched_reviews)

    print(len(ios_review_df))
    ios_review_df .head()
    ```
    ```
    5069
    ```

    <div class="code-example" markdown="1">

    |    | userName       | isEdited   |   rating | review          | title               | date                |   developerResponse |
    |---:|:---------------|:-----------|---------:|-------------------|:---------------------|:--------------------|--------------------:|
    |  0 | 저내림         | False      |        5 | 개발자 및 경영진분들 꼭 보시라고 별5개 답니다????? 꼭 읽으세요... | 버퍼링실화냐???              | 2020-05-26 12:27:32 |                 nan |
    |  1 | dkqjfsqfe      | False      |        5 | 넷플릭스에서 왓챠로 넘어온지 2개월째입니당\n음 영화를 좋아하시는 분이라면... | 너무좋아요❤️                  | 2020-04-06 06:40:28 |                 nan |
    |  2 | jsa38          | False      |        5 | 안녕하세요! 코로나때문에 너무 심심해서 넷플로 먼저 시작했는데 너무 볼 것이 없어... | 왓챠 최고예요❤️               | 2020-08-25 01:03:45 |                 nan |
    |  3 | 샌드위치냠냠냠 | False      |        5 | 넷플릭스도 쓰고 왓챠도 쓰는데 개인적으로 넷플릭스는 오리지널이 있지만 왓챠가 볼건...   | 너무 좋습니다                | 2020-04-26 16:13:03 |                 nan |
    |  4 | dkjfew         | False      |        5 | 맥북으로 보시려면 앱스토어에서 아이패드용 앱으로 다운 받으시면 됩니다~ 그리고... | 맥북으로 왓챠 볼 수 있어요~! | 2021-03-01 10:45:08 |                 nan |

    </div>

