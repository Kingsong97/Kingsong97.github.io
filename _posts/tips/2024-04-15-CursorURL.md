---
layout: post
title: CSS] 마우스 커서 URL 이미지로 적용하기
date: 2024-04-15 17:29 +0900
description: 
image: https://github.com/Kingsong97/Kingsong97.github.io/assets/161429740/48afbac1-736f-4307-91c7-9852bd85bebd
category: 팁 
tags: tip CSS cursor
published: true
sitemap: true
---

# 개요 #
안녕하세요 ! 오늘은 마우스 커서를 아주 쉽게 바꿔보는 법을 알아보려 합니다.

기본적으로 멋지고 이쁜 웹사이트는 인터랙션이 화려합니다.    
유저의 행동에 따라 알맞은 반응과 효과를 실시간으로 볼 수 있게 해준다면 우리가 공들여 만든 페이지의 머무는 시간은 더 높아지겠죠?

그 중 가장 쉬우면서도, 가장 많이 노출이 되어지는 마우스 커서 이미지를 지금부터 바꿔봅시다.

## 기본 커서 지정 속성 ##
오늘은 URL로 커서 이미지를 변경하는 법을 다루지만, 그에 앞서    
웹에서 기본으로 이미지 추가 없이 제공되어지는 커서들이 있는거 알고 계셨나요 ?

cursor : hlep;  
cursor : wait;  
cursor : grab;  
cursor : zoom-in;  
cursor : crosshair;  
cursor : not-allowed;

모래시계, 십자가, 물음표 등등.. 이와에도 더 있습니다.
생각보다 많은 속성들이 기본으로 제공 되어지죠 ?

## 커서 이미지 사이트 ##

기본커서는 이제 알겠고, 이제 우리가 원하는 다양한 커서 이미지를 가져와야겠죠? 직접 만들어도 ok! 가져와도 ok! 지만, 저는 귀찮은건 딱 질색이니까.. 다른 분들이 만들어 놓은 이미지를 가져 오려고 해요.

제가 가장 애용하는 사이트는 바로 !  
>https://www.cursors-4u.com/

들어가시면 다양한 커서 이미지를 지원하고 있습니다.  
들어가셔서 구경 후 원하는 이미지의 URL을 가져와 주세요 !

## 적용하기 ##

적용은 css를 이용합니다!    

```css
cursor: url("link") [x,y], [fallback]
```
- link : 사용할 커서 이미지의 URL을 넣어주세요.     
- x,y : 이미지의 크기를 지정합니다. 픽셀 단위입니다.        
- fallback : 이미지를 불러올 수 없는 경우에 사용할 기본 커서 스타일을 지정해줍니다.

```css
cursor: url("https://cur.cursors-4u.net/others/oth-8/oth755.cur") 4 4, auto;
```

위와같이 작성하면 끝 !

여러분만의 개성을 쉽고 간단하게 나타내는데 도움이 되길 바랍니다.


