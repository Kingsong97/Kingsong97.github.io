---
layout: post
title: Python) Python으로 Json파일 만들기
date: 2024-05-09 12:59 +0900
description: 
image: https://github.com/Kingsong97/Kingsong97.github.io/assets/161429740/db65de14-dc59-4576-8cec-82acc61d8013
category: Python
tags: Python
published: true
sitemap: true
---

# AppleMusic json 추출하기

안녕하세요! 오늘은 python으로 appleMusic 100위 차트를 json으로 만드는 방법에 대해 알려드리겠습니다.<br>

먼저 애플뮤직 같은경우 정적인 사이트가 아닌 동적인 사이트 이기 때문에 select로는 제가 원하는 파이을 추출 할 수가 없었습니다. <br>

동적인파일을 추출하기 위해서는 <b>Selenium</b>를 설정하여야 합니다.<br>

## Selenium 설정하기
1. Selenium 라이브러리 설치 :
````cmd
pip3 install selenium
````
2. webdriver_manager 설치 :

````cmd
pip3 install webdriver_manager
````
<br>

설치되는 각 라이브러리의 용도로는 다음과 같습니다. <br>
- Selenium: 웹 페이지를 자동으로 제어하기 위해 사용되는 라이브러리입니다. 웹 브라우저를 프로그래밍 방식으로 제어할 수 있게 해줍니다 <br>

- Webdriver-Manager: Selenium 웹드라이버(ChromeDriver 등)의 설치 및 관리를 자동화해주는 라이브러리입니다. 이 라이브러리를 사용하면, 매번 웹드라이버를 수동으로 다운로드하고 경로를 설정하는 번거로움 없이 자동으로 최신 버전의 웹드라이버를 관리할 수 있습니다 <br>

이렇게 되면 추출할 준비는 끝났습니다. 이제 본격적으로 들어가기 전 정적인 페이지와 동적인 페이지를 어떻게 구별하는지 설명하겠습니다.<br>

## 정적 vs 동적 어떻게 구별??

- 정적 웹사이트: 웹 페이지의 소스 코드를 보았을 때 대부분의 콘텐츠가 HTML, CSS, 간단한 JavaScript로 구성되어 있습니다. 페이지의 내용이 그대로 보이며, 사용자의 요청에 따라 서버에서 새로운 페이지를 로드하지 않고 HTML 파일을 그대로 전달합니다.
<br>
- 동적 웹사이트: JavaScript가 많이 사용됩니다. 특히 AJAX 호출이나 API 요청을 통해 데이터를 가져와 페이지에 콘텐츠를 동적으로 렌더링하는 구조를 보입니다. 페이지 소스를 보면 주요 콘텐츠가 비어 있거나 스크립트에 의존하는 경우가 많습니다. 
<br>
- 네트워크 탭 사용 : 브라우저의 개발자 도구에서 네트워크 탭을 열고 페이지를 새로고침하면, 네트워크 활동을 통해 페이지가 어떻게 데이터를 로드하는지 확인할 수 있습니다.
<br>

- 정적 웹사이트: 주로 HTML, CSS, 정적 이미지 파일이 로드됩니다. 
<br>
- 동적 웹사이트: 데이터를 요청하는 여러 AJAX 호출이나 JSON, XML 파일 요청이 보일 수 있습니다. 이러한 요청은 페이지의 일부분만을 업데이트하는 데 사용됩니다 
<br>

- 콘텐츠의 변화 관찰 : 웹사이트를 사용하면서 페이지 내용이 사용자와의 상호작용에 따라 변하는지 관찰합니다. 예를 들어, 페이지를 새로고침하지 않고도 콘텐츠가 변경되거나 추가되는 경우, 이는 동적 웹사이트의 특징입니다. 
<br>

- Javascript 비활성화 : 브라우저에서 JavaScript를 일시적으로 비활성화하고 웹사이트를 방문해보세요. JavaScript 없이 페이지의 주요 기능이나 콘텐츠가 제대로 표시되지 않는다면, 그 웹사이트는 동적인 요소에 크게 의존하고 있을 가능성이 높습니다.
<br>

- URL구조 : 일부 동적 웹사이트는 URL에 # 또는 ?를 포함하여 상태를 동적으로 관리하고, AJAX 콘텐츠를 로드하는 방식을 사용합니다. 정적 웹사이트에서는 각 페이지가 고유의 URL을 갖고 있으며, 이는 간단하고 명확한 경로를 가지는 경우가 많습니다. 
<br>

애플 뮤직을 살펴봤을때 Javascript를 비활성화 하게 되면 추출할려는 내용들이 사라지는 것이 확인 되었습니다. 
<br>

