---
layout: default
title: Requests & BeautifulSoup
parent: Web Scraping
nav_order: 1
---

# Requests & BeautifulSoup
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

## Requests
*역할: web communication. 서버에서 (raw) data (=source codes)를 받아온다.

```python
import requests  # import해서 사용

url = 'https://www.imdb.com/title/tt0805647/?ref_=vp_vi_tt'
r = requests.get(url)  ## sends request message & recieves source codes & returns variables

print(r.text)  ## .text로 간단히 불러온 source code 출력
```
```
<!DOCTYPE html>
<html
    xmlns:og="http://ogp.me/ns#"
    xmlns:fb="http://www.facebook.com/2008/fbml">
    <head>
         
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">

    <meta name="apple-itunes-app" content="app-id=342792525, app-argument=imdb:///title/tt0805647?src=mdot">
(생략)
```
+) header도 별도로 가져올 수 있음
```
r.headers 
```
```
{'Content-Type': 'text/html;charset=UTF-8', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'Server': 'Server', 'Date': 'Mon, 21 Dec 2020 12:27:34 GMT', 'x-amz-rid': 'H9QPTE15DWZ8GAJ3F3ZG', 'Set-Cookie': 'uu=BCYhjgIkRCdAZD3iSQrRQiy0YbMFhLNwk8zxrf0rHF5X3OWIKROLTsDOoCR5TLd5OyQ0OMF32iae%0D%0A1Y0pLcflXX3sTAoF6tCuyAmbPBhAOsNT3-PBrLCEP09Zd7kJEDY89VtMKo_UdYkrDrA6PrC082Jb%0D%0AzA%0D%0A; Domain=.imdb.com; Expires=Sat, 08-Jan-2089 15:41:41 GMT; Path=/; Secure, session-id=000-0000000-0000000; Domain=.imdb.com; Expires=Sat, 08-Jan-2089 15:41:41 GMT; Path=/; Secure, session-id-time=2239273654; Domain=.imdb.com; Expires=Sat, 08-Jan-2089 15:41:41 GMT; Path=/; Secure', 'X-Frame-Options': 'SAMEORIGIN', 'Content-Security-Policy': "frame-ancestors 'self' imdb.com *.imdb.com *.media-imdb.com withoutabox.com *.withoutabox.com amazon.com *.amazon.com amazon.co.uk *.amazon.co.uk amazon.de *.amazon.de translate.google.com images.google.com www.google.com www.google.co.uk search.aol.com bing.com www.bing.com", 'Content-Language': 'en-US', 'Strict-Transport-Security': 'max-age=47474747; includeSubDomains; preload', 'Vary': 'Content-Type,Accept-Encoding,X-Amzn-CDN-Cache,X-Amzn-AX-Treatment,User-Agent', 'X-Cache': 'Miss from cloudfront', 'Via': '1.1 214d8a3cdb14de6b0331d1f72902cc67.cloudfront.net (CloudFront)', 'X-Amz-Cf-Pop': 'HKG60-C1', 'X-Amz-Cf-Id': '-_bmiycwCogkwse1lduiRQkvFpgQb2evcKf0DlKRZKSiJwYrPimtfw=='}
```

### Proxy 사용
: proxy를 사용하면 국가를 우회하여 접속할 수 있다 (특정 국가에서만 접속 가능한 사이트에 접속하기)

```python
# Proxy 사용 예시 코드
proxyDict = { 
              "http"  : "http://proxy_example:8080", 
              "https" : "https://proxy_example:8080" 
            }

r = requests.get(url, proxies=proxyDict)
```

### Header 사용
: headers 옵션을 사용하면 사람인 척 접속할 수 있다 (scraper 차단하는 사이트에 접속하기)
- User Agent는 자신이 사용하는 브라우저에서 정보를 가져와도 되고, fake user agent를 받아와서 사용해도 된다.

```python
# Header 사용 예시 코드
headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36'}

r = requests.get(url, headers=headers)
```

+) fake user agent 쉽게 가져와서 사용할 수 있는 라이브러리
```python
import requests
from fake_useragent import UserAgent  # import해서 사용

ua = UserAgent()

headers = {
    'User-Agent': ua.random,
}

resp = requests.get(url, headers = headers)
```

## BeautifulSoup
*역할: information extraction. 원하는 정보를 담은 tag를 찾아서, 그 안의 text를 extract해준다

- 보통 Requests로 웹 상의 source code를 받아온 후에, BeautifulSoup를 통해 원하는 정보를 추출한다

    ```python
    import requests
    from bs4 import BeautifulSoup

    url = 'https://www.imdb.com/title/tt0805647/?ref_=vp_vi_tt'
    r = requests.get(url)  # requests가 url의 source code를 받아온 후,
    soup = BeautifulSoup(r.text, 'lxml')   # BeautifulSoup로 각 tag를 접근할 수 있게 준비
    ```

