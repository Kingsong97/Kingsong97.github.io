---
layout: post
title: JS를 이용한 Quiz 만들기(Part.0)
date: 2024-04-10 12:30 +0900
description: 
image: 
category: Javascript
tags: Javascript Quiz
published: true
sitemap: true
---

# 퀴즈게임 만들기

## 구상   

우선 어떤 기능들을 넣을건지 생각해보자.

- json 파일로 100문제 가져오기.(주관식)
- 정답확인 기능.
- 정답 확인 후 다음 버튼을 통해 넘어가도록 처리.
- 몇 문제 중에 몇 번째 문제를 풀고 있는지 현황 추가.
- 다 풀고 맞은 갯수 및 틀린 갯수 를 통한 총점 기능


이정도 기능이면 괜찮은 퀴즈게임이 나올 것 같다! 


다음은 기초 디자인 틀을 잡아주자, 모두 자신의 퀴즈게임 이미지를 상상해보길 바란다.

![퀴즈기본 디자인](https://github.com/Kingsong97/Kingsong97.github.io/assets/161429740/1cc14dc4-1951-48cb-8c89-0f57eca6b2ee)

위 사진과 같은 기본 틀을 잡고 진행 할 예정이다.

<details>
<summary>HTML 구조</summary>

````html
<div id="wrap">
        <div class="quiz">
            <div class="quiz_header"></div> <!-- 문제 종목 및 현재문항/남은문항 표시구역-->
            <div class="quiz_main">
                <div class="quiz_quizbox"> <!-- 퀴즈정보(문제,번호 등)-->
                    <div class="question"></div> <!-- 문제가 표출될 곳-->
                    <div id="input">
                        <input type="text" placeholder="정답 입력란">
                    </div> <!-- 답 적는곳 (주관식)-->
                </div>
            </div>
            <div class="quiz_gauge"></div> <!-- 문제 게이지 -->
            <div class="quiz_footer">
                <button class="previous"></button> <!-- 이전 문항 버튼 -->
                <button class="confirm"></button> <!-- 정답 제출 버튼 -->
                <button class="next"></button> <!-- 다음 문항 버튼-->
            </div>
        </div>
    </div>
````
</details>




## 스크립트