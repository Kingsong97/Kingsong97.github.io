---
layout: post
title: javascript) 싱글 스레드
date: 2024-05-17 09:59 +0900
description: 
image: ../assets/img/jsb.jpg
category: javascript
tags: javascript Single_thread
published: true
sitemap: true
---

# 싱글 스레드란?
> 싱글 스레드(Single Thread) 는 한 번에 하나의 명령어만 처리할 수 있는 실행 환경을 의미합니다. 자바스크립트는 대표적인 싱글 스레드 프로그래밍 언어로, 모든 명령이 하나의 실행 흐름을 따라 순차적으로 처리됩니다.

## 싱글 스레드의 특징

- 자원의 효율적 사용: 싱글 스레드는 멀티 스레딩 환경에 비해 자원을 적게 사용합니다. 스레드 관리에 필요한 메모리나 컴퓨팅 자원이 덜 필요하므로, 가벼운 애플리케이션에 적합합니다. <br>

- 단순성: 스레드 간의 상호작용을 고려할 필요가 없기 때문에, 상태 관리가 더 간단하고 버그 발생률이 낮습니다. <br>

- 순차 처리: 모든 작업이 순차적으로 처리되기 때문에, 작업 처리의 예측성이 높고 디버깅이 쉽습니다. <br>

## 싱글 스레드의 단점

- 성능 제한: 한 번에 하나의 작업만 처리할 수 있기 때문에, 대규모 처리가 필요한 애플리케이션에서는 성능이 제한됩니다. <br>

- 응답성 문제: 하나의 복잡한 작업이 CPU를 오랫동안 점유할 경우, 애플리케이션의 응답성이 떨어질 수 있습니다. <br>

## 자바스크립트에서의 싱글 스레드 처리 

자바스크립트는 싱글 스레드 제약을 극복하기 위해 비동기 처리 모델을 사용합니다. setTimeout, Promise, async/await 등을 통해 I/O 작업이나 긴 처리 시간이 필요한 작업을 비동기적으로 처리하여, 메인 스레드가 블록되지 않도록 합니다.
<br>
이러한 비동기 처리는 사용자 경험을 크게 향상시키고, 싱글 스레드 환경에서도 높은 성능을 유지할 수 있게 돕습니다. <br>

# 이벤트 루프(Event Loop)
> 자바스크립트의 싱글 스레드 모델에서 비동기 처리의 핵심은 이벤트 루프입니다. 이벤트 루프는 호출 스택(Call Stack), 이벤트 큐(Event Queue), 백그라운드 태스크 등 여러 컴포넌트로 구성되어 있습니다

## 이벤트 루프의 작동 원리 

- 호출 스택(Call Stack): 자바스크립트의 모든 실행 코드는 스택 구조에 따라 처리됩니다. 함수 호출이 발생하면 호출 스택에 쌓이고, 실행이 완료되면 스택에서 제거됩니다. <br>

- 백그라운드 태스크: 비동기 함수(예: setTimeout, AJAX 요청 등)는 호출 스택에서 제거되고 백그라운드에서 실행됩니다. 해당 태스크가 완료되면 완료된 작업의 콜백 함수가 이벤트 큐로 이동합니다.
<br>

- 이벤트 큐(Event Queue): 백그라운드에서 처리된 태스크의 콜백 함수가 대기하는 곳입니다. 호출 스택이 비어 있을 때, 이벤트 루프는 이벤트 큐에서 콜백 함수를 하나씩 호출 스택으로 이동시켜 실행합니다. <br>

## 이벤트 루프의 중요성
>이벤트 루프는 싱글 스레드 제한을 극복하고, 높은 응답성과 비동기 처리를 가능하게 합니다. 사용자 인터랙션, 네트워크 요청, 타이머 등 다양한 비동기 이벤트를 효율적으로 관리하여, 애플리케이션의 성능을 최적화합니다.

## 비동처리 예제 

````javascript
console.log('첫 번째 메시지 출력');

setTimeout(function() {
    console.log('두 번째 메시지 출력 (비동기적)');
}, 2000);  // 2초 후에 실행

console.log('세 번째 메시지 출력');

// 실행결과
// 첫 번째 메시지 출력
// 세 번째 메시지 출력
// (2초 후)
// 두 번째 메시지 출력 (비동기적)

````
이 예제에서 setTimeout 함수는 2초 후에 콜백 함수를 실행하도록 예약합니다. 이 때, setTimeout은 비동기적으로 작동하기 때문에 즉시 호출 스택에서 제거됩니다. 그 결과, console.log('세 번째 메시지 출력'); 이 바로 실행되고, 2초 후에 setTimeout의 콜백 함수가 실행되어 '두 번째 메시지 출력 (비동기적)'이 출력됩니다. <br>


## 자바스크립트의 비동기 처리 이해하기 
>자바스크립트의 싱글 스레드 처리 모델은 비동기적인 이벤트 처리를 통해 성능과 사용성을 극대화합니다. 이벤트 루프 외에도, 자바스크립트는 Promise와 async/await 구문을 제공하여 더욱 효율적으로 비동기 작업을 수행할 수 있습니다.

**Promise** 사용 예시

````javascript
function fetchData() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve("데이터 가져오기 성공");
        }, 2000);
    });
}

fetchData().then(data => {
    console.log(data);  // "데이터 가져오기 성공"
}).catch(error => {
    console.error(error);
});

````

- async/await 사용 예:
> async와 await를 사용하면 비동기 코드를 동기 코드처럼 간결하고 이해하기 쉽게 작성할 수 있습니다. Promise를 보다 직관적으로 사용할 수 있습니다.

````javascript
async function fetchData() {
    try {
        const response = await new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve("데이터 처리 완료");
            }, 2000);
        });
        console.log(response);  // "데이터 처리 완료"
    } catch (error) {
        console.error(error);
    }
}

fetchData();

````
실무적인 예제 : 비동기 API 요청

````javascript
async function getUserData(userId) {
    try {
        const response = await fetch(`https://api.example.com/users/${userId}`);
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error("API 요청 중 에러 발생:", error);
    }
}

getUserData(1);

````

## 이벤트 루프와 비동기 처리의 실용성

이벤트 루프를 이해하는 것은 자바스크립트 애플리케이션의 성능을 최적화하고, 사용자 경험을 향상시키는 데 중요합니다. 비동기 처리는 사용자가 복잡한 데이터 처리나 파일 입출력 작업을 요청했을 때 UI가 멈추지 않도록 보장합니다. 이는 모던 웹 애플리케이션에서 필수적인 요소입니다.