- parser의 종류:
    - html.parser: 각종 기능 완비, 적절한 속도, 관대함(lenient)
    - lxml: 아주 빠름, 관대함
    - html5lib: 아주 관대함 (웹 브라우저 방식으로 페이지를 해석함), 하지만 속도가 매우 느림
- parser에 따라 읽어오는 방식이 조금씩 다르기에, parser를 변경하면 크롤링이 가능해지는 경우도 있다.

### 직접 tag 이름 호출

```python
# 실습용으로 제작된 html code. (Requests로 웹 상의 source code를 받아오는 단계를 대체)
html_code = """<!DOCTYPE html>

<html>
	<head>
		<title> Cities and Locations </title>
	</head>

	<body>
		<span class="city1"> 
			<a href = "http://english.seoul.go.kr/">Seoul</a>
			<p class = "location"> South Korea </p>
			<a href = "https://en.wikipedia.org/wiki/Seoul">Wikipedia - Seoul </p>
		</span>
		<span class="city2">
			<a href = "hhttp://www1.nyc.gov/">New York City</a>
			<p class = "location western"> U.S.A. </p>
			<a href = "https://en.wikipedia.org/wiki/New_York_City">Wikipedia - NY City </p>
		</span>
		<span class="city3">
			<a href = "http://www.ebeijing.gov.cn/">Beijing</a>
			<p class = "location"> China </p>
			<a href = "https://en.wikipedia.org/wiki/Beijing">Wikipedia - Beijing </p>
		</span>
	</body>
</html>"""

# BeautifulSoup 타입으로 변환
soup = BeautifulSoup(html_code, 'html.parser')
```


1. title tag에 접근하기

    ```python
    soup.title 
    ```
    ```
    <title> Cities and Locations </title>
    ```


1. tag의 text만 추출하기
```python
soup.title.text.strip()  ## .text만 해줘도 되지만, 앞뒤에 space가 있을 수 있어서 보통은 strip()을 함께 써준다.
```
```
Cities and Locations
```


cf) 여러 개의 같은 tag가 있는 경우:
- 직접 tag 이름을 호출해 불러오는 경우는, 같은 tag가 두 개 이상이여도 맨 처음 tag 하나만 불러온다

    ```python
    soup.p
    ```
    ```
    <p class="location"> South Korea </p>
    ```

### find()와 find_all()
: html tag 기반 선택자
- find(): 조건을 충족하는 것 중 가장 처음의 tag 하나만 가져옴
- find_all(): 조건을 충족하는 모든 tag를 list 형태로 다 불러옴

1. find()
    ```python
    soup.find('p')  # 맨 처음 p tag만 반환. soup.p랑 동일.
    ```
    ```
    <p class="location"> South Korea </p>
    ```

1. find_all()
    ```python
    soup.find_all('p')  # list 형태로 모든 p tag 모두 반환
    ```
    ```
    [<p class="location"> South Korea </p>,
    <p class="location western"> U.S.A. </p>,
    <p class="location"> China </p>]
    ```
    → list 형태로 반환되면, indexing을 통해 text를 추출하면 된다
    ```python
    soup.find_all('p')[2].text.strip()
    ```
    ```
    China
    ``` 

1. 특정 attribute를 갖는 tag 찾기
    ```python
    soup.find('p', attrs={'class':'western'})  # 'western'이라는 'class'를 가진 p tag 찾기
    ```
    ```
    <p class="location western"> U.S.A. </p>
    ```
    - `soup.find('p', attrs={'class':'western'})`는 다음과 같이 써도 동일: `soup.find('p', 'western')`, `soup.find('p', class_='western')` (편한대로 줄여서 써도 된다)


### tag 내 element 추출

1. .get() 함수
    ```python
    soup.find('a')  # a tag에 접근
    ```
    ```
    <a href="http://english.seoul.go.kr/">Seoul</a>
    ```
    → a tag의 'href'에서 url 정보 추출하기
    ```python
    soup.find('a').get('href')
    ```
    ```
    http://english.seoul.go.kr/
    ```

1. \[ \] 방식
    ```python
    soup.find('a')['href']  ## 이렇게 접근하는 것도 가능
    ```
    ```
    http://english.seoul.go.kr/
    ```


### Nested Tags 접근
- a tag can be a parent / child / sibling of another tag
- 특정 parent tag에 먼저 접근한 후, child tag에 접근하면 더 정교한 접근이 가능하다

```python
city3_info = soup.find_all('span', 'city3')[0]   # parent tag
city3_info
```
```
<span class="city3">
<a href="http://www.ebeijing.gov.cn/">Beijing</a>
<p class="location"> China </p>
<a href="https://en.wikipedia.org/wiki/Beijing">Wikipedia - Beijing </a></span>
```
→ 해당 parent tag 아래에 있는 child tag에 접근해서 정보 추출
```python
city3_info.a.get('href')
```
```
http://www.ebeijing.gov.cn/
```


