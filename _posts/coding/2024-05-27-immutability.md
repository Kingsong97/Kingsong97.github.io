---
layout: post
title: Javascript) 데이터형 변환과 불변성 유지
date: 2024-05-27 01:39 +0900
description: 
image: ../assets/img/jsb.jpg
category: Javascript
tags: Javascript 
published: true
sitemap: true
---

# Javascript - 데이터형 변환과 불변성
--- 

자바스크립트는 매우 유연한 언어로, 데이터 형 변환과 불변성을 이해하고 사용하는 것이 중요합니다. 이번 글에서는 자바스크립트에서의 데이터 형 변환과 불변성을 유지하는 방법에 대해 쉽게 설명하겠습니다.   
   
## 데이터 형 변환   
   
자바스크립트에서는 함수와 연산자에 전달되는 값이 대부분 적절한 자료형으로 자동 변환됩니다. 이를 "형 변환"이라고 합니다.    
형 변환에는 두 가지 종류가 있습니다: 명시적 형 변환과 암시적 형 변환입니다.   

<br>

### 암시적 형 변환 (Implicit Type Conversion)
--- 
암시적 형 변환은 자바스크립트가 자동으로 값을 변환하는 것입니다. 예를 들어, 숫자와 문자열을 더할 때 자바스크립트는 숫자를 문자열로 변환합니다.

```javascript
let result = 1 + '2';
console.log(result); // "12"
```

위 예제에서 자바스크립트는 숫자 1을 문자열 '1'로 변환하여 '2'와 결합합니다. 이 결과는 문자열 "12"가 됩니다.

### 명시적 형 변환 (Explicit Type Conversion)
--- 
명시적 형 변환은 우리가 직접 형 변환을 하는 것입니다. 이를 위해 자바스크립트는 여러 가지 방법을 제공합니다.   
   
- 숫자를 문자열로 변환하기:   

```javascript
let num = 123;
let str = String(num);
console.log(str); // "123"
console.log(typeof str); // "string"
```   

- 문자열을 숫자로 변환하기:   

```javascript   
let str = "123";
let num = Number(str);
console.log(num); // 123
console.log(typeof num); // "number"   
```   

- 불리언을 문자열로 변환하기:   

```javascript   
let bool = true;
let str = String(bool);
console.log(str); // "true"
console.log(typeof str); // "string"   
```

이처럼 명시적 형 변환을 사용하면 데이터의 형을 명확하게 변환할 수 있습니다.   

## 불변성 유지하기   

프로그래밍에서 불변성(Immutability)이란 데이터의 원본을 변경하지 않고 유지하는 것을 의미합니다.     
불변성을 유지하면 예기치 않은 데이터 변경으로 인한 버그를 줄이고, 코드의 신뢰성과 예측 가능성을 높일 수 있습니다.   

### 이름 불변하게 하기   

변수 이름을 불변하게 만들기 위해서는 `var` 대신 `const`를 사용해야 합니다.     
`const`로 선언한 변수는 재할당할 수 없기 때문에, 변수의 이름을 변경할 수 없습니다.    

```javascript   
const name = "John";
// name = "Doe"; // TypeError: Assignment to constant variable.
```   

위 예제에서 `name` 변수는 `const`로 선언되었기 때문에 재할당이 불가능합니다.   

### 값을 불변하게 하기   

원시형 타입(숫자, 문자열, 불리언 등)은 그 값 자체가 불변합니다.    
예를 들어, 숫자나 문자열을 변경할 수 없습니다.   

```javascript   
const number = 10;
// number = 20; // TypeError: Assignment to constant variable.
```   

객체는 조금 다릅니다. `const`로 선언된 객체도 속성 값은 변경할 수 있기 때문에, 완전한 불변성을 유지하지 못합니다.   

```javascript   
const person = { name: "John", age: 30 };
person.age = 31; // 가능
console.log(person.age); // 31
```   
   
### 객체를 불변하게 만드는 방법    
    
#### 1. `Object.assign`      

`Object.assign` 메서드는 객체를 복사하여 새로운 객체를 생성할 수 있습니다. 원본 객체는 변경되지 않고, 새로운 객체에 변경 사항을 적용할 수 있습니다.    

```javascript    
const person = { name: "John", age: 30 };
const updatedPerson = Object.assign({}, person, { age: 31 });
console.log(person.age); // 30
console.log(updatedPerson.age); // 31
```    
    
#### 2. `Object.freeze`    
    
`Object.freeze` 메서드는 객체를 동결시켜 속성 값을 변경할 수 없도록 만듭니다.    
    
```javascript    
const person = Object.freeze({ name: "John", age: 30 });
person.age = 31; // 무시됨
console.log(person.age); // 30
```     

동결된 객체는 속성을 변경할 수 없으며, 변경을 시도해도 에러는 발생하지 않지만 변경 사항은 무시됩니다.     

#### 3. 중첩 객체 불변성 유지하기    

객체 내에 중첩된 객체들도 불변성을 유지하려면, 재귀적으로 `Object.freeze`를 적용해야 합니다.    

```javascript    
function deepFreeze(obj) {
    Object.keys(obj).forEach(name => {
        let prop = obj[name];
        if (typeof prop == 'object' && prop !== null) {
            deepFreeze(prop);
        }
    });
    return Object.freeze(obj);
}

const person = deepFreeze({ name: "John", address: { city: "New York" } });
person.address.city = "Los Angeles"; // 무시됨
console.log(person.address.city); // "New York"    
```

이 함수는 객체의 모든 속성에 대해 재귀적으로 `Object.freeze`를 적용하여 완전한 불변성을 유지합니다.     

#### 4. 불변 함수 사용하기    

불변성을 유지하는 또 다른 방법은 불변 함수를 사용하는 것입니다.     
불변 함수는 원본 데이터를 변경하지 않고, 새로운 데이터를 반환합니다.    

```javascript     
const person = { name: "John", age: 30 };

function updateAge(person, newAge) {
    return { ...person, age: newAge };
}

const updatedPerson = updateAge(person, 31);
console.log(person.age); // 30
console.log(updatedPerson.age); // 31
```

여기서는 스프레드 연산자 `...`를 사용하여 객체를 복사하고, 새로운 속성을 추가하여 새로운 객체를 반환합니다.
    
## 결론

자바스크립트에서 데이터 형 변환과 불변성을 이해하는 것은 매우 중요합니다.    
데이터 형 변환은 자바스크립트가 자동으로 처리하는 경우가 많지만, 명시적 형 변환을 사용하면 더욱 명확하게 코드를 작성할 수 있습니다.     
불변성을 유지하면 코드의 예측 가능성과 신뢰성을 높일 수 있습니다.   
 `const`를 사용하여 변수 이름을 불변하게 만들고, `Object.assign`, `Object.freeze`, 재귀적 동결, 불변 함수를 사용하여 객체의 불변성을 유지할 수 있습니다.    