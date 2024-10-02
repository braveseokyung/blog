---
title : '네이버 쇼핑 크롤링 (Selenium, 네이버 API)'
date : 2024-10-02T11:39:16+09:00
draft : false
authors : ["braveseokyung"]
description : 네이버 쇼핑에서 상품을 검색한 결과를 크롤링하는 두 가지 방법을 소개합니다. 1) Selenium로 검색 자동화 2) 네이버 API
categories : ["python"]
toc:
    auto: true
---

<!--more-->

{{<image src="스크린샷 2024-10-02 오전 11.49.17.png">}}

이 글에서는 위 사진처럼 각 데이터를 네이버 쇼핑에 검색한 결과(상품 이름, 상품 링크, 상품 이미지, 상품 가격) 을 크롤링하는 방법을 소개한다.

크롤링을 할 때, 사이트가 비동기이거나 데이터마다 검색을 해야 해서 클릭을 자동화해야 하는 경우가 있다. 이럴 경우 Selenium이라는 파이썬 라이브러리를 통해 크롬을 불러오고 클릭을 자동화할 수 있다. 하지만 네이버 쇼핑의 경우 Selenium으로 수동 크롤링하는 것 이외의 방법이 있다. 바로 네이버 API를 활용하는 것인데, 항상 API를 활용할 수 있는 것은 아니니 두 가지 방법 모두 정리해두겠다.

## Selenium으로 크롤링하기

크롤링에서 많이 쓰이는 라이브러리에는 Selenium, BeautifulSoup가 있다. BeautifulSoup가 조금 더 간단한데, BeautifulSoup를 쓸 때는 입력받은 링크를 기준으로 HTML을 파싱해 가져오기 때문에 비동기로 프로그래밍 된 사이트들, 즉 링크 변화없이 다른 정보를 보여줄 수 있는 사이트에서는 크롤링이 불가능하다. 이럴 때 클릭을 자동화해서 크롤링할 수 있게 해주는 라이브러리가 Selenium이다.

그럼 Selenium을 이용한 크롤링 코드를 살펴보자!

### 라이브러리 임포트

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service as ChromeService
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By
```

위에서 임포트한 라이브러리는 크롬 브라우저를 로컬에서 열기 위해 필요한 라이브러리들이다.

### 로컬에서 크롬 브라우저 열기

```python
# Chrome 브라우저를 오픈합니다
driver = webdriver.Chrome(service=ChromeService(ChromeDriverManager().install()))
```
위의 코드를 실행하면 아래와 같이 브라우저가 로컬에서 열리게 된다.

{{<image src="스크린샷 2024-10-02 오후 12.10.07.png">}}

```python
driver.get("https://shopping.naver.com/home")
```
이제 우리가 원하는 네이버 쇼핑으로 이동하기 위해 `driver.get()` 안에 원하는 링크를 넣어주면 아래와 같이 로컬에서도 페이지가 이동한다.

{{<image src="스크린샷 2024-10-02 오후 12.20.30.png">}}

사진을 보면 자동화된 소프트웨어에 의해 제어되고 있다고 나오는데, 이게 Selenium이다!

{{< admonition note >}}
크롤링이 끝나면 `driver.quit()` 으로 종료시켜주는 것을 잊지말자!
{{< /admonition >}}

### 검색창에 입력하기, 검색 자동화

```python
search_box=driver.find_element(By.CLASS_NAME,"_searchInput_search_text_3CUDs")
search_box.clear()
search_box.send_keys(query)
driver.find_element(By.CLASS_NAME,"_searchInput_button_search_1n1aw").click()
```
검색창 element를 가져온 다음, `.clear()` 를 통해 내용을 비워서 이전 쿼리를 지운다.
그 다음 새로운 query를 `.send_keys(query)`를 통해 입력한다. 마지막으로 검색 버튼을 눌러 검색을 실행하기 위해 검색 element를 찾아서 클릭한다. 

`.click()`

을 통해 검색을 자동화할 수 있다.

### HTML element 찾기

검색창에 원하는 쿼리를 입력하기 위해서, 또 크롤링할 항목을 가져오기 위해서는 HTML element를 찾아야 한다.
Selenium에서 `driver.find_element()`를 이용해서 HTML element를 찾을 수 있다.
아래와 같이 By를 통해 class, id 등 여러 방법으로 찾을 수 있다. XPATH가 조금 생소한데, HTML을 복붙해오듯이 element를 누르고 copy를 보면 XPATH로 가져올 수 있다.

* `By.CLASS_NAME`
* `By.ID`
* `By.TAG_NAME`
* `By.XPATH`
* `By.CSS_SELECTOR`

참고로 위의 라이브러리 import에서 By를 import 해야 위처럼 가져올 수 있다.

HTML element를 By로 가져와서 그 안의 text를 얻고 싶은 것이라면  `{element}.text().strip()`으로 가져올 수 있지만, 태그 안의 다른 attribute의 정보를 가져오고 싶을 수도 있다. 아래 사진을 보면,

{{<image src="스크린샷 2024-10-02 오후 12.24.36.png">}}

이렇게 class 내에서 우리가 원하는 상품의 이름은 title이라는 attribute에 들어가 있다. 이때 쓰는 것이 `{element}.get_attribute()` 이다.

```python
prd_title=link_element.get_attribute("title")
prd_link=link_element.get_attribute("href")
```

이제 전체 코드를 살펴보자! 영양소 이름이 든 csv를 읽어와서 각 영양소를 query로 검색해서 제품 이름, 링크, 이미지를 가져와서 리스트에 추가하는 코드이다.

```python
# 영양소 csv 읽어오기
prd_csv=pd.read_csv("prd_nm_fnclty.csv")

