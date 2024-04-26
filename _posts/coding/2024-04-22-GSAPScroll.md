---
layout: post
title: GSAP) GSAP_ScrollAnime
date: 2024-04-22 12:00 +0900
description: 
image: 
category: GSAP
tags: GSAP Javassciprt Animetion Effect
published: true
sitemap: true
---

# GSAP ScrollTrigger 애니메이션 기능 살펴보기 #

안녕하세요 ! 오늘은 GSAP의 스크롤을 활용한 애니메이션 기능을 봐볼까요 !

GSAP(GreenSock Animation Platform)은 웹에서 화려한 애니메이션을 구현할 수 있는 강력한 도구입니다. 특히, ScrollTrigger 플러그인은 스크롤과 관련된 애니메이션을 매끄럽게 제어할 수 있는 놀라운 기능을 제공합니다. 이번 포스트에서는 그 중에서도 주목할 만한 몇 가지 기능들을 짚어보려 합니다.



## 01. 기본 애니메이션 설정하기 ##
ScrollTrigger와 함께 gsap.to 메서드를 사용하면, 다음과 같이 요소를 원하는 방향으로 쉽게 움직이게 할 수 있습니다.

```javascript
gsap.to("#element", {
    duration: 2,
    x: 500,
    rotation: 360,
    borderRadius: 100
});
```
여기서는 요소를 오른쪽으로 500px 움직이고, 360도 회전시키며, 테두리의 모양을 원형으로 변경합니다.

## 02. 트리거 요소 설정 ##
scrollTrigger 옵션을 사용하면 애니메이션이 시작되는 시점을 정확히 제어할 수 있습니다. 예를 들어, 특정 요소가 화면에 등장하는 순간 애니메이션을 시작하게 설정할 수 있습니다.

```javascript
gsap.to("#element", {
    duration: 2,
    x: 500,
    rotation: 360,
    scrollTrigger: {
        trigger: "#triggerElement"
    }
});
```

## 03. 스크롤 방향에 따른 애니메이션 제어 ##
toggleActions 옵션을 활용하면, 사용자가 스크롤을 올릴 때와 내릴 때 각각 다른 애니메이션 효과를 적용할 수 있습니다.

```javascript
gsap.to("#element", {
    duration: 2,
    x: 500,
    rotation: 360,
    scrollTrigger: {
        toggleActions: "play none reverse none"
    }
});
```
## 04. 애니메이션 시작과 끝 조절하기 ##
애니메이션의 시작과 끝 위치를 'start'와 'end' 옵션으로 세밀하게 설정할 수 있습니다.

```javascript
gsap.to("#element", {
    duration: 2,
    x: 500,
    rotation: 360,
    scrollTrigger: {
        start: "top 50%",
        end: "bottom 30%"
    }
});
```
## 05. 스크롤에 따른 부드러운 애니메이션 조절 ##
scrub 옵션을 통해 스크롤을 할 때 애니메이션의 속도를 스크롤의 속도에 맞춰 조절할 수 있습니다.

```javascript
gsap.to("#element", {
    duration: 2,
    x: 500,
    rotation: 360,
    scrollTrigger: {
        scrub: true
    }
});
```
## 06. 요소를 스크롤에 고정하기 ##
pin 옵션을 활용하면 요소를 화면에 고정시켜 스크롤에 따라 같이 움직이도록 할 수 있습니다.

```javascript
gsap.to("#element", {
    duration: 2,
    x: 500,
    rotation: 360,
    scrollTrigger: {
        pin: true
    }
});
```
## 07. 클래스 토글로 다이내믹한 변화 주기 ##
스크롤 이벤트에 따라 요소의 클래스를 추가하거나 제거하는 것도 가능합니다.

```javascript
gsap.to("#element", {
    duration: 2,
    x: 500,
    rotation: 360,
    scrollTrigger: {
        toggleClass: "active"
    }
});
```
## 08. 콜백 함수로 더 많은 상호작용 추가 ##
onUpdate와 onToggle과 같은 콜백 함수를 사용하여 스크롤의 각 단계에서 원하는 코드를 실행시킬 수 있습니다. 이를 통해 애니메이션의 현재 상태를 로그로 확인하거나, 특정 조건에서 추가적인 액션을 실행시킬 수 있습니다.

```javascript
gsap.to("#element", {
    duration: 2,
    x: 500,
    rotation: 360,
    scrollTrigger: {
        onUpdate: (self) => { console.log("현재 진행 상태:", self.progress.toFixed(3)); },
        onToggle: (self) => { console.log("애니메이션 활성화 여부:", self.isActive); }
    }
});
```
이렇게 GSAP ScrollTrigger를 활용하면, 웹 페이지의 애니메이션을 매력적이고 효과적으로 제어할 수 있습니다. 이 도구를 이용해 보세요, 분명 놀라운 결과를 얻을 수 있을 겁니다!
