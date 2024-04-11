---
layout: post
title: 로또 번호 추첨기를 만들어보자 (part.0)
date: 2024-04-04 23:56 +0900
description: 로또 번호 추첨기 프로토타입
image: ../assets/img/jsb.jpg
category: Javascript
tags: Javascript While if rturn
published: true
sitemap: true
---
3월이 지나고 4월이 된 지금, 나의 학원 수업에 대한 자평을 하자면,

>수업을 듣는다 -> 강사님이 자세히 설명해준다 -> 이해가 간다(착각) -> 다음에 사용하거나 연상해야 할 상황이 온다 -> 기억이 안난다 -> 현타 + 자괴감 -> 공부하겠다 다짐 -> 공부하는 법 모르겠음.


라는 흐름으로 간다. 다른 분들은 어떨지 모르겠지만, 나는 이렇다.<br>

고민을 하다가 운이 좋게 같은 수업을 듣는 학생중에 실무경험이 있는 학생 두 분이 계셨다. 뭐든 직접 만들어 보는게 좋다고 했다 그리고 스크립트를 계속 해석하고.    
참 간단한 방법인데, 왜 안했지?! 바로 로또 추첨기를 만들어보기로 했다.

```html
<!DOCTYPE html>
<html lang="ko">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lotto 당첨번호 추첨기</title>
    <link rel="stylesheet" href="../Lotto/asset/style/style.css">
</head>

<body>
    <div id="wrap">
        <h1>번호 생성기</h1>
        <button>당첨 가즈아!</button>
        <div id="numbers"></div>
    </div>

    <script>
        document.querySelector('button').addEventListener('click', () => {
            let numbers = randomNumbers();
            document.getElementById('numbers').innerHTML = numbers.join(', ')
        });

        randomNumbers = () => {
            let lottoNumbers = [];
            while (lottoNumbers.length < 6) {
                let number = Math.floor(Math.random() * 45) + 1;
                if (lottoNumbers.indexOf(number) === -1) {
                    lottoNumbers.push(number);
                }
            }
            return lottoNumbers.sort((a, b) => a - b);
        }
    </script>
</body>

</html>
```
생각보다 금방 만들었다.    
그리고 만들면서 느낀점은 정말 수업시간에 많이 배웠다는걸 느낀다, 어떤식으로 사용해야할지는 아직 감각이 부족한 느낌이라 검색하면서 했지만 만드니까 뿌듯하고 실력이 늘은게 체감이 된다.<br>

<hr/><br>

## Script 해석

```javascript
document.querySelector('button').addEventListener('click', () => {});
```
문서(document)에서 버튼(button)을 선택하겠어!, 그리고 이벤트가 발생을 알아챌 녀석을 추가 하겠다!(addEventListener) 그럼 언제 발생하느냐? 클릭(click) 했을때! 그리고 클릭이 되었다면? () => {} 보이듯 함수가 실행 되겠죠? <br>
즉, 저 함수에 버튼을 클릭했을 때 로또 번호를 생성하고 표시하는 함수를 넣어야 하는거지.   

```javascript
let numbers = randomNumbers();
```
변수 선언과 같지? 후에 작성할 randomNumbers 함수를 가져와서 numbers 라는 변수에 저장할거야.<br>

```javascript
document.getElementById('numbers').innerHTML = numbers.join(', ')
```
문서(document)에서 요소의 이름이 numbers 인걸 가져오겠다(getElementById('numbers')). 그리고 이 요소를 홈페이지에서 나타낼거야(innerHTML). 근데 숫자가 연달아서 나오면 이게 11인지 1인지 헷갈리겠지? 기껏 당첨번호를 뽑았는데 말이야, 그래서 출력되어지는 numbers의 모든 요소를 하나의 문자열로 만드는데 ,와 ' '한칸 공백을 줘서 만들거야(numbers.join). 

```javascript
randomNumbers = () => {}
```
그럼 이제 로또 번호를 생성하는 함수를 위에 코드로 만들어줘야겠지?

```javascript
let lottoNumbers = [];
```
우선, 로또 번호가 6개 나오니까, 그 값을 저장해줄 빈 배열 lottoNumbers 를 만들거야!
```javascript
while (lottoNumbers.length < 6) {
    let number = Math.floor(Math.random() * 45) + 1;
    if (lottoNumbers.indexOf(number) === -1) {
        lottoNumbers.push(number);
    }
}
```
for문과 while문 모두 사용 가능하지만, while문은 특정 조건이 만족 시 까지 반복할 때 사용한다고 내가 기억을해서 이번엔 while문을 사용했어!<br>
lottoNumbers의 배열의 길이가 6이 될 때 까지 라고 조건을 걸어주고(lottoNumbers.length < 6) <br>
1부터 45까지의 무작위 숫자를 생성해서(Math.floor(Math.random() * 45) + 1;), 변수 number에 저장(let number = Math.floor(Math.random() * 45) + 1;).<br>
근데, 여기서 우리가 로또는 중복된 번호가 없잖아? 그래서 if문을 사용해서 중복을 제거하는거지.<br>

```javascript
return lottoNumbers.sort((a, b) => a - b);
```
마지막이야 ! 위 코드를 통해, 배열에 저장된 로또 번호들을 오름차순으로 정렬하는거지 !

그럼 로또 번호 추첨기 prototype 완성 !



