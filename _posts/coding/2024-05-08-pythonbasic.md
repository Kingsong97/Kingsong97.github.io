---
layout: post
title: Python) Python 기초
date: 2024-05-08 12:59 +0900
description: 
image: https://github.com/Kingsong97/Kingsong97.github.io/assets/161429740/db65de14-dc59-4576-8cec-82acc61d8013
category: Python
tags: Python
published: true
sitemap: true
---

# 파이썬 설치하기 

안녕하세요! 오늘은 python 설치 하는법에 대해서 알려드리도록 하겠습니다.<br>

python을 알고 공부하시게 되면 많은 정보를 손 쉽게 얻을 수 있습니다. <br>

https://www.python.org/ 해당 사이트로 이동 후 python을 설치 합니다.<br>

## 설치된 목록 보기

````cmd
pip3 list (mac)
pip list (windows)
````

설치된 목록을 보기위해 pip3 list를 터미널에 입력 합니다. <br>

![image](https://github.com/nicejmp1/nicejmp1.github.io/assets/163364733/3a046070-0567-4096-87cd-f101426545c9)

저는 설치된게 많아서 이렇게 나오지만 처음 설치하시게 되면 한가지만 나올 것입니다.<br>

다음 중요하게 설치 해야 할것을 알려드리겠습니다. <br>

````cmd
pip3 install requests (Mac)
pip3 install beautifulsoup4
pip3 install lxml
pip3 install pandas
pip3 install selenium

````

각 라이브러리의 용도와 기능에 대한 설명
1. requests      
    requests 라이브러리는 Python에서 HTTP 요청을 보내는 기능을 제공합니다. 이를 통해 웹 페이지의 내용을 가져오거나 API를 사용할 수 있습니다. 웹 서버와의 간단한 상호작용을 위해 널리 사용됩니다.
2. beautifulsoup4
    beautifulsoup4는 HTML과 XML 파일로부터 데이터를 추출하기 위해 사용되는 라이브러리입니다. 복잡한 HTML 문서에서 데이터를 쉽게 파싱하고 조작할 수 있어 웹 스크래핑에 매우 유용합니다.
3. lxml
    lxml은 XML과 HTML을 처리하기 위한 라이브러리로, 매우 빠르고 기능이 풍부합니다. 주로 beautifulsoup4와 함께 사용되어 파싱 처리 속도를 향상시키는 파서로 사용됩니다.
4. pandas
    pandas는 데이터 분석과 조작을 위해 널리 사용되는 라이브러리입니다. 데이터를 효율적으로 처리하고 변형 및 분석할 수 있게 해주며, 다양한 형식의 데이터를 쉽게 다룰 수 있습니다.
5. selenium
    selenium은 웹 브라우저를 자동화하기 위해 사용되는 라이브러리입니다. 웹 애플리케이션의 테스트 자동화나 복잡한 웹 페이지와의 상호작용을 자동화하는데 사용됩니다.

## 파이썬 확인 하기

VScode에서 터미널을 킨 다음 <br>
python3 들어갈 주소 .py를 입력하면 파이썬이 정상적으로 작동 되는것을 확인 하실 수 있습니다. <br>

## 파이썬 활용 하기
- 초기설정을 맞췄으면 이제 파이썬을 활용 하여 필요한 파일을 받아 오도록 하겠습니다. <br>

저흰 벅스 뮤직에서 필요한 파일을 가져 오도록 하겠습니다. <br>

````python
import requests as req
from bs4 import BeautifulSoup as bs
import pandas as pd

res = req.get("https://music.bugs.co.kr/chart")
print(res.status_code)
print(res.text)

````

import로 라이브러리안에 설치된 파일들을 불러옵니다.<br>
저희가 쓸 파일은 requests, BeautifulSoup ,pandas 입니다.<br>
길게 쓰면 불편하니까 전부 약자로 사용 하도록 하겠습니다. <br>

res 같은 경우 해당 파일에 있는 내용을 불러오는 것입니다.<br> 
실행 시켜면 다음과 같이 나옵니다.<br>

res.text 같은 경우 해당 소스 전체를 text로 보게 하는것이고, status_code는 사이트에 들어간 코스 갯수를 확인하는 명령어입니다.<br>


````python
soup = bs(res.text, "lxml")
print(soup)
````
해당 명령어로 text를 좀 더 깔끔하게 보이게 끔 합니다. <br>

## 데이터 선택

필요한 데이터를 선택해야 하니 앞선에 해었던 소스를보고 필요한 소스를 선택합니다. <br>
저희는 오늘 랭킹, 타이틀, 아티스트에 대한 정보를 가져 오도록 하겠습니다. <br>


````python
ranking = soup.select(".ranking > strong")
title = soup.select(".title > a")
artist = soup.select(".artist >a:nth-child(1)")

print(len(ranking))
print(len(title))
print(len(artist))

````

해당 데이터를 가져 오게 되면 그 안에 코드 갯수를 알 수 있습니다. <br>

ranking : 100 <br>
title : 100 <br>
artist : 100 <br>

이렇게 100개씩 출력 되는것을 확인 하실 수 있습니다. <br>

## 데이터 저장

````python
rankingList = []
titleList = []
artistList = []

for i in range(len(ranking)) : 
    rankingList.append(ranking[i].text)
    titleList.append(title[i].text)
    artistList.append(artist[i].text)

print(rankingList)
print(titleList)
print(artistList)

data = {"rank" : rankingList, "title" : titleList, "artist" : artistList}
print(pd.DataFrame(data))
````

rankingList 를 [] 로 선언 한 후 저흰 100문장을 불러와야 하니까 for문 중에서 in 을 이용하겠습니다. <br>

python에서 for문은 range를 이용한후 len을 사용합니다. <br>
그리고 append 많이 보셨을텐데 jquery에서 원하는 코드를 찾을때 사용하는 코드입니다. <br>

그렇게 해서  출력하게 되면 rank : rankingList 안에 저장된 내용, title: titleList 안에 저장된 내용, artist : artistList 안에 저장된 내용들이 출력되게됩니다. <br>

## 데이터 저장 한문장으로 변환

````python
rankingList = [r.text.strip() for r in ranking]
titleList = [t.text.strip() for t in title]
artistList = [a.text.strip() for a in artist]

````

이런식으로 코드를 짜게 되면 가독성도 좋아보이고 편하게 사용 할 수 있습니다.<br>
python은 약자를 많이 사용하니까 헷갈리지 않게 사용 하시면 됩니다. <br>

## json 데이터 프레임 생성 후 저장 하기

````python
chart_df = pd.DataFrame({
    'Ranking' : rankingList,
    'Title': titleList,
    'Artist' : artistList
})

chart_df.to_json("bugsChart100.json", force_ascii=False, orient="records")
````

이렇게 하시면 되면 DataFrame 안에있는 내용을 json으로 변환해서 받을 수 있습니다. <br>

이렇게 오늘은 python을 이용하여 벅스뮤직에서 필요한 데이터를 찾아 사용 할 수 있게 되었습니다.! <br>