### re.compile
: 정규표현식을 활용하면 특정 글자를 포함하는 tag를 모두 찾아오는 것이 가능

```python
import re

tags = soup.find_all('span', attrs={'class':re.compile('city')})   
# 이렇게 하면 class attribute에 'city'가 포함된 span tag를 다 찾는다
## <span class='city1'>, <span class='city2'>, <span class='city3'> 등을 다 찾아옴!
tags
```
```
[<span class="city1">
 <a href="http://english.seoul.go.kr/">Seoul</a>
 <p class="location"> South Korea </p>
 <a href="https://en.wikipedia.org/wiki/Seoul">Wikipedia - Seoul </a></span>,
 <span class="city2">
 <a href="hhttp://www1.nyc.gov/">New York City</a>
 <p class="location western"> U.S.A. </p>
 <a href="https://en.wikipedia.org/wiki/New_York_City">Wikipedia - NY City </a></span>,
 <span class="city3">
 <a href="http://www.ebeijing.gov.cn/">Beijing</a>
 <p class="location"> China </p>
 <a href="https://en.wikipedia.org/wiki/Beijing">Wikipedia - Beijing </a></span>]
```



## BeautifulSoup: select()
: CSS 기반 선택자
    
- select(): find_all()과 유사. 조건을 충족하는 모든 tag를 list 형태로 다 불러옴
- select_one(): find()와 유사. 조건을 충족하는 것 중 가장 처음의 tag 하나만 가져옴

※ 태그 이름, 속성, 속성값을 특정하는 방식은 find()와 같다. 하지만 select()는 이 외에도 다양한 선택자(selector)를 갖기 때문에 여러 요소를 조합하여 태그를 특정하기 쉽다는 장점이 있다 (more flexible!)


```python
html_code = """<!DOCTYPE html>
<html>
<head>
    <title>Sample Website</title>
</head>
<body>
<h2>HTML 연습!</h2>

<p>이것은 첫 번째 문단입니다.</p>
<p>이것은 두 번째 문단입니다!</p>

<ul>
    <li id="coffee">커피</li>
    <li class="green-tea">녹차</li>
    <li class="green-tea favorite">Sencha Green Tea</li>
    <li class="milk">우유</li>
</ul>

<img src='https://i.imgur.com/bY0l0PC.jpg' alt="coffee"/>
<img src='https://i.imgur.com/fvJLWdV.jpg' alt="green-tea"/>
<img src='https://i.imgur.com/rNOIbNt.jpg' alt="milk"/>

</body>
</html>"""

# BeautifulSoup 타입으로 변환
soup = BeautifulSoup(html_code, 'html.parser')
```

1. html tag로 선택
    ```python
    # soup.select()
    li_tags = soup.select('li')  # <li> 태그만 모아서 리스트로 출력
    li_tags
    ```
    ```
    [<li id="coffee">커피</li>,
    <li class="green-tea">녹차</li>,
    <li class="green-tea favorite">Sencha Green Tea</li>,
    <li class="milk">우유</li>]
    ```

    → text만 추출
    ```python
    li_tags[0].text  # find()에서와 동일하게 .text를 사용
    ## 하나만 추출하고 싶을 때는 soup.select_one('li').text라고 해도 동일
    ```
    ```
    커피
    ```

1. tag 내 element 추출
    ```python
    img_tags = soup.select('img')
    img_tags
    ```
    ```
    [<img alt="coffee" src="https://i.imgur.com/bY0l0PC.jpg"/>,
    <img alt="green-tea" src="https://i.imgur.com/fvJLWdV.jpg"/>,
    <img alt="milk" src="https://i.imgur.com/rNOIbNt.jpg"/>]
    ```
    → find()에서와 동일하게, `[]`이나 `.get()`을 활용하면 tag 내 element 추출 가능
    ```python
    img_tags[0]['src']  ## img_tags[0].get('src')와 동일
    ```
    ```
    https://i.imgur.com/bY0l0PC.jpg
    ```

### CSS 선택자로 접근

1. 특정 id의 태그 선택
- `#`으로 접근 - ex) id가 coffee인 태그: `#coffee`

    ```python
    soup.select('#coffee')     #soup.find('#coffee')는 성립X
    ```
    ```
    [<li id="coffee">커피</li>]
    ```

1. 특정 class의 태그 선택
- `.`으로 접근 - ex) class가 'green-tea'인 태그: `.green-tea`

    ```python
    soup.select('.green-tea')
    ```
    ```
    [<li class="green-tea">녹차</li>,
    <li class="green-tea favorite">Sencha Green Tea</li>]
    ```

