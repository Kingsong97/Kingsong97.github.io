---
layout: post
title: TypeScript) 일반함수? 화살표함수?
date: 2024-05-20 09:59 +0900
description: 
image: ../assets/img/jsb.jpg
category: javascript
tags: javascript 
published: true
sitemap: true
---

# 화살표 함수와 일반 함수의 차이
> 자바스크립트에서 함수를 정의하는 방법은 여러 가지가 있습니다. 그 중에서도 화살표 함수(Arrow Functions)와 일반 함수(Function Expressions) 또는 함수 선언(Function Declarations)은 자주 사용되는 두 가지 방식입니다. 이 두 방식의 차이점을 이해하는 것은 자바스크립트의 중요한 개념들을 이해하는 데 매우 유용합니다.

## 1. 문법 (Syntax)

### 일반 함수(Function Expressions)

````javascript
function regularFunction(a, b) {
    return a + b;
}
````

### 화살표 함수 (Arrow Functions)

````javascript
const arrowFunction = (a, b) => a + b;

````

차이점 : 화살표 함수는 더 간결한 문법을 사용합니다. 중괄호 {}와 return 키워드가 생략될 수 있으며, 한 줄로 표현할 수 있는 간단한 함수에 특히 유용합니다. <br>

## 2. 'this' 바인딩 (Binding of this)

### 일반 함수 
> 일반 함수는 호출된 방식에 따라 this의 값이 결정됩니다. 이는 일반 함수가 다양한 컨텍스트에서 호출될 때 this가 동적으로 바인딩된다는 의미입니다.

````javascript
const obj = {
    value: 10,
    regularFunction: function() {
        console.log(this.value);
    }
};
obj.regularFunction(); // 출력: 10

const detachedFunction = obj.regularFunction;
detachedFunction(); // 출력: undefined (또는 글로벌 객체의 값)

````

### 화살표 함수
> 화살표 함수는 자신만의 this 바인딩을 가지지 않고, 정의된 위치에서 상위 스코프의 this를 상속받습니다.

````javascript
const obj = {
    value: 10,
    arrowFunction: () => {
        console.log(this.value);
    }
};
obj.arrowFunction(); // 출력: undefined (상위 스코프의 this를 참조)

const obj2 = {
    value: 10,
    arrowFunction: function() {
        const innerArrowFunction = () => {
            console.log(this.value);
        };
        innerArrowFunction();
    }
};
obj2.arrowFunction(); // 출력: 10

````

## 3. arguments 객체

### 일반함수
> 일반 함수는 함수 내에서 암묵적으로 arguments 객체를 사용할 수 있습니다. 이는 함수로 전달된 모든 인자를 배열 형태로 접근할 수 있게 합니다

````javascript
function regularFunction() {
    console.log(arguments);
}
regularFunction(1, 2, 3); // 출력: [1, 2, 3]

````

### 화살표 함수
> 화살표 함수는 자체적인 arguments 객체를 가지지 않습니다. 대신 상위 스코프의 arguments 객체를 참조합니다.

````javascript
const arrowFunction = () => {
    console.log(arguments); // ReferenceError: arguments is not defined
};
arrowFunction(1, 2, 3);

function outerFunction() {
    const arrowFunction = () => {
        console.log(arguments);
    };
    arrowFunction(1, 2, 3);
}
outerFunction(1, 2, 3); // 출력: [1, 2, 3]

````

## 4. 생성자 함수 

### 일반함수 
> 일반 함수는 new 키워드와 함께 호출될 때 생성자 함수로서 객체 인스턴스를 생성할 수 있습니다.

````javascript
function Person(name) {
    this.name = name;
}
const person = new Person('Alice');
console.log(person.name); // 출력: Alice

````

### 화살표 함수
> 화살표 함수는 생성자 함수로 사용할 수 없습니다. new 키워드를 사용하여 호출할 경우 에러가 발생합니다

````javascript
const Person = (name) => {
    this.name = name;
};
const person = new Person('Alice'); // TypeError: Person is not a constructor

````

## 5. 메서드 정의

### 일반함수
> 객체의 메서드를 정의할 때 주로 일반 함수를 사용합니다. 일반 함수는 메서드로 호출될 때 해당 객체를 this로 바인딩합니다.

````javascript
const obj = {
    value: 10,
    regularFunction: function() {
        console.log(this.value);
    }
};
obj.regularFunction(); // 출력: 10

````

### 화살표 함수
> 화살표 함수는 메서드로 정의하는 것이 권장되지 않습니다. 화살표 함수는 this 바인딩을 상위 스코프에서 상속받기 때문에, 객체 메서드로 사용 시 의도한 동작을 하지 않을 수 있습니다.

````javascript
const obj = {
    value: 10,
    arrowFunction: () => {
        console.log(this.value);
    }
};
obj.arrowFunction(); // 출력: undefined

````

# 요약
>화살표 함수와 일반 함수는 각기 다른 특성과 용도를 가지며, 상황에 맞게 적절히 선택하여 사용해야 합니다. 화살표 함수는 간결한 문법과 상위 스코프의 this 바인딩을 필요로 할 때 유용합니다. 반면, 일반 함수는 다양한 컨텍스트에서 this 바인딩과 arguments 객체를 필요로 할 때, 그리고 생성자 함수로 사용할 때 적합합니다.