# 크롤링한 항목 저장할 리스트
prd_title_list=[]
prd_link_list=[]
prd_img_list=[]

def get_title_link_img(query):

    driver.get("https://shopping.naver.com/home")
    search_box=driver.find_element(By.CLASS_NAME,"_searchInput_search_text_3CUDs")
    search_box.clear()
    search_box.send_keys(query)
    driver.find_element(By.CLASS_NAME,"_searchInput_button_search_1n1aw").click()

    # 페이지 로딩 대기
    time.sleep(2)

    prd=driver.find_element(By.CLASS_NAME,"product_item__MDtDF")
    link_element=prd.find_element(By.CLASS_NAME,"product_link__TrAac")
    img_element=prd.find_element(By.CLASS_NAME,"product_img_area__cUrko")
    # img_tag=img_element.find_element(By.CSS_SELECTOR,"img")


    prd_title=link_element.get_attribute("title")
    prd_link=link_element.get_attribute("href")
    prd_title_list.append(prd_title)
    prd_link_list.append(prd_link)


    # prd_img=img_tag.get_attribute("src")
    # prd_img_list.append(prd_img)
    # print(prd_img)
```

하지만 여기서 문제가 생겼는데, 이미지가 어떻게 해도 가져와지지 않는다는 점이었다. 이미지가 들어있는 태그를 살펴보면,

{{<image src="스크린샷 2024-09-27 오후 8.44.28.png">}}

이렇게 ::before에 들어가 있어 어떻게 해도 가져올 수가 없었다.

그래서 이미지까지 가져오고자 선택한 방법이 바로 API이다!

## 네이버 API로 크롤링하기

네이버 API를 사용하기 위해서는 먼저 [Naver Developers](https://developers.naver.com/main/) 에서 API key를 발급받아야 한다.

사이트에 들어가서 서비스 API -> 오픈 API 이용 신청을 눌러 네이버 검색 API를 선택하면 client id와 secret key를 발급받을 수 있다.

### API 크롤링 코드
```python
import requests
import urllib
import json
import re

CLIENT_ID=""
CLIENT_SECRET=""

def search_by_api(query):
    query = urllib.parse.quote(query)

    url = "https://openapi.naver.com/v1/search/shop?query=" + query 

    request = urllib.request.Request(url)
    request.add_header('X-Naver-Client-Id', CLIENT_ID)
    request.add_header('X-Naver-Client-Secret', CLIENT_SECRET)

    response = urllib.request.urlopen(request)
    rescode=response.getcode()
    if(rescode==200):
        items=json.load(response)['items']
        return items
    else:
        print("error")
        return None
```
위 코드를 통해 query에 대한 네이버 쇼핑 검색 결과를 가져올 수 있다! 검색을 통해 가져올 수 있는 정보는 아래와 같다.

{{<image src="스크린샷 2024-10-02 오후 12.42.20.png">}}

제품 이름, 링크, 이미지, 가격, 카테고리 모두를 한번에 json 형식으로 가져올 수 있다.

## Reference
* [이미지 다운받는법](https://hevton.tistory.com/735?category=1168625)
* [네이버 api 등록방법](https://yenpa.tistory.com/2)
* [네이버 api 내 애플리케이션](https://developers.naver.com/apps/#/myapps/Oy2Qg5SDr74V_43Y5O0p/overview)
* 네이버 api로 웹크롤링
    * https://velog.io/@mingu4u/Web-crawling-naver-API-%EC%9B%B9-%ED%81%AC%EB%A1%A4%EB%A7%81%EC%9C%BC%EB%A1%9C-%EB%84%A4%EC%9D%B4%EB%B2%84-%EC%87%BC%ED%95%91-%EC%95%84%EC%9D%B4%ED%8C%A8%EB%93%9C-%EA%B0%80%EA%B2%A9-%EC%9E%90%EB%8F%99-%EB%B9%84%EA%B5%90
    * https://suyeoniii.tistory.com/39
* [결측치 확인 및 제거](https://m.blog.naver.com/j7youngh/222845624447)
* [괄호와 괄호 안 문자열 제거](https://mingchin.tistory.com/64)