### Javascript비활성화 방법

![Group 1671](https://github.com/nicejmp1/nicejmp1.github.io/assets/163364733/3277c28c-7d05-4e83-aa64-19b29f5b9d9b)

설정 - 개인정보 및 보안 - 사이트 설정 - 자바스크립트 - 자바스크립트 허용 비활성
<br>

이렇게 하면 Javascript를 비활성화 시켜 동적인 페이지인지 정적인 페이지인지 한눈에 알 수 있게 되었습니다. <br>

## Selenium 사용하기

이제 본격적으로 Selenium을 사용하여 데이터를 추출해 보도록 하겠습니다. <br>

```` python
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service as ChromeService
from selenium.webdriver.chrome.options import Options as ChromeOptions
from webdriver_manager.chrome import ChromeDriverManager
import pandas as pd

# Chrome 옵션 설정
options = ChromeOptions()
options.add_argument("window-size=1920x1080")
options.add_argument("disable-gpu")
options.add_argument("lang=ko_KR")
options.add_argument("user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36")
service = ChromeService(executable_path=ChromeDriverManager().install())
driver = webdriver.Chrome(service=service, options=options)

driver.get("https://music.apple.com/kr/playlist/%EC%98%A4%EB%8A%98%EC%9D%98-top-100-%EB%8C%80%ED%95%9C%EB%AF%BC%EA%B5%AD/pl.d3d10c32fbc540b38e266367dc8cb00c")
````

해당 코드를 보게되면 전시간에는 res.get을 하였었지만 동적인 페이지에서 불러와야 하는 것이기 때문에 driver.get을 이용 하였습니다. 
<br>
다음으로 time.sleep란 코드를 사용하였습니다.

````python
time.sleep(2) 
````

time.sleep(2) 함수는 파이썬 스크립트의 실행을 일시적으로 중지하고, 지정된 시간(여기서는 2초) 동안 기다리게 합니다. 이 함수는 time 모듈에 정의되어 있으며, 주로 프로그램 실행을 일정 시간 동안 멈추게 할 때 사용됩니다 
<br>

이렇게 설정한 다음 이제 본격적으로 사이트에 들어가 필요한 파일들을 추출들의 경로를 찾아야 합니다.
<br>

경로를 찾기 전 저번시간엔 select 하나만으로 찾을 수 없었습니다.
여기선 dirver.find_elements를 사용해야 합니다.
<br>

단 주의해야 할 점이 find_element는 단일객체만 찾기 때문에 여러개의 파일을 찾을려면 복수인 find_elements를 사용 하셔야 한다는점 기억하고 계시면 될것같습니다. <br>

<img width="886" alt="image" src="https://github.com/nicejmp1/nicejmp1.github.io/assets/163364733/a58d51fd-a557-4136-98e3-faa8f807fa02">

먼저 애플뮤직에서 페이지보기를 클릭해서 확인해 보시면 해당 title은 보이시는 경로로 지정 되어있다는 것을 확인하실 수 있습니다. 
<br>

하지만 저희가 필요로 하는 경로는 오직 [data-testid="track-title"] 이부분입니다.
<br>
이제 이것을 어떻게 선택 하는지 그리고 어떻게 사용해야 하는건지 알려드리기전에 Selenium에서 웹 요소를 찾기 위한 선택자에 대해 알려드리겠습니다.
<br>

### Selenium 사용법
Selenium 4 이후 버전에서는 find_element_by_* 메서드 대신 find_element 메서드에 By 클래스를 사용하는 것이 권장됩니다 
<br>

### selenium By 클래스 코드

- driver.find_element(By.ID, "id") : ID를 이용한 검색
- driver.find_element(By.CLASS_NAME, "my-class") : 클래스 이름을 이용한 검색
- driver.find_element(By.NAME) : Name을 이용한 검색
- driver.find_element(By.TAG_NAME) : TAG_NAME을 이용한 검색
- driver.find_element(By.CSS_SELECTOR) : CSS선택자를 이용한 검색

### CSS 선택자 사용, 장점 

- driver.find_element(By.CSS_SELECTOR, "#my-id")
- driver.find_element(By.CSS_SELECTOR, ".my-class")
- driver.find_element(By.CSS_SELECTOR, "div#my-id.my-class")
<br>

#### CSS 선택자의 장점
CSS 선택자를 사용하는 것은 특히 요소가 특정 ID, 클래스, 속성 조합을 가지고 있거나 특정 부모 요소의 자식인 경우와 같이 복잡한 위치 지정이 필요할 때 매우 유용합니다. CSS 선택자는 매우 강력하며, 복잡한 DOM 구조에서도 정확하게 원하는 요소를 찾을 수 있게 해줍니다.
<br>

여기서 CSS선택자를 선택할 수 있는 요소들이 몇가지 있습니다.
<br>

### CSS 선택자 요소

1. ID 선택자 : 요소의 ID를 기반으로 선택합니다. (#elementID)
2. 클래스 선택자 : 요소의 클래스를 기반으로 선택합니다. (.elementClass)
3. 태그 선택자 : 특정 태그를 가진 요소를 선택합니다. (div, p , a)
4. 속성 선택자 : 특정 속성을 가진 요소를 선택합니다. ([attribute="value"],[data-testid="ttestIdValue])
5. 자식 선택자 : 특정 부모 요소의 직접적인 자식 요소를 선택합니다. (parent >child)
6. 하위 선택자 : 특정 요소 하위에 있는 모든 요소를 선택합니다. (div span)
7. 인접 형제 선택자 : 같은 부모를 가지며 DOM에서 바로 다음에 위치하는 형제 요소를 선택합니다. (element + sibling)
8. 일반 형제 선택자 : 같은 부모를 가지고, 특정 요소 이후에 위치하는 모든 형제 요소를 선택합니다. (element ~ sibling)
9. 가상 클래스 선택자 : 요소의 특정 상태(예:hover,active,focus등)에 따라 선택합니다.
10. 가상 요소 선택자 : 요소의 특정 부분(예:before,after)에 스타일을 적용합니다.

여기서 저희가 사용할 선택자 요소는 4번 속성선택자 입니다. <br>

````python
rankings = driver.find_elements(By.CSS_SELECTOR, '[data-testid="track-ranking"]')
titles = driver.find_elements(By.CSS_SELECTOR, '[data-testid="track-title"]')
artists = driver.find_elements(By.CSS_SELECTOR, '.svelte-154tqzm [data-testid="click-action"]')
````
저는 이런식으로 rank,title,artis 경로를 찾아 해당 코드들의 갯수를 출력하는 것에 성공하였습니다. 
<br>

다음으로 저흰 100개를 출력 받아야 하기 때문에 for in문을 이용하여 코드를 짜보겠습니다.

````shell
rankingList = [rank.text for rank in rankings]
titleList = [title.text for title in titles]
artistList = [artist.text for artist in artists if artist.text.strip() != '']

min_length = min(len(titleList), len(artistList), len(rankingList))
titleList = titleList[:min_length]
artistList = artistList[:min_length]
rankingList = rankingList[:min_length]
````

여기서 보시게 되면 min_legth는 뭐고 strip()은 무엇인지 궁굼하실 겁니다.<br>

artist 같은 경우 title 보다 배열의 길이가 서로 다르기 때문에 각 리스트에 대한 길이를 조절하기 위해 min_legth를 사용하게 된겁니다. 해당 코드를 사용하지 않게 되면 에러가 발생하니 귀찮으시더라도 사용 하시는것을 권장드립니다. 
<br>

다음으로 artist를 출력하게 되면 아래와 같이 나올 것입니다.
````python
'미리 듣기', '', '아일릿', '', '', '', 'ZICO', '', '', '', 'BABYMONSTER', '', '', '', 'RIIZE', '', '', '', 'QWER', '', '', '', '아일릿', '', '', '', '보이넥스트도어', '', '', '', 'KISS OF LIFE', '', '', '', 'RIIZE', '', '', '', '투어스', '', '', '', '크러쉬', '', '', '', '(여자)아이들', '', '', '', '도영', '', '', '', '데이식스', '', '', '', '뉴진스', '', '', '', '뉴진스', '', '', '', '르세라핌', '', '', '', 'Taylor Swift', '', '', '', '르세라핌', '', '', '', '뉴진스', '', '', '', '데이식스', '', '', '', '아이유', '', '', '', '데이식스', '', '', '', '도영', '', '', '', 'KISS OF LIFE', '', '', '', '뉴진스', '', '', '', '태연', '', '', '', 'RIIZE', '', '', '', 'aespa', '', '', '', '뉴진스', '', '', '', 'Nerd Connection', '', '', '', '뉴진스', '', '', '', '투모로우바이투게더', '', '', '', '르세라핌', '', '', '', '비비', '', '', '', '도영', '', '', '', 'RIIZE', '', '', '', 'Taylor Swift', '', '', '', '뉴진스', '', '', '', '도영', '', '', '', '뉴진스', '', '', '', '범진', '', '', '', '크러쉬', '', '', '', '이창섭', '', '', '', '임재현', '', '', '', '투모로우바이투게더', '', '', '', 'Benson Boone', '', '', '', 'KISS OF LIFE', '', '', '', '루시', '', '', '', '', '정국', 'Latto', '', '', '', 'RIIZE', '', '', '', '뉴진스', '', '', '', '아이브', '', '', '', 'aespa', '', '', '', 'NMIXX', '', '', '', '백예린', '', '', '', '비비지', '', '', '', 'Alaina Castillo', '', '', '', '잔나비', '', '', '', '보이넥스트도어', '', '', '', 'RIIZE', '', '', '', '도영', '', '', '', '릴러말즈', '', '', '', '데이먼스 이어', '', '', '', 'Nerd Connection', '', '', '', '최유리', '', '', '', 'NCT DREAM', '', '', '', '뉴진스', '', '', '', '백예린', '', '', '', '보이넥스트도어', '', '', '', 'QWER', '', '', '', 'BIG Naughty', '', '', '', '그루비룸', '', '', '', 'KISS OF LIFE', '', '', '', '보이넥스트도어', '', '', '', '도영', '', '', '', '악뮤', '', '', '', '검정치마', '', '', '', '아이유', '', '', '', '도영', '', '', '', '성시경', '', '', '', '아이브', '', '', '', '세븐틴', '', '', '', '르세라핌', '', '', '', 'Ariana Grande', '', '', '', '실리카겔', '', '', '', '헤이즈', '', '', '', '르세라핌', '', '', '', '로이킴', '', '', '', '데이식스', '', '', '', '우디', '', '', '', '뉴진스', '', '', '', '이클립스', '', '', '', '도영', '', '', '', '폴킴', '', '', '', 'NCT WISH', '', '', '', 'BLACKPINK', '', '', '', '데이식스', '', '', '', '백예린', '', '', '', 'KISS OF LIFE', '', '', '아일릿', 'ZICO', 'BABYMONSTER', 'RIIZE', 'QWER', '보이넥스트도어', 'KISS OF LIFE', '투어스', '크러쉬', '(여자)아이들', '도영'
````

해당 공백을 제거 하기 위해 strip() 메서드를 사용하여 공백을 제거한 후 필터링을 수행하여야 했습니다.
<br>

이렇게 하고 난 이후 json 파일으로 저장하면 끝이 나는 겁니다.

````python
chart_df = pd.DataFrame({
    "Rank" : rankingList,
    "Title" : titleList,
    "Artist" : artistList
})

chart_df.to_json("appleMusic100.json", force_ascii=False , orient="records")


driver.quit()
````

참고로 driver.quit() 같은 경우 항상 마지막에 써 주셔야 에러가 안나고 정상적으로 작동 됩니다.
<br>

끝으로 최종 코드를 올려드리고 마무리 하겠습니다.

````python
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service as ChromeService
from selenium.webdriver.chrome.options import Options as ChromeOptions
from webdriver_manager.chrome import ChromeDriverManager
import pandas as pd

# Chrome 옵션 설정
options = ChromeOptions()
options.add_argument("window-size=1920x1080")
options.add_argument("disable-gpu")
options.add_argument("lang=ko_KR")
options.add_argument("user-agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36")
service = ChromeService(executable_path=ChromeDriverManager().install())
driver = webdriver.Chrome(service=service, options=options)

driver.get("https://music.apple.com/kr/playlist/%EC%98%A4%EB%8A%98%EC%9D%98-top-100-%EB%8C%80%ED%95%9C%EB%AF%BC%EA%B5%AD/pl.d3d10c32fbc540b38e266367dc8cb00c")

time.sleep(2) 

# 여러 요소를 찾는 메서드를 사용합니다.
rankings = driver.find_elements(By.CSS_SELECTOR, '[data-testid="track-ranking"]')
titles = driver.find_elements(By.CSS_SELECTOR, '[data-testid="track-title"]')
artists = driver.find_elements(By.CSS_SELECTOR, '.svelte-154tqzm [data-testid="click-action"]')
# 타이틀의 개수를 출력합니다.
# print(len(titles))
# print(len(artist))

# 브라우저 종료
rankingList = [rank.text for rank in rankings]
titleList = [title.text for title in titles]
artistList = [artist.text for artist in artists if artist.text.strip() != '']

min_length = min(len(titleList), len(artistList), len(rankingList))
titleList = titleList[:min_length]
artistList = artistList[:min_length]
rankingList = rankingList[:min_length]

chart_df = pd.DataFrame({
    "Rank" : rankingList,
    "Title" : titleList,
    "Artist" : artistList
})

chart_df.to_json("appleMusic100.json", force_ascii=False , orient="records")


driver.quit()

````