1. 속성의 이름/값으로 태그 선택
- `[name="value"]` 형식으로 접근
- ex) alt 속성의 값이 "green-tea"인 태그: `[alt="green-tea"]`
- ex) href 속성의 값이 "https://chaelist.github.io/"인 태그: `[href="https://chaelist.github.io/"]`

    ```python
    soup.select('[alt="green-tea"]')
    ```
    ```
    [<img alt="green-tea" src="https://i.imgur.com/fvJLWdV.jpg"/>]
    ```

### CSS 선택자 조합하여 사용
: 아래와 같이 다양한 조합으로 접근할 수 있다는 것이 CSS 기반 선택자인 select()의 장점!

1. OR 연산
- `,` 사용 → 두 CSS 선택자 중 하나라도 해당되면 선택

    ex1)
    {: .fs-3 }
    ```python
    # id가 coffee이거나 class가 milk인 태그
    soup.select('#coffee, .milk')
    ```
    ```
    [<li id="coffee">커피</li>, <li class="milk">우유</li>]
    ```
    ex2) 
    {: .fs-3 }
    ```python
    # 모든 p 태그와 모든 li 태그
    soup.select('p, li')
    ```
    ```
    [<p>이것은 첫 번째 문단입니다.</p>,
    <p>이것은 두 번째 문단입니다!</p>,
    <li id="coffee">커피</li>,
    <li class="green-tea favorite">녹차</li>,
    <li class="milk">우유</li>]
    ```

1. AND 연산
- 두 CSS 선택자를 붙여씀 → 두 CSS 선택자 모두 해당되는 요소만 선택

    ex1)
    {: .fs-3 }
    ```python
    # green-tea 클래스와 favorite 클래스를 모두 가진 태그
    soup.select('.green-tea.favorite')
    ```
    ```
    [<li class="green-tea favorite">Sencha Green Tea</li>]
    ```
    ex2) 
    {: .fs-3 }
    ```python
    # favoite 클래스를 가진 li 태그
    soup.select('li.favorite')
    ```
    ```
    [<li class="green-tea favorite">Sencha Green Tea</li>]
    ```

1. Nested Tags 접근 (Descendant Combinator)
- 두 CSS 선택자를 띄어씀 → 첫 번째 CSS 선택자로 선택된 태그 내부에서 두 번째 CSS 선택자를 찾는다

    ```python
    # ul 태그 아래에 있는 자손 태그 중, milk라는 class를 가진 태그
    soup.select('ul .milk')
    ```
    ```
    [<li class="milk">우유</li>]
    ```

    cf) `.class1 .class2`와 `.class1 > .class2`의 차이:
    - `.class1 .class2`: class1 클래스를 가진 태그의 "자손" 중에 class2 클래스를 가진 모든 태그를 의미
    - `.class1 > .class2`: class1 클래스를 가진 태그의 "자식" 중에 class2 클래스를 가진 모든 태그를 의미
    - "자손": 부모 요소에 포함된 모든 하위 요소를 의미 vs "자식": 부모의 바로 아래 자식 요소만을 의미
    
    &nbsp;
    {: .fs-1 }

1. 형제 태그 접근 (Sibling Combinator)
- `~` 사용 → ` 특정 태그 뒤에 나오는 형제 태그를 선택
- 형제 태그: parent-child 관계가 아닌, 같은 depth를 가진 태그 (병렬적 나열)

    ex1)
    {: .fs-3 }
    ```python
    # green-tea라는 class를 가진 태그 뒤에 나오는 green-tea라는 class를 가진 태그
    soup.select('.green-tea ~ .green-tea')
    ```
    ```
    [<li class="green-tea favorite">Sencha Green Tea</li>]
    ```

    ex2)
    {: .fs-3 }
    ```python
    # ul 태그 뒤에 나오는 img 태그 (뒤에 나오는 모든 img 태그)
    soup.select('ul ~ img')
    ```
    ```
    [<img alt="coffee" src="https://i.imgur.com/bY0l0PC.jpg"/>,
    <img alt="green-tea" src="https://i.imgur.com/fvJLWdV.jpg"/>,
    <img alt="milk" src="https://i.imgur.com/rNOIbNt.jpg"/>]
    ```

    cf) 인접형제 선택자 `+`: 특정 태그 바로 뒤에 나오는 형제 태그를 선택. 
    - `~`와 유사하나, 바로 뒤에 있는 1개만 선택한다는 점에서 차이
    ```python
    # ~ 대신 +를 사용하면 바로 뒤에 나오는 태그 1개만 찾아줌
    soup.select('ul + img')
    ```
    ```
    [<img alt="coffee" src="https://i.imgur.com/bY0l0PC.jpg"/>]
    ```



